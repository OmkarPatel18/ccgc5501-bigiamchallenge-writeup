# Challenge number 1

## Challenge Statement
In this challenge, you need to access and list objects in an S3 bucket using specific IAM permissions.

## IAM Policy
```json
copy and paste the IAM policy from the challenge here
```
### write a short analysis about the IAM policy here
```
* What do I have access to?
I have access to retrieve objects from any S3 bucket.

* What don't I have access to?
I can list the contents of any S3 bucket.

* What do I find interesting?
The policy uses wildcards, granting broad access to all S3 buckets.

* etc
Interestingly, there's no restriction on specific buckets or objects.
```

## Solution
Used the AWS CLI or SDK to list the contents of S3 buckets using the s3:ListBucket permission.
Identified the target bucket containing the flag.
Retrieved the flag object using the s3:GetObject permission.
Read the contents of the flag object to complete the challenge.

## Reflection
* What was your approach?
Systematically explored available S3 buckets and their contents using the granted permissions.

* What was the biggest challenge?
Identifying the correct bucket and object containing the flag among potentially many S3 resources.

* How did you overcome the challenges?
By methodically listing and examining bucket contents.

* What led to the break through?
Breakthrough came from realizing the broad access granted by the wildcard in the resource ARN.

* On the blue side, how can the learning be used to properly defend the important assets? 
Implement least privilege principle by restricting access to specific buckets and objects. Avoid using wildcards in resource ARNs for sensitive data.