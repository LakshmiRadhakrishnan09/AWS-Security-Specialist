1. A financial company needs to limit access to the web application only for users from specific regions where they have a presence.  
Which service can you use for this requirement?

 ``` AWS WAF has the option to allow or block based on the request originating countries. ```
 
 2. Which two services can generate findings when an S3 bucket allows access to an external account [outside of an AWS Organizations]?

``` IAM Access Analyser and Macie ```

3. Amazon Machine Images are centrally available and can be used in any region of your choice

``` False. AMI s are regional ```

4. Can u attach EBS volume to another instance in same region?

```
No. EBS volume is availability zone specific; so, you can attach an EBS volume to an instance in the same availability zone. To copy the volume to a different availability zone or a region, you need to first take a snapshot of the volume. Snapshot is stored in S3 in the same region and you can use the snapshot to create a new volume in any of the availability zones in that region. You can also copy snapshot to a different region and restore it – this is typically used for disaster recovery or for expansion to a new region.
```

5. File system that can be used for both Linux and Windows:

```EFS is a file share for Linux instances. FSx for windows is a file share for windows instances and also now supports Linux and MacOS. FSx for Lustre is a high performance file share and it currently supports only Linux instances.```

6. The requests for a web application need to be routed to the webserver close to the user.  Which of these options would you use?  ( CF or Route 53 or ELB)

``` Route 53 latency-based routing.  With Route 53, you can configure region-specific endpoints. Route 53 will respond with the nearest endpoint address for a DNS query. Elastic Load Balancers are regional resources and are meant for routing traffic to web servers in the same region.  While private IP based cross-region registration is possible, it will not guarantee to route the request to the nearest webserver.  All webservers in an elastic load balancer receive equal traffic.  CloudFront routes request to the edge and the configured origin.CloudFront does not support region-specific webservers.```


7. CloudFront offers protection against DDoS attack

``` True.CloudFront uses global edge network to block certain types of infrastructure level DDoS attacks.You can also whitelist acceptable query parameters and headers to block cache-busting attacks.```

8. What capability allows you to have a single-tenant hardware security module in the AWS cloud for key generation and other cryptographic operations.

```CloudHSM```

9. Your organization requires the use of approved protocols and ciphers for HTTPS communication.  Where would you configure this information?

``` Security Policy in ALB```

10. You are planning to deploy your web app with the Application Load Balancer in Oregon and Frankfurt regions. Both regions need to be accessible using the same domain name. You have configured latency-based routing in Route 53.  How many certificates do you need in this scenario?

``` Two certificates one for each region.```

11. When you use HTTPS to connect to CloudFront distribution, which part of the connection is encrypted?

``` Client to Edge Location is encrypted. If you need end-end encryption, you also need to configure CloudFront to use HTTPS when talking to Origin.```

12. AWS Certificate Manager Certificates are: Free and can be used with AWS resources

13. The corporate requirement calls for a mandatory host intrusion detection system and hardening operating system parameters to secure the system. A containerized application developed by the company's application team needs to be deployed in AWS. Which of the following would you use that meets the requirements while minimizing the effort needed to maintain the system?

```
ECS Cluster with EC2 instance launch type. Even though the ECS manages the underlying EC2 instances, you still have administrative access to the EC2 instances. You can configure additional software that needs to be run along with optimizing the parameters based on your need. Fargate does not provide customers with access to underlying instances. While a customer can deploy their own cluster and manage EC2 instances, it requires more effort as ECS already takes care of cluster management
```

14. The container images are hosted on a third-party private repository. The image must be privately shared with applications running on an ECS cluster. How would you configure security to allow image access for authorized applications?

```
Store access credentials in Secrets Manager and configure Task Execution Role with permission to access Secrets Manager. The task execution role is assigned to a specific task definition and is used by ECS Container Agent to download images and run the container. The Container Instance Role would also work; however, the permissions are assigned to the EC2 instance, and all Tasks running on that instance would get elevated privileges. The task role is used for granting permissions to your application code to access other AWS services (like S3, DynamoDB, SQS, and so forth)
```

