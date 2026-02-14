# 🏗️ Terraform AWS Infrastructure Lab

A hands-on collection of **Terraform** projects for provisioning and managing AWS infrastructure — covering IAM user management, networking & compute resources, and S3 static website hosting.

---

## 📋 Table of Contents

- [Overview](#-overview)
- [Projects](#-projects)
  - [IAM User Management](#1--iam-user-management)
  - [AWS Resources (Networking & Compute)](#2--aws-resources-networking--compute)
  - [S3 Static Website](#3--s3-static-website)
- [Architecture](#-architecture)
- [Prerequisites](#-prerequisites)
- [Getting Started](#-getting-started)
- [Project Structure](#-project-structure)
- [Best Practices](#-best-practices)
- [Cleanup](#-cleanup)
- [Tech Stack](#-tech-stack)
- [Resources](#-resources)
- [License](#-license)

---

## 🌟 Overview

This repository is a **Terraform learning lab** organized into independent sub-projects, each demonstrating real-world AWS infrastructure provisioning patterns:

| # | Project | Description |
|---|---------|-------------|
| 1 | **IAM User Management** | YAML-driven IAM users, roles & policy attachments |
| 2 | **Resources** | VPC networking and EC2 compute infrastructure |
| 3 | **S3 Static Website** | Fully deployed static website hosted on S3 |

Each project is self-contained with its own provider configuration, state, and resources.

---

## 🚀 Projects

### 1. 👥 IAM User Management

> **Path:** [`iam-user-management/`](iam-user-management/)

Automates AWS IAM user creation and role assignment using a **YAML-driven** approach.

**Key Features:**
- 📄 User and role definitions via [`user-roles.yaml`](iam-user-management/user-roles.yaml)
- 🔐 Dynamic IAM role creation with AWS managed policies
- 👤 Automated user provisioning with role attachments
- 🏷️ Clean separation: [`users.tf`](iam-user-management/users.tf) · [`roles.tf`](iam-user-management/roles.tf) · [`providers.tf`](iam-user-management/providers.tf)

**Sample YAML Config:**
```yaml
users:
  - username: john
    roles: [readonly, developer]
  - username: jane
    roles: [admin, auditor]
  - username: lauro
    roles: [readonly]
```

---

### 2. 🖧 AWS Resources (Networking & Compute)

> **Path:** [`resources/`](resources/)

Provisions core AWS networking and compute infrastructure.

**Key Features:**
- 🌐 VPC, subnets, and networking setup via [`networking.tf`](resources/networking.tf)
- 💻 EC2 instances and compute resources via [`compute.tf`](resources/compute.tf)
- ⚙️ Provider configuration in [`provider.tf`](resources/provider.tf)

---

### 3. 🌐 S3 Static Website

> **Path:** [`s3-static-website/`](s3-static-website/)

Deploys a production-ready static website on AWS S3 with custom HTML pages.

**Key Features:**
- 🪣 S3 bucket with website hosting configuration via [`s3.tf`](s3-static-website/s3.tf)
- 📤 Output values for easy access via [`outputs.tf`](s3-static-website/outputs.tf)
- 🎨 Beautiful responsive landing page in [`html-files/`](s3-static-website/html-files/)
- 📖 Dedicated README at [`s3-static-website/README.md`](s3-static-website/README.md)

---

## 🏗️ Architecture

```
┌─────────────────────────────────────────────────────────┐
│                     AWS Cloud                           │
│                                                         │
│  ┌──────────────┐  ┌──────────────┐  ┌──────────────┐  │
│  │  IAM Users   │  │     VPC      │  │  S3 Bucket   │  │
│  │  & Roles     │  │  ┌────────┐  │  │  (Website)   │  │
│  │              │  │  │ Subnet │  │  │              │  │
│  │  👤 john     │  │  └────────┘  │  │  index.html  │  │
│  │  👤 jane     │  │  ┌────────┐  │  │  error.html  │  │
│  │  👤 lauro    │  │  │  EC2   │  │  │              │  │
│  │              │  │  └────────┘  │  │       🌐     │  │
│  └──────────────┘  └──────────────┘  └──────────────┘  │
│                                                         │
└─────────────────────────────────────────────────────────┘
        ▲                   ▲                  ▲
        │                   │                  │
   iam-user-mgmt/      resources/      s3-static-website/
```

---

## ✅ Prerequisites

| Requirement | Version |
|-------------|---------|
| [Terraform](https://www.terraform.io/downloads.html) | `>= 1.14.0` |
| [AWS CLI](https://aws.amazon.com/cli/) | Latest |
| AWS Account | With appropriate IAM permissions |

---

## 🏁 Getting Started

1. **Clone the repository:**
   ```bash
   git clone https://github.com/<your-username>/terraform-aws-infrastructure-lab.git
   cd terraform-aws-infrastructure-lab
   ```

2. **Configure AWS credentials:**
   ```bash
   export AWS_ACCESS_KEY_ID="your-access-key"
   export AWS_SECRET_ACCESS_KEY="your-secret-key"
   export AWS_DEFAULT_REGION="ap-south-1"
   ```

3. **Navigate to a project and deploy:**
   ```bash
   cd iam-user-management   # or resources/ or s3-static-website/

   terraform init            # Initialize providers
   terraform plan            # Preview changes
   terraform apply           # Deploy infrastructure
   ```

---

## 📁 Project Structure

```
terraform-aws-infrastructure-lab/
│
├── README.md                        # ← You are here
├── .gitignore                       # Global git ignore rules
├── practice.tf                      # Terraform practice/scratch file
│
├── iam-user-management/             # 👥 IAM Users & Roles
│   ├── providers.tf
│   ├── users.tf
│   ├── roles.tf
│   ├── user-roles.yaml
│   └── .terraform.lock.hcl
│
├── resources/                       # 🖧 Networking & Compute
│   ├── provider.tf
│   ├── networking.tf
│   ├── compute.tf
│   └── .terraform.lock.hcl
│
└── s3-static-website/               # 🌐 S3 Static Website
    ├── providers.tf
    ├── s3.tf
    ├── outputs.tf
    ├── README.md
    ├── .gitignore
    ├── .terraform.lock.hcl
    └── html-files/
        ├── index.html
        └── error.html
```

---

## 🔐 Best Practices

- ✅ **Provider version pinning** with `.terraform.lock.hcl`
- ✅ **Modular project organization** — each concern is isolated
- ✅ **YAML-driven configuration** for IAM (data-driven approach)
- ✅ **Sensitive files excluded** from version control
- ✅ **No hardcoded credentials** — use environment variables or AWS profiles
- ✅ **State file awareness** — never commit `.tfstate` files

> ⚠️ **Security Note:** Never commit `.env` files, `.tfvars`, or `terraform.tfstate` files containing credentials or sensitive data to version control.

---

## 🧹 Cleanup

To destroy all resources in any project:

```bash
cd <project-directory>
terraform destroy
```

---

## 📊 Tech Stack

| Tool | Purpose |
|------|---------|
| **Terraform** | Infrastructure as Code |
| **AWS Provider** `~> 5.0 / 6.x` | Cloud resource provisioning |
| **AWS IAM** | Identity & access management |
| **AWS VPC / EC2** | Networking & compute |
| **AWS S3** | Static website hosting |
| **YAML** | Data-driven configuration |

---

## 📚 Resources

- 📖 [Terraform Documentation](https://www.terraform.io/docs)
- ☁️ [AWS Provider Registry](https://registry.terraform.io/providers/hashicorp/aws/latest/docs)
- 🪣 [AWS S3 Static Hosting Guide](https://docs.aws.amazon.com/AmazonS3/latest/userguide/WebsiteHosting.html)
- 👥 [AWS IAM Best Practices](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html)
- 🏗️ [Terraform Best Practices](https://www.terraform-best-practices.com/)

---

## 📄 License

This project is open source and available under the **MIT License**.

---

<p align="center">
  Made with ❤️ using <strong>Terraform</strong> & <strong>AWS</strong>
</p>
