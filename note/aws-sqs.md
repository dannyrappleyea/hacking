---
is_a: "[[note]]"
topics: "[[hacking]]"
in: 
---
# Notes
## Permissions
- https://docs.aws.amazon.com/AWSSimpleQueueService/latest/SQSDeveloperGuide/sqs-using-identity-based-policies.html
	- There are two ways to give your users permissions to your Amazon SQS resources: using the Amazon SQS policy system and using the IAM policy system. You can use one or the other, or both.
	- The Amazon SQS policy system lets you grant permission to other AWS Accounts, whereas IAM doesn't.
	- Cross-account permissions don't apply to the following actions:
		- [AddPermission](https://docs.aws.amazon.com/AWSSimpleQueueService/latest/APIReference/API_AddPermission.html)
		- [CreateQueue](https://docs.aws.amazon.com/AWSSimpleQueueService/latest/APIReference/API_CreateQueue.html)
		- [DeleteQueue](https://docs.aws.amazon.com/AWSSimpleQueueService/latest/APIReference/API_DeleteQueue.html)
		- [ListQueues](https://docs.aws.amazon.com/AWSSimpleQueueService/latest/APIReference/API_ListQueues.html)
		- [ListQueueTags](https://docs.aws.amazon.com/AWSSimpleQueueService/latest/APIReference/API_ListQueueTags.html)
		- [RemovePermission](https://docs.aws.amazon.com/AWSSimpleQueueService/latest/APIReference/API_RemovePermission.html)
		- [SetQueueAttributes](https://docs.aws.amazon.com/AWSSimpleQueueService/latest/APIReference/API_SetQueueAttributes.html)
		- [TagQueue](https://docs.aws.amazon.com/AWSSimpleQueueService/latest/APIReference/API_TagQueue.html)
		- [UntagQueue](https://docs.aws.amazon.com/AWSSimpleQueueService/latest/APIReference/API_UntagQueue.html)
	- 

## Sending to queue
The AWS Command Line Interface (AWS CLI) don’t pick up the destination Region from the queue URL. Must set the region through environment variable or parameter.

```
aws sqs send-message --queue-url https://sqs.us-east-1.amazonaws.com/80398EXAMPLE/MyQueue --region us-east-1 --message-body '{"foo": "bar"}'
```

## Receiving from queue
```
aws sqs receive-message --queue-url https://sqs.us-east-1.amazonaws.com/80398EXAMPLE/MyQueue --region us-east-1
```
