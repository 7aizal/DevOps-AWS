

## 🚀 DevOps Progress: AWS Journey

### 🔐 Identity and Access Management (IAM)

IAM is a fundamental security component in AWS. It enables fine-grained access control across all AWS services.

#### Key Concepts:

- **Users**
  Individuals with AWS credentials (username/password or access keys).

- **Groups**
  Logical collections of users for easier permission management.

- **Policies**
  JSON documents that define what actions are allowed or denied on AWS resources. Attached to users, groups, or roles.

- **Roles**
  Used to grant temporary access to AWS services or users. Commonly used by EC2 instances or Lambda functions.

#### Security Features:

- **MFA (Multi-Factor Authentication)**
  Adds an extra layer of security beyond just a password.

- **Password Policy**
  Enforces password strength, expiration, and reuse limits.

#### Programmatic Access:

- **AWS CLI**
  Command-line tool to manage AWS services.

- **AWS SDK**
  Programming libraries for various languages (e.g., Python, Node.js) to interact with AWS APIs.

- **Access Keys**
  Pair of credentials (Access Key ID and Secret Access Key) for CLI or SDK access.

#### Auditing Tools:

- **IAM Credential Reports**
  Summary of all user credentials in the account (passwords, keys, etc.).

- **IAM Access Advisor**
  Tracks which services users or roles have accessed—useful for least-privilege access control.

---

🧠 _This section is part of my hands-on DevOps learning journey using AWS services. More updates will follow as I explore services like EC2, S3, Lambda, and CloudFormation._