16. You would like to reduce the time to launch new tasks in your ECS Cluster. Which of these container repositories can reduce the image download time?

``` ECR: traffic internal to AWS network ```

18. A distributed application uses many containers to process the pending work. The containers are short-lived, and after completing the work, they stop running. The container logs must be stored in CloudWatch logs to troubleshoot any issues. How would you send the logs to CloudWatch logs?
```Configure awslog driver in task definition. Docker containers use log drivers to collect containers' standard output and standard error streams and forward the log to the configured destination. The awslogs driver publishes the captured logs to the CloudWatch log group. This is a straightforward setup to consolidate logs from containers```
20.How can I monitor the account activity of specific IAM users, roles, and AWS access keys?
```
CloudTrail Console(less than 90 days), CloudWatch Log Insights, S3 Athena
```
21.A threat assessment has identified a risk whereby an internal employee could exfiltrate sensitive data from production host running inside AWS (Account 1). The threat was documented as follows:
Threat description: A malicious actor could upload sensitive data from Server X by configuring credentials for an AWS account (Account 2) they control and uploading data to an Amazon S3 bucket within their control.
Server X has outbound internet access configured via a proxy server. Legitimate access to S3 is required so that the application can upload encrypted files to an
S3 bucket. Server X is currently using an IAM instance role. The proxy server is not able to inspect any of the server communication due to TLS encryption.
22. Which of the following options will mitigate the threat? (Choose two.)

```
Bypass the proxy and use an S3 VPC endpoint with a policy that whitelists only certain S3 buckets within Account 1.
Block outbound access to public S3 endpoints on the proxy server.
Configure Network ACLs on Server X to deny access to S3 endpoints.(Not ccorrect)
```
23. A company requires that IP packet data be inspected for invalid or malicious content.
Which of the following approaches achieve this requirement? (Choose two.)

```
A. Configure a proxy solution on Amazon EC2 and route all outbound VPC traffic through it. Perform inspection within proxy software on the EC2 instance.
B. Configure the host-based agent on each EC2 instance within the VPC. Perform inspection within the host-based agent.
```
24. A company plans to move most of its IT infrastructure to AWS. They want to leverage their existing on-premises Active Directory as an identity provider for AWS.
Which combination of steps should a Security Engineer take to federate the company's on-premises Active Directory with AWS? (Choose two.)
```
A. Create IAM roles with permissions corresponding to each Active Directory group.
B. Create IAM groups with permissions corresponding to each Active Directory group.
C. Configure Amazon Cloud Directory to support a SAML provider.
D. Configure Active Directory to add relying party trust between Active Directory and AWS.
E. Configure Amazon Cognito to add relying party trust between Active Directory and AWS.
```

A and D
24. A company plans to move most of its IT infrastructure to AWS. The company wants to leverage its existing on-premises Active Directory as an identity provider for
AWS.
Which steps should be taken to authenticate to AWS services using the company's on-premises Active Directory? (Choose three.)
```
A. Create IAM roles with permissions corresponding to each Active Directory group.
B. Create IAM groups with permissions corresponding to each Active Directory group.
C. Create a SAML provider with IAM.
D. Create a SAML provider with Amazon Cloud Directory.
E. Configure AWS as a trusted relying party for the Active Directory
F. Configure IAM as a trusted relying party for Amazon Cloud Directory.
```
ACE

25. Penetration Request not required for
```
Permitted Services
Amazon EC2 instances, NAT Gateways, and Elastic Load Balancers
Amazon RDS
Amazon CloudFront
Amazon Aurora
Amazon API Gateways
AWS Fargate
AWS Lambda and Lambda Edge functions
Amazon Lightsail resources
Amazon Elastic Beanstalk environments

Prohibited Activities
DNS zone walking via Amazon Route 53 Hosted Zones
Denial of Service (DoS), Distributed Denial of Service (DDoS), Simulated DoS, Simulated DDoS (These are subject to the DDoS Simulation Testing policy)
Port flooding
Protocol flooding
Request flooding (login request flooding, API request flooding)
```
An AWS account includes two S3 buckets: bucket1 and bucket2. The bucket2 does not have a policy defined, but bucket1 has the following bucket policy:

