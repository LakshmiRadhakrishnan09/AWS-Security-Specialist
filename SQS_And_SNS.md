
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


SNS -> SQS in same account

For an Amazon SNS topic to be able to send messages to a queue, you must set a policy on the queue that allows the Amazon SNS topic to perform the sqs:SendMessage action.
```
{
  "Statement": [
    {
      "Effect": "Allow",
      "Principal": {
        "Service": "sns.amazonaws.com"
      },
      "Action": "sqs:SendMessage",
      "Resource": "arn:aws:sqs:us-east-2:123456789012:MyQueue",
      "Condition": {
        "ArnEquals": {
          "aws:SourceArn": "arn:aws:sns:us-east-2:123456789012:MyTopic"
        }
      }
    }
  ]
}
```

To publish message to SNS. There is no policy set in SNS to allow sending message to SQS. Policy is set at Queue only

```
{
  "Statement": [{
    "Sid": "grant-1234-publish",
    "Effect": "Allow",
    "Principal": {
      "AWS": "111122223333"
    },
    "Action": ["sns:Publish"],
    "Resource": "arn:aws:sns:us-east-2:444455556666:MyTopic"
  }]
}
```

SNS -> SQS cross account. There are two ways

The queue owner can create a subscription to the topic.
The topic owner can subscribe a queue in another account to the topic.

For Quque owner subscription (Automatic Confirmation): SNS topic should have below policy
```
{
   "Statement": [
      {
         "Effect": "Allow",
         "Principal": {
            "AWS": "QUEUE_OWNER_ACCOUNT"
         },
         "Action": "sns:Subscribe",
         "Resource": "arn:aws:sns:us-east-2:123456789012:MyTopic"
      }
   ]
}
```

For topic owner subscription

1. SNS will create subscription
2. To be able to communicate with the service, the queue must have permissions for Amazon SNS.
3. To confirm a subscription using SQS Service in the AWS Management Console



