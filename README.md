# ☁️ Terraform & Boto3 AWS Cloud Automation Lab

This lab project is all about combining **Terraform** (for infrastructure as code) and **Boto3**, a custom Python automation bot I built using `boto3`, to create and manage AWS resources more efficiently. It was my first time diving into Terraform, and I used this project to really understand how infrastructure and automation can work together.

---

## 🛠️ Tools & Tech Used

- **Terraform** — for provisioning AWS infrastructure
- **Python (`boto3`)** — for automating AWS resource management
- **AWS IAM & CLI** — for authentication and permissions
- **.env Files** — to securely manage secrets

---

## 🚀 What This Project Does

- Automatically creates an **EC2 instance** and an **S3 bucket** using Terraform
- Uses a **Python automation bot** to:
  - Tag resources dynamically
  - Upload files to S3
  - Check and log EC2 instance states
- Separates infrastructure config (`main.tf`, `variables.tf`, `terraform.tfvars`) from credentials
- Logs activity and keeps everything modular and reusable

---

## 🧠 Key Concepts I Practiced

- Writing modular **Terraform** code
- **Infrastructure as Code (IaC)** approach
- Automating AWS with `boto3` + `.env` config
- Logging and resource state checking with Python
- IAM permissions and AWS CLI usage
- Hands-on debugging of both Terraform and Python issues

---

👉 **Full hands-on documentation with screenshots and code:**  
📄 [`documenting.md`](https://github.com/jmcoded0/AWS-S3-Data-Breach-Detection-Response-Lab-Terraform-Boto3-/blob/main/documenting.md)

