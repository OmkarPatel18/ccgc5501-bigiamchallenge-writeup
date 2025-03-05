# Bastion Challenge Writeup

## Overview
This writeup documents the steps taken to complete the **Bastion** challenge as part of the **CloudFoxable Lab**. The objective of this challenge was to gain access to an EC2 instance (bastion host) using AWS Systems Manager Session Manager (SSM) and enumerate permissions to find the hidden flag.

## Challenge Setup

### Prerequisites
- AWS CLI installed and configured
- CloudFox installed and set up on Windows
- Terraform installed
- Session Manager Plugin installed

### CloudFoxable Deployment
1. Navigate to the CloudFoxable directory:
   cd C:\Users\91816\cloudfoxable\aws

2. Edit the **terraform.tfvars** file:
   - Set the `bastion_enabled` flag to `true`:
   bastion_enabled = true

3. Deploy the challenge using Terraform:
   terraform apply

## Challenge Walkthrough

### 1. Discover EC2 Instance
Run the following CloudFox command to list all EC2 instances:
cloudfox aws -p cloudfoxable instances -v2
This command provides the **Instance ID** and **Loot file location**.

### 2. Access EC2 Instance via SSM
To connect to the EC2 instance using AWS Systems Manager Session Manager, install the **Session Manager Plugin**:
aws ssm start-session --target i-xx

### 3. Enumerate Permissions
Use CloudFox to list IAM permissions assigned to the instance:
cloudfox aws -p cloudfoxable permissions --principal EC2InstanceRole
Identify permissions that grant access to AWS services.

### 4. Flag Discovery
By reviewing the permissions, the **flag** can be found in the S3 bucket or Secrets Manager.



## Cleanup
After completing the challenge, clean up resources:
1. Edit **terraform.tfvars** to disable the challenge:
   bastion_enabled = false

2. Apply the changes:
   terraform apply


## Reflection
This challenge demonstrated how to gain access to EC2 instances through AWS Session Manager, enumerate permissions using CloudFox, and leverage IAM permissions to discover sensitive information. The key takeaway was understanding how attackers can exploit misconfigured permissions and the importance of following the principle of least privilege.

## Conclusion
The Bastion challenge provided hands-on experience in cloud security enumeration and privilege escalation techniques. This exercise reinforced the importance of securing EC2 instances and properly configuring IAM roles in AWS environments.