In addition, the same account has an IAM User named `alice`, with the following IAM policy.

Bucket Policy for bucket 1 - allows alice \
IAM Policy for alice - allows bucket2

Which buckets can user `alice` access?

A. Bucket1 only
B. Bucket2 only
C. Both bucket1 and bucket2
D. Neither bucket1 nor bucket2

Answer - C


### Not Sure

During a recent internal investigation, it was discovered that all API logging was disabled in a production account, and the root user had created new API keys that appear to have been used several times.
What could have been done to detect and automatically remediate the incident?

A. Using Amazon Inspector, review all of the API calls and configure the inspector agent to leverage SNS topics to notify security of the change to AWS CloudTrail, and revoke the new API keys for the root user.
B. Using AWS Config, create a config rule that detects when AWS CloudTrail is disabled, as well as any calls to the root user create-api-key. Then use a Lambda function to re-enable CloudTrail logs and deactivate the root API keys. Most Voted
C. Using Amazon CloudWatch, create a CloudWatch event that detects AWS CloudTrail deactivation and a separate Amazon Trusted Advisor check to automatically detect the creation of root API keys. Then use a Lambda function to enable AWS CloudTrail and deactivate the root API keys.
D. Using Amazon CloudTrail, create a new CloudTrail event that detects the deactivation of CloudTrail logs, and a separate CloudTrail event that detects the creation of root API keys. Then use a Lambda function to enable CloudTrail and deactivate the root API keys.

AWS Config seems correct. But AWS config remediation works by SSM document. Not by Lambda. C may be correct


A company has multiple production AWS accounts. Each account has AWS CloudTrail configured to log to a single Amazon S3 bucket in a central account. Two of the production accounts have trails that are not logging anything to the S3 bucket.
Which steps should be taken to troubleshoot the issue? (Choose three.)

A. Verify that the log file prefix is set to the name of the S3 bucket where the logs should go.
B. Verify that the S3 bucket policy allows access for CloudTrail from the production AWS account IDs.
C. Create a new CloudTrail configuration in the account, and configure it to log to the account's S3 bucket.
D. Confirm in the CloudTrail Console that each trail is active and healthy.
E. Open the global CloudTrail configuration in the master account, and verify that the storage location is set to the correct S3 bucket.
F. Confirm in the CloudTrail Console that the S3 bucket name is set correctly.


F - Should be Correct
B - Should be Correct

D or A


An employee accidentally exposed an AWS access key and secret access key during a public presentation. The company Security Engineer immediately disabled the key.
How can the Engineer assess the impact of the key exposure and ensure that the credentials were not misused? (Choose two.)

A. Analyze AWS CloudTrail for activity. Most Voted
B. Analyze Amazon CloudWatch Logs for activity.
C. Download and analyze the IAM Use report from AWS Trusted Advisor.
D. Analyze the resource inventory in AWS Config for IAM user activity.
E. Download and analyze a credential report from IAM. 

A - Sure
E - Not sure. Should be correct as it helps in assessing if credentials are used or not.

A distributed web application is installed across several EC2 instances in public subnets residing in two Availability Zones. Apache logs show several intermittent brute-force attacks from hundreds of IP addresses at the layer 7 level over the past six months.
What would be the BEST way to reduce the potential impact of these attacks in the future?

A. Use custom route tables to prevent malicious traffic from routing to the instances.
B. Update security groups to deny traffic from the originating source IP addresses.
C. Use network ACLs.
D. Install intrusion prevention software (IPS) on each instance.

D-??
B- No as SG cant deny.
C - A subnet can have only one NACL. you can have 20 inbound and 20 outbound rules per NACL.
A Route table controls outgoing traffic

