# Terraform & Bot3 AWS Cloud Automation Lab

This lab project demonstrates how I combined Terraform (for infrastructure-as-code) and Bot3 (a custom Python-based automation bot) to deploy and manage AWS cloud resources efficiently. This is my first time working with Terraform and automation bots, and I documented every step as I learned it hands-on.

## 🔧 Tools Used:
- **Terraform**: For provisioning AWS infrastructure
- **AWS CLI & IAM**: To configure and authenticate access
- **Python + Bot3 Script**: For automated resource creation and tagging
- **GitHub**: Version control & documentation

## 🧠 What I Built
- A Terraform script that spins up an AWS EC2 instance and S3 bucket with correct IAM permissions
- A Bot3 automation bot that interacts with AWS services (via `boto3`), reads config from `.env`, and creates tags, uploads files, and checks EC2 states
- A `.tfvars`-based setup for reusability and separation of credentials
- Logs and output are stored and managed for visibility

## 🔍 Key Concepts I Practiced
- Infrastructure as Code (IaC) with Terraform
- Using `.tf`, `.tfvars`, and state management
- IAM Roles and policy attachment
- `boto3` Python scripting to automate AWS tasks
- Reading from `.env` and securely managing secrets
- Project structuring and cleanup

## 📁 Project Structure

```
terraform-bot3-lab/
├── main.tf              # Terraform main configuration
├── variables.tf         # Terraform variables definition
├── terraform.tfvars     # Actual variable values for deployment

├── bot3/
│   ├── automation.py    # Python script using boto3 to automate AWS tasks
│   ├── config.env       # Environment variables for boto3 authentication
│   └── logs/            # Folder where script logs are stored

└── README.md            # Project documentation
```
