## ✅ Phase 1: Infrastructure Setup with Terraform — EC2 Bot Environment

In this phase, I automated the provisioning of an EC2 instance with a custom IAM role using Terraform. This sets the foundation for the AWS bot script that will run on the instance.

---

### 🔹 Step 1: Create Terraform Working Directory & Files

I created a new directory and added 4 files:

- `main.tf`
- `variables.tf`
- `terraform.tfvars`
- `outputs.tf`

📸 `screenshots/phase1/01-create-files.png`
<img width="1278" height="798" alt="VirtualBox_Kali Linux_18_07_2025_23_19_58" src="https://github.com/user-attachments/assets/b9ecf4b4-f11f-4e9e-a62f-b340340b5e43" />

---

### 🔹 Step 2: Define AWS Resources in `main.tf`

This file sets the provider, EC2 instance, IAM role, and instance profile.

📸 `screenshots/phase1/02-main-tf.png`
<img width="1278" height="798" alt="image" src="https://github.com/user-attachments/assets/9bcd6bb0-1a0a-4f7a-b272-c6fcac7af533" />

---

### 🔹 Step 3: Declare Variables in `variables.tf`

I used three variables to make the config flexible:

- `aws_region`
- `ami_id`
- `key_name`

📸 `screenshots/phase1/03-variables-tf.png`
<img width="1278" height="798" alt="image" src="https://github.com/user-attachments/assets/87d6a886-1099-4988-88b9-31da35ed85c8" />

---

### 🔹 Step 4: Populate Values in `terraform.tfvars`

This file holds actual values for the declared variables:

- `aws_region = "us-east-1"`
- `ami_id = "ami-020cba7c55df1f615"`
- `key_name = "coded key"`

📸 `screenshots/phase1/04-tfvars.png`
<img width="1278" height="798" alt="image" src="https://github.com/user-attachments/assets/3e1fd909-bb91-461f-9f29-640b379b74b3" />

---

### 🔹 Step 5: Add Output in `outputs.tf`

I configured it to display the instance ID after provisioning:

- `output "instance_id" { value = aws_instance.bot3_ec2.id }`

📸 `screenshots/phase1/05-outputs-tf.png`
<img width="1278" height="798" alt="image" src="https://github.com/user-attachments/assets/078e6f63-46c1-44e4-a884-f89ee1c7c2ac" />

---

### 🔹 Step 6: Initialize, Plan, and Apply with Terraform

Here’s the order I followed in terminal:

- `terraform init`
- `terraform plan`
- `terraform apply` → (confirm with `yes` when prompted)

📸 `screenshots/phase1/06-terraform-apply.png`
<img width="1278" height="798" alt="VirtualBox_Kali Linux_19_07_2025_01_11_35" src="https://github.com/user-attachments/assets/d254e01f-f36a-4237-9c94-b2846abf4ce5" />

---

### ✅ Final Outcome for Phase 1

- EC2 instance with IAM role successfully created  
- All Terraform configs organized into 4 separate files  
- Verified instance and role in AWS Console  
<img width="1920" height="1010" alt="image" src="https://github.com/user-attachments/assets/9cb4f26e-e04d-4f59-8af4-7cf36e536eb4" />


---
## ✅ Phase 2: AWS Bot Simulation — EC2 Script Execution and IAM Role Validation

In this phase, I connected to the EC2 instance provisioned in Phase 1 and executed a Python script that used the assigned IAM role credentials to interact with AWS services. The goal was to simulate how attackers might use EC2 instances with scoped permissions.

---

### 🔹 Step 1: SSH into the EC2 Instance

Using the private key file associated with the key pair in Phase 1 (`coded-key.pem`), I connected to the instance:

```bash
chmod 400 coded-key.pem
ssh -i coded-key.pem ubuntu@44.204.16.40
```

📸 `screenshots/phase2/01-ssh-ec2.png`
<img width="1278" height="798" alt="image" src="https://github.com/user-attachments/assets/5fc3fede-f72e-4303-968c-e86de0d513f9" />

---

### 🔹 Step 2: Check IAM Role from Within EC2

Once connected, I verified that the EC2 instance had an IAM role attached by using the Instance Metadata Service (IMDSv2):