A financial institution has the following security requirements:
✑ Cloud-based users must be contained in a separate authentication domain.
✑ Cloud-based users cannot access on-premises systems.
As part of standing up a cloud environment, the financial institution is creating a number of Amazon managed databases and Amazon EC2 instances. An Active
Directory service exists on-premises that has all the administrator accounts, and these must be able to access the databases and instances.
How would the organization manage its resources in the MOST secure manner? (Choose two.)
```
A. Configure an AWS Managed Microsoft AD to manage the cloud resources. Most Voted Most Voted
B. Configure an additional on-premises Active Directory service to manage the cloud resources.
C. Establish a one-way trust relationship from the existing Active Directory to the new Active Directory service. Most Voted
D. Establish a one-way trust relationship from the new Active Directory to the existing Active Directory service. Most Voted
E. Establish a two-way trust between the new and existing Active Directory services.
```
A and D??

A company has a forensic logging use case whereby several hundred applications running on Docker on EC2 need to send logs to a central location. The Security
Engineer must create a logging solution that is able to perform real-time analytics on the log files, grants the ability to replay events, and persists data.
Which AWS Services, together, can satisfy this use case? (Choose two.)
```
A. Amazon Elasticsearch
B. Amazon Kinesis
C. Amazon SQS
D. Amazon CloudWatch
E. Amazon Athena

```

A and B --?
or
A and D?? https://aws.amazon.com/blogs/big-data/implement-serverless-log-analytics-using-amazon-kinesis-analytics/

Amazon Kinesis Streams streams data on AWS, which allows you to collect, store, and process TBs per hour at a low cost.

Amazon Kinesis Firehose loads streaming data in to Amazon Kinesis Analytics, Amazon S3, Amazon Redshift, or Amazon OpenSearch Service.

Amazon Kinesis Analytics helps you analyze streaming data by writing SQL queries and in turn overcoming the management and monitoring of streaming logs in near real time. Analytics allows you to reference metadata stored in S3 in SQL queries for real-time analytics.

Application nodes run Apache applications and write Apache logs locally to disk. The Amazon Kinesis agent on the EC2 instance ingests the log stream in to the Amazon Kinesis stream.

The Lambda function consumes the aggregated response from the destination stream, processes it, and publishes it to Amazon CloudWatch. It is event driven: as soon as new records are pushed to the destination stream, they are processed in batches of 200 records.

An organization is using AWS CloudTrail, Amazon CloudWatch Logs, and Amazon CloudWatch to send alerts when new access keys are created. However, the alerts are no longer appearing in the Security Operations mail box.
Which of the following actions would resolve this issue?
```
A. In CloudTrail, verify that the trail logging bucket has a log prefix configured.
B. In Amazon SNS, determine whether the ג€Account spend limitג€ has been reached for this alert.
C. In SNS, ensure that the subscription used by these alerts has not been deleted.
D. In CloudWatch, verify that the alarm threshold ג€consecutive periodsג€ value is equal to, or greater than 1.
```

C or D

May be D


During a security event, it is discovered that some Amazon EC2 instances have not been sending Amazon CloudWatch logs.
Which steps can the Security Engineer take to troubleshoot this issue? (Choose two.)

```
A. Connect to the EC2 instances that are not sending the appropriate logs and verify that the CloudWatch Logs agent is running.
B. Log in to the AWS account and select CloudWatch Logs. Check for any monitored EC2 instances that are in the ג€Alertingג€ state and restart them using the EC2 console.
C. Verify that the EC2 instances have a route to the public AWS API endpoints.
D. Connect to the EC2 instances that are not sending logs. Use the command prompt to verify that the right permissions have been set for the Amazon SNS topic.
E. Verify that the network access control lists and security groups of the EC2 instances have the access to send logs over SNMP.
```

A - Sure
B or C?
May be C

You can query the CloudWatch agent to find whether it's running or stopped using Systems Manager Run command. 

If you update your CloudWatch agent configuration file, the next time that you start the agent, you need to use the fetch-config option. 

To connect your VPC to CloudWatch Logs, you define an interface VPC endpoint for CloudWatch Logs.

