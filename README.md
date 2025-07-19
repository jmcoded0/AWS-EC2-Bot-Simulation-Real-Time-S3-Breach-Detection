# ☁️ Terraform & Bot3 AWS Cloud Automation Lab

This lab project is all about combining **Terraform** (for infrastructure as code) and **Bot3**, a custom Python automation bot I built using `boto3`, to create and manage AWS resources more efficiently. It was my first time diving into Terraform, and I used this project to really understand how infrastructure and automation can work together.

---

## 🛠️ Tools & Tech Used

- **Terraform** — for provisioning AWS infrastructure
- **Python (`boto3`)** — for automating AWS resource management
- **AWS IAM & CLI** — for authentication and permissions
- **.env Files** — to securely manage secrets


---

## 🚀 What This Project Does

- Automatically creates an **EC2 instance** and an **S3 bucket** using Terraform
- Uses **Python automation bot** to:
  - Tag resources dynamically
  - Upload files to S3
  - Check and log EC2 instance states
- Separates infrastructure config (`.tf`) from credentials (`.tfvars`)
- Stores logs and keeps everything modular and reusable

---

## 🧠 Key Concepts I Practiced

- Writing modular **Terraform** code with `main.tf`, `variables.tf`, and `terraform.tfvars`
- **Infrastructure as Code (IaC)** approach
- Automating AWS with `boto3` + `.env` configs
- Logging and output management in Python
- IAM role permissions and best practices
- Hands-on debugging of both Terraform and Python API issues

---
