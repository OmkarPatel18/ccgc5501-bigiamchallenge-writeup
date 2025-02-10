# Challenge number 4

## Challenge Statement
The challenge involves analyzing an IAM policy to determine permissions and find a way to access a hidden flag.

## IAM Policy
```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Principal": "*",
      "Action": "s3:GetObject",
      "Resource": "arn:aws:s3:::thebigiamchallenge-admin-storage-abf1321/*"
    },
    {
      "Effect": "Allow",
      "Principal": "*",
      "Action": "s3:ListBucket",
      "Resource": "arn:aws:s3:::thebigiamchallenge-admin-storage-abf1321",
      "Condition": {
        "StringLike": {
          "s3:prefix": "files/*"
        },
        "ForAllValues:StringLike": {
          "aws:PrincipalArn": "arn:aws:iam::133713371337:user/admin"
        }
      }
    }
  ]
}

```
### write a short analysis about the IAM policy here
```
* What do I have access to?
Anyone (Principal: *) can GetObject from thebigiamchallenge-admin-storage-abf1321 bucket.
ListBucket is restricted to the admin IAM user under account 133713371337 and only for objects inside files/*.

* What don't I have access to?
Listing the bucket contents unless I'm admin.
Retrieving objects outside files/* if I had ListBucket access.

* What do I find interesting?
Public s3:GetObject permission is a potential misconfiguration.
The s3:ListBucket restriction suggests hidden files could exist outside files/*.

* etc
```

## Solution
Exploit the GetObject permission:
Since anyone can download objects, try accessing files directly.
Use aws s3 cp s3://thebigiamchallenge-admin-storage-abf1321/flag.txt .

Bypass ListBucket restriction:
List access is limited, so brute-force possible filenames.
Try aws s3 cp s3://thebigiamchallenge-admin-storage-abf1321/hidden_flag.txt .

Extract the flag:
If successful, open the downloaded file to reveal the flag.

## Reflection
* What was your approach?
Identified public access, attempted direct object retrieval, and explored potential hidden files.

* What was the biggest challenge?
Lack of ListBucket permission required guessing file names.

* How did you overcome the challenges?
I guessed potential file names based on common patterns (e.g., flag.txt, hidden_flag.txt) and attempted direct downloads.

* What led to the break through?
Recognizing that s3:GetObject was open to everyone, which allowed direct file retrieval without needing ListBucket access.

* On the blue side, how can the learning be used to properly defend the important assets? 
Restrict public access, audit policies, enforce least privilege, and monitor access logs.