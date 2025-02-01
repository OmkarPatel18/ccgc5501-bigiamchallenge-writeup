# Challenge number 1

## Challenge Statement


## IAM Policy
```json
copy and paste the IAM policy from the challenge here
```
### write a short analysis about the IAM policy here
```
* What do I have access to?
The policy allowed listing and describing certain resources, with limited permissions on IAM roles and policies.

* What don't I have access to?
Denied full IAM privileges and sensitive actions like policy modifications.

* What do I find interesting?
Found a permission that indirectly allowed privilege escalation.

* etc
```

## Solution
Reviewed IAM Policy: Identified permissions allowing role assumption.
Enumerated Roles & Policies: Found a role with elevated access.
Assumed Role: Used sts:AssumeRole to gain higher privileges.
Retrieved Flag: Accessed the hidden resource with escalated permissions.

## Reflection
* What was your approach?
Focused on analyzing policy gaps and privilege escalation paths.

* What was the biggest challenge?
Identifying indirect privilege escalation paths.

* How did you overcome the challenges?
Systematically tested permissions and role assumptions.

* What led to the break through?
Found an overlooked role with AssumeRole permission.

* On the blue side, how can the learning be used to properly defend the important assets? 
Enforce least privilege, monitor IAM role assumptions, and restrict privilege escalation paths.