```bash
TOKEN=$(curl -X PUT "http://169.254.169.254/latest/api/token" -H "X-aws-ec2-metadata-token-ttl-seconds: 21600")
curl -H "X-aws-ec2-metadata-token: $TOKEN" http://169.254.169.254/latest/meta-data/iam/info
```

📸 `screenshots/phase2/02-iam-info.png`
<img width="1278" height="798" alt="VirtualBox_Kali Linux_19_07_2025_02_36_11" src="https://github.com/user-attachments/assets/5c993ec0-3374-4187-9279-a657e9ba3101" />

---

### 🔹 Step 3: Write and Run the Python Bot Script

I created a simple Python script called `bot.py` that uses the `boto3` library to list all S3 buckets available to the EC2 instance:

```python
import boto3

s3 = boto3.client('s3')
response = s3.list_buckets()

print("✅ Accessible S3 Buckets:")
for bucket in response['Buckets']:
    print(f" - {bucket['Name']}")
```

📸 `screenshots/phase2/03-bot-script.png`
<img width="1278" height="798" alt="VirtualBox_Kali Linux_19_07_2025_03_03_08" src="https://github.com/user-attachments/assets/f83462a1-37ff-47c0-8a51-4c6c8f67e7da" />

---

### 🔹 Step 4: Run the Script and View Output

I executed the script using Python 3:

```bash
python3 bot.py
```

The script successfully listed all accessible S3 buckets, confirming that the EC2 instance could interact with AWS services through its IAM role.

📸 `screenshots/phase2/04-script-output.png`
<img width="1278" height="798" alt="VirtualBox_Kali Linux_19_07_2025_03_22_14" src="https://github.com/user-attachments/assets/dded62c4-e9d5-4459-a4fe-a17d701b54ef" />

---

### ✅ Final Outcome for Phase 2

- Connected to EC2 using SSH
- Verified attached IAM role via metadata service
- Created and ran a Python bot script with Boto3
- Successfully retrieved a list of accessible S3 buckets


## 🚨 Phase 3: AWS Real-Time S3 Breach Detection & Alerting System (CloudTrail + CloudWatch + SNS)

In this phase, I implemented a real-time alerting mechanism to detect suspicious S3 activities such as **PutObjectAcl**, **GetObject**, or **ListBucket** actions. These alerts are triggered by AWS **CloudTrail** logs and forwarded to **CloudWatch**, which then pushes notifications to **SNS (Simple Notification Service)**, ultimately sending me an **email alert**.

---

### 🔹 Step 1: Create Terraform Files for SNS Setup

To manage the SNS topic and email subscription, I created two new Terraform files:

- `sns.tf`: Defines the SNS topic resource and the email subscription

- `sns-variables.tf`: Declares variables for the SNS configuration, including the notification email address

This separation keeps the SNS configuration modular and easy to maintain.

---

### 🔹 Step 2: Define SNS Topic and Email Subscription in `sns.tf`

Inside `sns.tf`, I defined the SNS topic resource:

```hcl
resource "aws_sns_topic" "alerts_topic" {
  name = "s3-security-alerts"
}
```
Then, I created an SNS email subscription resource linked to the topic and the notification email variable:

```hcl
resource "aws_sns_topic_subscription" "email_subscription" {
  topic_arn = aws_sns_topic.alerts_topic.arn
  protocol  = "email"
  endpoint  = var.notification_email
}
```
📸 `screenshots/phase3/01-create-sns-files.png`

<img width="1278" height="798" alt="VirtualBox_Kali Linux_19_07_2025_04_20_34" src="https://github.com/user-attachments/assets/62180bc1-7ad9-4563-9e6e-5548dde9536f" />

This setup ensures that any message published to the SNS topic triggers an email to the subscribed address.

---

### 🔹 Step 3: Declare Email Variable in `sns-variables.tf`

I added this variable declaration for the notification email address:

```hcl
variable "notification_email" {
  description = "Email address to receive SNS alerts"
  type        = string
}
```
<img width="1278" height="798" alt="image" src="https://github.com/user-attachments/assets/0ede792f-a78f-483e-b075-deea1d3cdf51" />

This makes it easy to change the recipient without touching the core SNS config.

---

### 🔹 Step 4: Populate Email Variable in `terraform.tfvars`

In the main variables file, I added my email:

