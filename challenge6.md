# CloudFoxable CTF - Challenge 1: "Do This First!"

## Overview
CloudFoxable is a Terraform-based AWS security challenge designed to help users identify security misconfigurations in cloud environments. This write-up documents the steps taken to deploy the CloudFoxable environment, retrieve the first flag, and clean up the deployed resources.

## Setup Process

1. Setting Up the AWS Environment

Created a new AWS account to ensure that no production resources or sensitive data were at risk.

Created an IAM user with administrative privileges and generated an access key for authentication.

Installed and configured the AWS CLI with the newly created IAM user's credentials.

Verified the AWS CLI configuration by running:
aws sts get-caller-identity
This confirmed the correct setup by displaying account details and the authenticated user.

2. Deploying CloudFoxable with Terraform

Installed Terraform and added it to the system's PATH.

Cloned the CloudFoxable repository:
git clone https://github.com/BishopFox/cloudfoxable
cd cloudfoxable/aws

Initialized Terraform:
terraform init

Copied the example Terraform variables file and updated it with the AWS profile:
cp terraform.tfvars.example terraform.tfvars

Edited terraform.tfvars to specify the AWS profile:
aws_local_profile = "my-aws-profile"

Applied the Terraform configuration to deploy the environment:
terraform apply
This provisioned the necessary AWS resources and displayed important configuration details.

## Retrieving the First Flag

Carefully reviewed the Terraform output at the end of deployment.

The output contained guidance on setting up AWS CLI access for the ctf-starting-user.

Configured AWS CLI for the ctf-starting-user.

Identified the flag format:
FLAG{challengeName::CamelCaseText}

Extracted the first flag from the Terraform output:
FLAG{congrats_you_are_now_a_terraform_expert_happy_hunting}

## Clean-Up Process

To remove all deployed resources and avoid unnecessary AWS costs, executed:
terraform destroy
This ensured that all CloudFoxable infrastructure was properly deleted.

## Key Takeaways

AWS IAM security: Highlighted the importance of least privilege access and security best practices.

Infrastructure as Code (IaC) benefits: Terraform simplifies the process of deploying and managing cloud resources.

Deployment outputs: Reviewing Terraform outputs is essential, as they may contain critical security insights.