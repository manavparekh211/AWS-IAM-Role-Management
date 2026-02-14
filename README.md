# 👥 Terraform IAM User Management

Terraform-powered AWS IAM automation that provisions users and assigns roles dynamically using a single YAML configuration file — enabling scalable, data-driven identity management as code.

---

## 📋 Table of Contents

- [Overview](#-overview)
- [Architecture](#-architecture)
- [Prerequisites](#-prerequisites)
- [Getting Started](#-getting-started)
- [Project Structure](#-project-structure)
- [Configuration](#-configuration)
- [Usage](#-usage)
- [Cleanup](#-cleanup)
- [Resources](#-resources)

---

## 🌟 Overview

This project uses a **YAML-driven approach** to manage AWS IAM users and roles via Terraform. All user definitions and role assignments live in a single `user-roles.yaml` file, eliminating hardcoded HCL and making it simple to onboard or offboard users at scale.

**Highlights:**
| Feature | Description |
|---------|-------------|
| 📄 YAML Config | Users & roles defined in `user-roles.yaml` |
| 🔐 Dynamic Roles | IAM roles created with AWS managed policies |
| 👤 Auto Provisioning | Users created and linked to roles automatically |
| 🏷️ Modular Layout | Separate files for users, roles & provider config |

---

## 🏗️ Architecture

```
┌──────────────────────────────────────────────────┐
│                    AWS IAM                        │
│                                                   │
│  ┌────────────┐          ┌─────────────────────┐  │
│  │   Users     │          │       Roles         │  │
│  │             │          │                     │  │
│  │  👤 john   ─┼────────▶│  📋 readonly        │  │
│  │            ─┼────────▶│  📋 developer       │  │
│  │  👤 jane   ─┼────────▶│  📋 admin           │  │
│  │            ─┼────────▶│  📋 auditor         │  │
│  │  👤 lauro  ─┼────────▶│  📋 readonly        │  │
│  └────────────┘          └──────────┬──────────┘  │
│                                     │             │
│                          ┌──────────▼──────────┐  │
│                          │  AWS Managed        │  │
│                          │  Policies           │  │
│                          └─────────────────────┘  │
└──────────────────────────────────────────────────┘
```

---

## ✅ Prerequisites

| Requirement | Version |
|-------------|---------|
| [Terraform](https://www.terraform.io/downloads.html) | `>= 1.0` |
| [AWS CLI](https://aws.amazon.com/cli/) | Latest |
| AWS Provider | `~> 5.100.0` |
| AWS Account | With IAM admin permissions |

---

## 🏁 Getting Started

```bash
# 1. Configure AWS credentials
export AWS_ACCESS_KEY_ID="your-access-key"
export AWS_SECRET_ACCESS_KEY="your-secret-key"
export AWS_DEFAULT_REGION="ap-south-1"

# 2. Initialize Terraform
terraform init

# 3. Preview changes
terraform plan

# 4. Deploy
terraform apply
```

---

## 📁 Project Structure

```
iam-user-management/
├── providers.tf            # AWS provider & region configuration
├── users.tf                # IAM user creation & role attachments
├── roles.tf                # IAM role definitions & policy mappings
├── user-roles.yaml         # Single source of truth for users & roles
├── .gitignore              # Excludes state, credentials & .terraform/
├── .terraform.lock.hcl     # Provider version lock file
└── README.md               # ← You are here
```

---

## ⚙️ Configuration

All user and role management is controlled through **`user-roles.yaml`**:

```yaml
users:
  - username: john
    roles:
      - readonly
      - developer
  - username: jane
    roles:
      - admin
      - auditor
  - username: lauro
    roles:
      - readonly
```

### ➕ Add a New User

Append to the `users` list in `user-roles.yaml`:

```yaml
  - username: alex
    roles:
      - developer
```

Then apply:

```bash
terraform plan
terraform apply
```

### ➖ Remove a User

Delete the user entry from `user-roles.yaml` and run:

```bash
terraform apply
```

Terraform will automatically destroy the removed user and their role attachments.

---

## 💡 Usage

| Command | Description |
|---------|-------------|
| `terraform init` | Initialize providers |
| `terraform plan` | Preview infrastructure changes |
| `terraform apply` | Apply changes to AWS |
| `terraform show` | View current state |
| `terraform state list` | List all managed resources |
| `terraform fmt` | Auto-format `.tf` files |
| `terraform validate` | Validate configuration syntax |

---

## 🧹 Cleanup

Destroy all IAM resources created by this project:

```bash
terraform destroy
```

> Type `yes` when prompted to confirm deletion.

---

## 📚 Resources

- 📖 [Terraform Docs](https://www.terraform.io/docs)
- ☁️ [AWS IAM Terraform Resource](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/iam_user)
- 👥 [AWS IAM Best Practices](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html)
- 📄 [Terraform `yamldecode`](https://developer.hashicorp.com/terraform/language/functions/yamldecode)
