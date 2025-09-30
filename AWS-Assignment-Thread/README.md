# AWS Networking Assignment — Stage 1 + Stage 2

> Region: **eu-west-2 (London)**  
> VPC CIDR: **10.0.0.0/16** (DNS hostnames + resolution ✅)  
> VPC ID: **vpc-0340d04f9cde4eb47**

---

## 📦 Current Inventory

- **Subnets**
  - Public (ALB): `alb-subnet1` (AZ A), `alb-subnet2` (AZ B)
  - Private (Targets): `private-1` (AZ A), `private-2` (AZ B)
- **Route Tables**
  - `public-route`  → `0.0.0.0/0 → igw-…` (associated to: `alb-subnet1`, `alb-subnet2`)
  - `private-route` → `0.0.0.0/0 → nat-…` (associated to: `private-1`, `private-2`)
- **NAT Gateway:** 1× in a **public** subnet 

- **Security Groups (non-default)**
  - **`public-subnet-sg`** — for the *public* instance in Stage 1
  - **`private-subnet-sg`** — for the *private* instance in Stage 1
  - **`securitygroup-alb`** — attach to the **ALB**
  - **`securitygroup-web-backend`** — attach to **both target EC2s**

---

## 🧭 Objective

- **Stage 1:** Build VPC + public/private subnets + IGW + NAT. Launch **public** and **private** EC2s. Prove public HTTP and private egress via NAT.
- **Stage 2:** Internet-facing **ALB** across two public subnets, forwarding to **two EC2 targets** in private subnets (different AZs). Targets accept **only** ALB-originated web traffic.

---

## 🧱 Stage 1 — VPC + Subnets + IGW/NAT + EC2

### 🔧 Build (summary)
- VPC `10.0.0.0/16` with DNS features **enabled**
- **Public subnet A** `10.0.1.0/24` (auto-assign public IP **ON**) → `public-route` → **IGW**
- **Private subnet A** `10.0.2.0/24` → `private-route` → **NAT**
- **IGW** attached, **EIP** allocated, **NAT GW** created **in public subnet**
- **IAM role** `ec2-ssm-instance-role` with `AmazonSSMManagedInstanceCore`

### 🔒 Security Groups (Stage 1)
- **`public-subnet-sg`**  
  - Inbound: **HTTP 80** from `0.0.0.0/0` (optionally **SSH 22** from **my IP /32** if used)  
  - Outbound: **All**
- **`private-subnet-sg`**  
  - Inbound: **None**  
  - Outbound: **All**

### 🖥️ Instances
- **Public instance** (public subnet, has public IPv4, SG = `public-subnet-sg`, IAM = SSM role)  
- **Private instance** (private subnet, **no** public IP, SG = `private-subnet-sg`, IAM = SSM role)

**User data (public instance, Amazon Linux 2023)**
```bash
#!/bin/bash
set -eux
dnf -y install nginx
echo "Public OK: $(hostname -f)" > /usr/share/nginx/html/index.html
systemctl enable --now nginx

