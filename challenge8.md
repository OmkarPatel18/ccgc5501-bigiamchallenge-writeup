# CloudFoxable Challenge: "It's a Secret"

# Overview
This writeup documents the steps taken to complete the "It's a Secret" challenge as part of the CloudFoxable Lab. The objective of this challenge was to:

Use CloudFox to enumerate AWS resources and permissions.
Identify and retrieve a hidden secret stored in AWS Secrets Manager.
Demonstrate AWS enumeration and privilege escalation techniques.

## Challenge Setup

###  Prerequisites
Before starting the challenge, the following tools and configurations were required:

 AWS CLI installed and configured with the cloudfoxable profile.
 CloudFox installed and properly set up.
 Go environment installed and configured (for building CloudFox).
 IAM credentials with access to AWS Secrets Manager.

### CloudFoxable Deployment
Navigate to the CloudFoxable Directory I navigated to the CloudFoxable directory where the lab environment was deployed:
cd ~/cloudfoxable/aws

Deploy the Challenge The challenge was already deployed, so no additional Terraform deployment was needed. However, if redeployment was necessary, the following commands would be used:
terraform apply
This would provision the necessary AWS resources, including IAM policies and secrets.

## Challenge Walkthrough
 1. Verify AWS Profile and Permissions
First, I verified that I was using the correct AWS profile by running:
aws --profile cloudfoxable sts get-caller-identity
This confirmed that I was authenticated as the ctf-starting-user with the necessary policies attached:
SecurityAudit (AWS Managed)
CloudFox (Customer Managed)
its-a-secret (Customer Managed)

 2. Run CloudFox Enumeration
To identify AWS secrets, I ran CloudFox with the cloudfoxable profile:
cloudfox --profile cloudfoxable
CloudFox automatically enumerated AWS resources, including:
Secrets Manager
IAM permissions
S3 buckets
EC2 instances

 3. Identify and Retrieve the Secret
After the CloudFox enumeration, I discovered a secret named its-a-secret.
To access the secret, I ran the following command:
aws --profile cloudfoxable secretsmanager get-secret-value --secret-id its-a-secret
This returned the flag stored inside the secret.

 4. Cleanup
Since this challenge did not require any additional deployment, no explicit cleanup steps were necessary.
However, if I needed to destroy the lab resources, I could run:
terraform destroy
This would remove all deployed resources.

## Reflection

### What was my approach?
My approach involved using CloudFox to enumerate AWS resources and identify misconfigured IAM permissions.
I leveraged AWS Secrets Manager and CloudFox to extract the flag.

### What was the biggest challenge?
The biggest challenge was dealing with disk corruption and a read-only filesystem that prevented CloudFox from running properly.
This required troubleshooting, disk repair, and reconfiguring the Go environment.

### How did I overcome the challenges?
I booted into a live USB environment.
Ran fsck on the root disk to repair corruption:
sudo fsck -y /dev/sdc
Rebooted the system and restored read-write access.
Reinstalled CloudFox properly using:
go install github.com/BishopFox/cloudfox/v3@latest

### What led to the breakthrough?
The breakthrough came after successfully repairing the corrupted disk, which allowed CloudFox to run correctly.
Enumerating AWS Secrets Manager with CloudFox revealed the hidden secret and the flag.

### Blue Team Perspective: Defensive Measures
From a defensive security standpoint, the following practices would help protect against this type of enumeration:
Principle of Least Privilege (PoLP): The challenge used a minimal-privilege IAM policy, which is a good practice.
AWS CloudTrail Logging: Enable detailed logging to detect and alert on suspicious AWS Secrets Manager access attempts.
Secret Rotation: Regularly rotate secrets to limit exposure time.
Resource Scoping: Use specific IAM policies with restricted ARNs to prevent over-permissioning.

## Conclusion
The "It's a Secret" challenge provided hands-on experience with:
AWS enumeration techniques using CloudFox.
IAM permission analysis.
Retrieving secrets from AWS Secrets Manager.
This challenge highlighted the importance of proper IAM policy management, secret protection, and the Principle of Least Privilege.
It also emphasized the significance of troubleshooting skills in cloud security labs when facing issues like disk corruption.

## Commands Used
### Verify AWS profile identity
aws --profile cloudfoxable sts get-caller-identity  

### Install CloudFox (after disk repair)
go install github.com/BishopFox/cloudfox/v3@latest  

### Verify CloudFox installation
cloudfox --version  

### Run CloudFox with the cloudfoxable profile
cloudfox --profile cloudfoxable  

### Retrieve the secret using AWS CLI
aws --profile cloudfoxable secretsmanager get-secret-value --secret-id its-a-secret