```hcl
notification_email = "johnsonmatthewayobami@gmail.com"
```
<img width="1278" height="798" alt="image" src="https://github.com/user-attachments/assets/2f0bee1b-d979-483f-bd21-ea70f80862ea" />


After applying Terraform, AWS sends a confirmation email to this address to verify the subscription.
<img width="960" height="505" alt="Screenshot 2025-07-19 043154" src="https://github.com/user-attachments/assets/02067a04-5450-451d-8852-4305eaf80303" />

---

### 🔹 Step 5: Apply Terraform to Provision SNS Resources

I ran the standard Terraform commands to deploy the SNS resources:

```bash
terraform init
terraform plan
terraform apply
```

I typed `yes` to confirm the apply. Terraform created the SNS topic and email subscription in my AWS account.
<img width="1278" height="798" alt="VirtualBox_Kali Linux_19_07_2025_04_31_08" src="https://github.com/user-attachments/assets/bd73ce66-0268-4666-a117-522a3f207268" />

After this, I checked my email and clicked the confirmation link to activate the subscription.
<img width="960" height="505" alt="Screenshot 2025-07-19 043338" src="https://github.com/user-attachments/assets/47267331-0ee7-4c40-9d1a-347919f2e7a4" />

---

### 🔹 Step 6: Update IAM Role to Allow SNS Publishing

To let the EC2 instance publish alerts, I updated the IAM role policy attached to the instance with SNS permissions.

I added this policy statement:

```json
{
  "Effect": "Allow",
  "Action": "sns:Publish",
  "Resource": "arn:aws:sns:us-east-1:123456789012:s3-security-alerts"
}
```
<img width="1278" height="798" alt="image" src="https://github.com/user-attachments/assets/1329a967-b0ec-407b-ae7e-dd57a6e7b7f7" />

This lets the Python bot publish messages to the SNS topic securely.

---

### 🔹 Step 7: Modify Python Bot to Send Alerts to SNS

In `bot.py`, I imported Boto3 SNS client and added a function to publish alert messages to the topic:

```python
import boto3

sns_client = boto3.client('sns')
topic_arn = 'arn:aws:sns:us-east-1:123456789012:s3-security-alerts'

def send_alert(message):
    response = sns_client.publish(
        TopicArn=topic_arn,
        Message=message,
        Subject='AWS S3 Security Alert'
    )
    print(f"Alert sent! Message ID: {response['MessageId']}")
```
<img width="1116" height="798" alt="VirtualBox_Kali Linux_19_07_2025_06_26_54" src="https://github.com/user-attachments/assets/d55b54c0-b27e-4ea1-84f9-e2784cc56293" />


Wherever the bot detects a public bucket or risky policy, it calls `send_alert()` with details.

---

### 🔹 Step 8: Test Alert Workflow End-to-End

I ran the Python bot and simulated a detection of a public S3 bucket. Immediately, I received an email alert from AWS SNS with the alert message.

This confirmed the entire alerting pipeline works: detection → SNS publish → email notification.
<img width="1920" height="1010" alt="image" src="https://github.com/user-attachments/assets/531afab1-09bb-4554-951b-fb0ab4a5c959" />

---

### ✅ Final Outcome for Phase 3

- Terraform automated provisioning of SNS topic and email subscription

- Verified email subscription activation

- Updated IAM role to allow SNS publishing

- Enhanced Python bot to send real-time SNS alerts

- Tested and confirmed instant email notifications upon detection

- This phase sets the stage for future auto-remediation capabilities

---
---

## ✅ Key Learnings & Reflection

- Learned how to automate AWS infra with Terraform (IAM, EC2, SNS)
- Practiced EC2 instance role delegation with metadata inspection
- Used CloudTrail + SNS for real-time S3 breach detection
- Integrated Boto3 for secure AWS scripting inside EC2
- Gained confidence debugging permissions and service connectivity in AWS

---
---

## 🧠 Final Thoughts

This lab taught me a lot about AWS cloud security—especially how exposed metadata credentials and public S3 buckets can be abused by attackers. Building the infrastructure with Terraform, simulating the attack from an EC2 instance, and sending real-time alerts with Python and SNS gave me a hands-on understanding of how to detect and respond to these threats in real environments.

---
