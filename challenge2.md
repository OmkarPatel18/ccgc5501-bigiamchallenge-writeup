# Challenge number 2

## Challenge Statement
In this challenge, you need to send and receive messages using Amazon SQS (Simple Queue Service) to obtain the flag.

## IAM Policy
```json
copy and paste the IAM policy from the challenge here
```
### write a short analysis about the IAM policy here
```
* What do I have access to?
I have access to perform specific SQS actions on MyQueue.

* What don't I have access to?
I can get the send messages and receive messages.

* What do I find interesting?
I don't have access to create or modify queues, or access other AWS services.

* etc
The policy is tightly scoped to just the necessary SQS actions for the challenge.
```

## Solution
Used sqs:GetQueueUrl to obtain the URL for MyQueue.
Sent a message to the queue using sqs:SendMessage.
Used sqs:ReceiveMessage to retrieve messages from the queue.
Found the flag in one of the received messages.


## Reflection
* What was your approach?
Systematically used each allowed SQS action to interact with the queue.

* What was the biggest challenge?
Understanding the flow of messages in SQS and how to properly retrieve them.

* How did you overcome the challenges?
By carefully reading AWS SQS documentation and experimenting with each action.

* What led to the break through?
From realizing the flag was hidden within the message body.

* On the blue side, how can the learning be used to properly defend the important assets? 
Implement strict access controls on SQS queues, use encryption for sensitive messages, and regularly audit queue access and usage patterns.