A Security Engineer discovers that developers have been adding rules to security groups that allow SSH and RDP traffic from 0.0.0.0/0 instead of the organization firewall IP.
What is the most efficient way to remediate the risk of this activity?
```
A. Delete the internet gateway associated with the VPC.
B. Use network access control lists to block source IP addresses matching 0.0.0.0/0.
C. Use a host-based firewall to prevent access from all but the organization's firewall IP.
D. Use AWS Config rules to detect 0.0.0.0/0 and invoke an AWS Lambda function to update the security group with the organization's firewall IP.
```

B or D - Should be D

How to Monitor AWS Account Configuration Changes and API Calls to Amazon EC2 Security Groups: \
Method 1 uses AWS Config to monitor changes to a security group’s configuration as part of an organization’s overall compliance auditing program.  \
Method 2 uses AWS CloudTrail and Amazon CloudWatch Events to identify AWS API calls that could change the configurations of VPC security groups. 

A Security Analyst attempted to troubleshoot the monitoring of suspicious security group changes. The Analyst was told that there is an Amazon CloudWatch alarm in place for these AWS CloudTrail log events. The Analyst tested the monitoring setup by making a configuration change to the security group but did not receive any alerts.
Which of the following troubleshooting steps should the Analyst perform?
```
A. Ensure that CloudTrail and S3 bucket access logging is enabled for the Analyst's AWS account. 
B. Verify that a metric filter was created and then mapped to an alarm. Check the alarm notification action.
C. Check the CloudWatch dashboards to ensure that there is a metric configured with an appropriate dimension for security group changes.
D. Verify that the Analyst's account is mapped to an IAM policy that includes permissions for cloudwatch: GetMetricStatistics and Cloudwatch: ListMetrics.
```
B - Almost sure

Create a metric filter and create an alarm
- Example security group configuration changes
- Example AWS Management Console sign-in failures
- Example: IAM policy changes

Example.com hosts its internal document repository on Amazon EC2 instances. The application runs on EC2 instances and previously stored the documents on encrypted Amazon EBS volumes. To optimize the application for scale, example.com has moved the files to Amazon S3. The security team has mandated that all the files are securely deleted from the EBS volume, and it must certify that the data is unreadable before releasing the underlying disks.
Which of the following methods will ensure that the data is unreadable by anyone else?
```
A. Change the volume encryption on the EBS volume to use a different encryption mechanism. Then, release the EBS volumes back to AWS.
B. Release the volumes back to AWS. AWS immediately wipes the disk after it is deprovisioned.
C. Delete the encryption key used to encrypt the EBS volume. Then, release the EBS volumes back to AWS.
D. Delete the data by using the operating system delete commands. Run Quick Format on the drive and then release the EBS volumes back to AWS.
```
C - Almost sure

For compliance reasons, an organization limits the use of resources to three specific AWS regions. It wants to be alerted when any resources are launched in unapproved regions.
Which of the following approaches will provide alerts on any resources launched in an unapproved region?
```
A. Develop an alerting mechanism based on processing AWS CloudTrail logs.
B. Monitor Amazon S3 Event Notifications for objects stored in buckets in unapproved regions.
C. Analyze Amazon CloudWatch Logs for activities in unapproved regions.
D. Use AWS Trusted Advisor to alert on all resources being created.
```

A - Almost Sure \
C - Not sure

An organization has three applications running on AWS, each accessing the same data on Amazon S3. The data on Amazon S3 is server-side encrypted by using an AWS KMS Customer Master Key (CMK).
What is the recommended method to ensure that each application has its own programmatic access control permissions on the KMS CMK?
```
A. Change the key policy permissions associated with the KMS CMK for each application when it must access the data in Amazon S3.
B. Have each application assume an IAM role that provides permissions to use the AWS Certificate Manager CMK.
C. Have each application use a grant on the KMS CMK to add or remove specific access controls on the KMS CMK. Most Voted
D. Have each application use an IAM policy in a user context to have specific access permissions on the KMS CMK.
```
C - As Grant allows programmatic access
