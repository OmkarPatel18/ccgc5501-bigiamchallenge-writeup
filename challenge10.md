# CloudFoxable Challenge: "Backwards"

## Overview
This write-up documents the steps taken to complete the "Backwards" challenge as part of the CloudFoxable Lab. The objective of this challenge was to:

Identify an IAM role or user with access to a specific AWS Secrets Manager secret (DomainAdministrator-Credentials).
Analyze AWS IAM policies and trust relationships to determine if privilege escalation was possible.
Assume an IAM role with sufficient permissions to retrieve the secret.
Understand how role-based access control (RBAC) and trust policies impact cloud security.

## Challenge Setup

### Prerequisites
Before starting the challenge, the following tools and configurations were required:

AWS CLI installed and configured with the cloudfoxable profile.
CloudFox installed and properly set up for enumeration.
IAM credentials that allow running enumeration commands.

### CloudFoxable Deployment
Since the challenge environment was already deployed, no additional provisioning was necessary.

## Challenge Walkthrough

### Step 1: Identifying IAM Roles with Secret Access
The first step was to determine who had access to the DomainAdministrator-Credentials secret.

I ran the following command to list permissions related to AWS Secrets Manager:
cloudfox aws -p cloudfoxable permissions -v2 | grep -i secret

Findings
From the output, I identified an IAM role with secretsmanager:GetSecretValue permissions. This meant that this role could retrieve secret values, including the DomainAdministrator-Credentials.

### Step 2: Checking Role Trust Policies
Identifying the role with permission to retrieve the secret was just part of the process. The next step was to determine whether I could assume that role.

I ran:
cloudfox aws -p cloudfoxable role-trusts -v2

Findings
The role Alexander-Arnold had the required permissions.
The trust policy for Alexander-Arnold allowed assumption directly from ctf-starting-user, which meant that I did not need explicit sts:AssumeRole permissions.

### Step 3: Assuming the Alexander-Arnold Role
Since ctf-starting-user was allowed to assume the Alexander-Arnold role, I created a new AWS CLI profile to assume this role.

I added the following configuration to ~/.aws/config:
[profile Alexander-Arnold]
region = us-west-2
role_arn = arn:aws:iam::ACCOUNT_ID:role/Alexander-Arnold
source_profile = cloudfoxable

Then, I verified the role assumption:
aws --profile Alexander-Arnold sts get-caller-identity
Success! The output confirmed that I had assumed the Alexander-Arnold role.

### Step 4: Retrieving the Secret
Now that I had access to the required role, I attempted to retrieve the secret.

I ran:
aws secretsmanager get-secret-value --secret-id DomainAdministrator-Credentials --profile Alexander-Arnold

This returned:
{
  "SecretString": "FLAG{this_is_the_secret_flag}"
}
Flag Retrieved!

## Challenges Faced
### 1. Identifying the Correct IAM Role
Initially, I wasn't sure which role had access to the secret. To solve this, I used CloudFox permissions enumeration to find roles with SecretsManager access.

### 2. Understanding Role Trust Policies
I first checked my own permissions and found no sts:AssumeRole permission. However, CloudFox's role-trusts output revealed that Alexander-Arnold explicitly trusted ctf-starting-user, allowing me to assume the role without sts:AssumeRole.

### 3. AWS CLI Profile Misconfiguration
The first time I tried to assume the role, I made an error in the role_arn format. After correcting it in the AWS CLI profile, the assumption worked.

## Blue Team Perspective: Defensive Measures
From a security standpoint, the following best practices could prevent unauthorized access:

Principle of Least Privilege (PoLP): Ensure that only necessary roles have access to sensitive secrets.
Role Trust Policies: Restrict IAM roles to only trusted users and groups.
AWS CloudTrail Logging: Monitor sts:AssumeRole and secretsmanager:GetSecretValue events for suspicious activity.
IAM Policy Review: Regularly audit IAM roles and policies to prevent over-permissive configurations.

## Conclusion
This challenge demonstrated key AWS security concepts, including role enumeration, privilege escalation, and secrets retrieval.

### Key Takeaways
IAM role trust relationships can allow privilege escalation without explicit permissions.
CloudFox is an effective tool for identifying AWS permission misconfigurations.
AWS Secrets Manager secrets should be tightly restricted to prevent unauthorized access.

## Commands Used

### Enumerate AWS Permissions
cloudfox aws -p cloudfoxable permissions -v2 | grep -i secret

### Check Role Trust Policies
cloudfox aws -p cloudfoxable role-trusts -v2

### Create AWS Profile for Role Assumption
[profile Alexander-Arnold]
region = us-west-2
role_arn = arn:aws:iam::ACCOUNT_ID:role/Alexander-Arnold
source_profile = cloudfoxable

### Verify Role Assumption
aws --profile Alexander-Arnold sts get-caller-identity

### Retrieve the Secret
aws secretsmanager get-secret-value --secret-id DomainAdministrator-Credentials --profile Alexander-Arnold
This exercise reinforced the importance of IAM role security and proper access control in AWS environments.
