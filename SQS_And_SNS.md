
## AWS Messaging Systems

AWS Queuing Service
- SQS
- Amazon MQ

AWS push services
- SNS
- EventBridge

Streams
- History of all messages( can go back and look for past messages)
- Kept for retension period
- Multiple consumers can read all available records simultaneously

- Kinesis
- Managed Kafka

## Permission Management

SQS queue support resource based policy. Who can send. Who can receive. \
or u can set in Identity based policy Producer -> Allow to send msg. \
and set in Consumer -> Allow ro receive msg. \

For cross account access: Only queues resource based policy will work. Or Assume Role

SNS Topic Subscriber
- Email : No permission
- Lambda : Resource based policy
- SQS : Resource based policy
- Kinesis : IAM based policy

**Kinesis does not support resource based policy**ou can grant permission to the application using an identity-based policy (assigned to IAM Role, User, or Groups). 
Firehose relies on IAM Role to deliver messages to the destination.



Note:
Resource-based policy is used to specify who can access the resource (that is why a resource-based policy is also called an access policy). 

For SNS to call Lambda -> use resource-based policy and grant permission to the Topic to invoke the function. \
For Lambda to call S3 ->use  IAM Role
