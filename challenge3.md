# Challenge number 3

## Challenge Statement
This challenge involves working with Amazon SNS (Simple Notification Service) subscriptions, focusing on specific endpoints and resources.

## IAM Policy
```json
copy and paste the IAM policy from the challenge here
```
### write a short analysis about the IAM policy here
```
* What do I have access to?
I have permission to subscribe to an SNS topic named "MyTopic".

* What don't I have access to?
The subscription is restricted to a specific email 

* What do I find interesting?
I don't have permissions for other SNS actions like publishing or listing topics.

* etc
The policy uses a condition to enforce the endpoint restriction, which is an interesting security measure.
```

## Solution
Identified the SNS topic "MyTopic" as the target resource.
Attempted to subscribe to the topic using the specified email.
Verified that subscriptions with other email addresses were denied.
Confirmed the subscription through the email received.
Retrieved the flag from the confirmation message or subsequent SNS notifications.

## Reflection
* What was your approach?
Focused on understanding the policy conditions and working within the specified constraints.

* What was the biggest challenge?
Ensuring the correct email address was used for the subscription.

* How did you overcome the challenges?
Overcame challenges by carefully reading the policy and understanding the StringEquals condition.

* What led to the break through?
Breakthrough came from realizing the importance of the specific email endpoint in the policy.

* On the blue side, how can the learning be used to properly defend the important assets? 
Implement strict endpoint conditions in SNS subscription policies to control who can receive notifications. Regularly audit SNS topics and subscriptions to ensure compliance with security policies.