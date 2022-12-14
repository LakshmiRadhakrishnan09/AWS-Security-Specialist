1. A financial company needs to limit access to the web application only for users from specific regions where they have a presence.  
Which service can you use for this requirement?

 ``` AWS WAF has the option to allow or block based on the request originating countries. ```
 
 WAF: Use the **Geographic match rule** statement to block access to your site from specific countries or to allow access only from specific countries. You can use the geo match statement for country and region matching, as follows:

Country — Use a geo match rule by itself to manage requests based on their country of origin.
Region — **Use a geo match rule followed by a label match rule** to manage requests based on their country and region of origin. 

 Use the CloudFront geographic restrictions feature. Use this option to restrict access to all of the files that are associated with a distribution and to restrict access at the country level. If you need to prevent users in specific countries from accessing your content, you can use the CloudFront geographic restrictions feature to do one of the following:

Allow your users to access your content only if they’re in one of the approved countries on your allow list.
Prevent your users from accessing your content if they’re in one of the banned countries on your block list.

Note: For CloudFront distributions, if you use the CloudFront geo restriction feature, the feature doesn't forward blocked requests to AWS WAF. The feature does forward allowed requests to AWS WAF. If you want to block requests based on geography(not by country) and other AWS WAF criteria, use the AWS WAF geo match statement and do not use the CloudFront geo restriction feature(if u use CF it block by country not region).

If you have a use case for geographic restrictions where the restrictions don't follow country boundaries, or if you want to restrict access to only some of the files that you're serving by a given distribution, you can combine **CloudFront with a third-party geolocation service**. This can allow you to control access to your content based not only on country but also based on city, zip or postal code, or even latitude and longitude.
```
Summary:
To block by country : CF geo restriction
To block by region: WAF geographic match or CF third party geolocation service
``` 

 2. Which two services can generate findings when an S3 bucket allows access to an external account [outside of an AWS Organizations]?

``` IAM Access Analyser and Macie ```

3. Amazon Machine Images are centrally available and can be used in any region of your choice

``` False. AMI s are regional ```

4. Can u attach EBS volume to another instance in same region?

```
No. EBS volume is availability zone specific; so, you can attach an EBS volume to an instance in the same availability zone. 
To copy the volume to a different availability zone or a region, you need to first take a snapshot of the volume. **Snapshot is stored in S3** in the same region and you can use the snapshot to create a new volume in any of the availability zones in that region. 
You can also copy snapshot to a different region and restore it – this is typically used for disaster recovery or for expansion to a new region.
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

12. AWS Certificate Manager Certificates are: 

Free and can be used with AWS resources

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

Which of the following options will mitigate the threat? (Choose two.)

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
26. An AWS account includes two S3 buckets: bucket1 and bucket2. The bucket2 does not have a policy defined, but bucket1 has the following bucket policy:

In addition, the same account has an IAM User named `alice`, with the following IAM policy.

Bucket Policy for bucket 1 - allows alice \
IAM Policy for alice - allows bucket2

Which buckets can user `alice` access?

A. Bucket1 only
B. Bucket2 only
C. Both bucket1 and bucket2
D. Neither bucket1 nor bucket2

Answer - C

27. A company has two AWS accounts, each containing one VPC. The first VPC has a VPN connection with its corporate network. The second VPC, without a VPN, hosts an Amazon Aurora database cluster in private subnets. Developers manage the Aurora database from a bastion host in a public subnet as shown in the image.

A security review has flagged this architecture as vulnerable, and a Security Engineer has been asked to make this design more secure. The company has a short deadline and a second VPN connection to the Aurora account is not possible.
How can the Security Engineer securely set up the bastion host?
```
A. Move the bastion host to the VPC with VPN connectivity. Create a VPC peering relationship between the bastion host VPC and Aurora VPC.
B. Create an SSH port forwarding tunnel on the Developer's workstation to the bastion host to ensure that only authorized SSH clients can access the bastion host.
C. Move the bastion host to the VPC with VPN connectivity. Create a cross-account trust relationship between the bastion VPC and Aurora VPC, and update the Aurora security group for the relationship.
D. Create an AWS Direct Connect connection between the corporate network and the Aurora account, and adjust the Aurora security group for this connection.
```

A. Bastion host can be in private network.
```
28. A Developer who is following AWS best practices for secure code development requires an application to encrypt sensitive data to be stored at rest, locally in the application, using AWS KMS. What is the simplest and MOST secure way to decrypt this data when required?

A. Request KMS to provide the stored unencrypted data key and then use the retrieved data key to decrypt the data.
B. Keep the plaintext data key stored in Amazon DynamoDB protected with IAM policies. Query DynamoDB to retrieve the data key to decrypt the data
C. Use the Encrypt API to store an encrypted version of the data key with another customer managed key. Decrypt the data key and use it to decrypt the data when required.
D. Store the encrypted data key alongside the encrypted data. Use the Decrypt API to retrieve the data key to decrypt the data when required.
```
D

To encrypt data outside of AWS KMS:  1. Use the GenerateDataKey operation to get a data key. 2. Use the plaintext data key (in the Plaintext field of the response) to encrypt your data outside of AWS KMS. Then erase the plaintext data key from memory. 3. **** Store the encrypted data key (in the CiphertextBlob field of the response) with the encrypted data. ****  To decrypt data outside of AWS KMS:  1. **** Use the Decrypt operation to decrypt the encrypted data key. **** The operation returns a plaintext copy of the data key. 2. Use the plaintext data key to decrypt data outside of AWS KMS, then erase the plaintext data key from memory.

29. A company has a few dozen application servers in private subnets behind an Elastic Load Balancer (ELB) in an AWS Auto Scaling group. The application is accessed from the web over HTTPS. The data must always be encrypted in transit. The Security Engineer is worried about potential key exposure due to vulnerabilities in the application software.
Which approach will meet these requirements while protecting the external certificate during a breach?
```
A. Use a Network Load Balancer (NLB) to pass through traffic on port 443 from the internet to port 443 on the instances.
B. Purchase an external certificate, and upload it to the AWS Certificate Manager (for use with the ELB) and to the instances. Have the ELB decrypt traffic, and route and re-encrypt with the same certificate.
C. Generate an internal self-signed certificate and apply it to the instances. Use AWS Certificate Manager to generate a new external certificate for the ELB. Have the ELB decrypt traffic, and route and re-encrypt with the internal certificate.
D. Upload a new external certificate to the load balancer. Have the ELB decrypt the traffic and forward it on port 80 to the instances.
```

C - Use two certificates.

30. A company uses identity federation to authenticate users into an identity account (987654321987) where the users assume an IAM role named IdentityRole. The users then assume an IAM role named JobFunctionRole in the target AWS account (123456789123) to perform their job functions.
A user is unable to assume the IAM role in the target account. The policy attached to the role in the identity account is:

What should be done to enable the user to assume the appropriate role in the target account?


Scenario: the source account want to access the destination account. 

You can assume the IAM role from the source to destination account by providing your IAM user permission for the AssumeRole API. You must specify your IAM user in the trust relationship of the destination IAM role.

1. Create an IAM policy in source account
```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "sts:AssumeRole"
      ],
      "Resource": [
        "arn:aws:iam::DESTINATION-ACCOUNT-ID:role/DESTINATION-ROLENAME"
      ]
    }
  ]
}
```
2. Create Role in Destination account with trust policy
```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Principal": {
        "AWS": "arn:aws:iam::SOURCE-ACCOUNT-ID:user/SOURCE-USERNAME"
      },
      "Action": "sts:AssumeRole"
    }
  ]
}
```

31. A Security Engineer for a large company is managing a data processing application used by 1,500 subsidiary companies. The parent and subsidiary companies all use AWS. The application uses TCP port 443 and runs on Amazon EC2 behind a Network Load Balancer (NLB). For compliance reasons, the application should only be accessible to the subsidiaries and should not be available on the public internet. To meet the compliance requirements for restricted access, the Engineer has received the public and private CIDR block ranges for each subsidiary.
What solution should the Engineer use to implement the appropriate access restrictions for the application?
```
A. Create a NACL to allow access on TCP port 443 from the 1,500 subsidiary CIDR block ranges. Associate the NACL to both the NLB and EC2 instances
B. Create an AWS security group to allow access on TCP port 443 from the 1,500 subsidiary CIDR block ranges. Associate the security group to the NLB. Create a second security group for EC2 instances with access on TCP port 443 from the NLB security group.
C. Create an AWS PrivateLink endpoint service in the parent company account attached to the NLB. Create an AWS security group for the instances to allow access on TCP port 443 from the AWS PrivateLink endpoint. Use AWS PrivateLink interface endpoints in the 1,500 subsidiary AWS accounts to connect to the data processing application.
D. Create an AWS security group to allow access on TCP port 443 from the 1,500 subsidiary CIDR block ranges. Associate the security group with EC2 instances.
```

C

32. To meet regulatory requirements, a Security Engineer needs to implement an IAM policy that restricts the use of AWS services to the us-east-1 Region.
What policy should the Engineer implement?

It should be "DENY" policy . StringNotEquals aws:RequestedRegion = use-east-1


33. A company has several workloads running on AWS. Employees are required to authenticate using on-premises ADFS and SSO to access the AWS Management
Console. Developers migrated an existing legacy web application to an Amazon EC2 instance. Employees need to access this application from anywhere on the internet, but currently, there is no authentication system built into the application.
How should the Security Engineer implement employee-only access to this system without changing the application?
```
A. Place the application behind an Application Load Balancer (ALB). Use Amazon Cognito as authentication for the ALB. Define a SAML-based Amazon Cognito user pool and connect it to ADFS. Most Voted
B. Implement AWS SSO in the master account and link it to ADFS as an identity provider. Define the EC2 instance as a managed resource, then apply an IAM policy on the resource.
C. Define an Amazon Cognito identity pool, then install the connector on the Active Directory server. Use the Amazon Cognito SDK on the application instance to authenticate the employees using their Active Directory user names and passwords.
D. Create an AWS Lambda custom authorizer as the authenticator for a reverse proxy on Amazon EC2. Ensure the security group on Amazon EC2 only allows access from the Lambda function.
```

A - Almost Sure

or C - Not sure


34. A company plans to use custom AMIs to launch Amazon EC2 instances across multiple AWS accounts in a single Region to perform security monitoring and analytics tasks. The EC2 instances are launched in EC2 Auto Scaling groups. To increase the security of the solution, a Security Engineer will manage the lifecycle of the custom AMIs in a centralized account and will encrypt them with a centrally managed AWS KMS CMK. The Security Engineer configured the KMS key policy to allow cross-account access. However, the EC2 instances are still not being properly launched by the EC2 Auto Scaling groups. Which combination of configuration steps should the Security Engineer take to ensure the EC2 Auto Scaling groups have been granted the proper permissions to execute tasks?

Create a customer-managed CMK in the centralized account. Allow other applicable accounts to use that key for cryptographical operations by applying proper cross-account permissions in the key policy. Create an IAM role in all applicable accounts and configure its access policy with permissions to create grants for the centrally managed CMK. Use this IAM role to create a grant for the centrally managed CMK with permissions to perform cryptographical operations and with the EC2 Auto Scaling service-linked role defined as the grantee principal.

36. A company wants to deploy a distributed web application on a fleet of EC2 instances. The fleet will be fronted by a Classic Load Balancer that will be configured to terminate the TLS connection. The company wants to make sure that all past and current TLS traffic to the Classic Load Balancer stays secure, even if the certificate private key is leaked.
To ensure the company meets these requirements, a Security Engineer can configure a Classic Load Balancer with:

An HTTPS listener that uses a custom security policy that allows only perfect forward secrecy cipher suites.(This should be correct)
An HTTPS listener that uses the latest AWS predefined ELBSecurityPolicy-TLS-1-2-2017-01 security policy.

ALB will not allow custom security policy. Classic LB allows custom security policy.

ALB: You can use one of the ELBSecurityPolicy-FS policies if you require Forward Secrecy (FS).

37. A company policy requires that no insecure server protocols are used on its Amazon EC2 instances. These protocols include FTP, Telnet, and HTTP. The company’s security team wants to evaluate compliance with this requirement by using a scheduled Amazon EventBridge (Amazon CloudWatch Events) event to review the current infrastructure and create a regular report about the EC2 instances.

Which process will check the compliance of the company's EC2 instances? ( AWS Config, Trusted Advisor, GuardDuty, Amazon Inspector)

```
Amazon Inspector tests the network accessibility of your EC2 instances and the security state of your applications that run on those instances. Amazon Inspector assesses applications for exposure, vulnerabilities, and deviations from best practices. The rules in the Network Reachability package analyze your network configurations to find security vulnerabilities of your EC2 instances.
```

38. What if we need the capability to perform an inline inspection of all inbound and outbound traffic from our VPC?

```
Gateway Load Balanacer(network appliance in another VPC and vpc end point to connect to gateway LB)
```

39. If u want to capture packet content

```
VPC traffic MIRRORing for EC2 instances, Third-party AMIs in the marketplace approved for packet capture
```

40. How can I be notified when an AWS resource is non-compliant using AWS Config? How can I receive custom email notifications when a resource is created in my AWS account using AWS Config service?How can I receive custom email notifications when a resource is deleted in my AWS account using AWS Config service?

Use an EventBridge rule with a custom event pattern and an input transformer to match an AWS Config evaluation rule output as NON_COMPLIANT. Then, route the response to an Amazon Simple Notification Service (Amazon SNS) topic.

```
For Rule type, choose Rule with an event pattern. 
For Event source, choose AWS events or EventBridge partner events.
In the Event pattern pane, choose Custom patterns (JSON editor)
Choose Configure input transformer. Under Target input transformer, for the Input Path text box, copy and paste the transformer logic.(Mapping fromjson to variables)
In the Template text box, copy and paste the the template. Enter the time, rule, resource type, resource ID, AWS account ID and AWS Region, compliance, and resource information as required by your use case. Eg: <rule> evaluated the <resourceType> with Id <resourceId> in the account <awsAccountId> region <awsRegion> 
```

41.You are a security professional for a large organization, and you need to come up with an incident response plan to handle accidental credential disclosure.  You need a solution that automatically monitors for possible misuse of credentials.  Which service can meet this requirement?
```
GuardDuty
```
42. I need to be immediately alerted when someone is using the root identity in my aws environment. Which of these mechanisms should I use? 
```
Enable Trail and use CloudWatch Events(EventBridge). CloudWatchLog Metric Filter has delay of 15 minutes.
```
43. Your organization has subscribed to third-party threat intelligence feeds.  As part of the service, you receive a known list of suspicious IPs from various locations.  Your CISO wants to monitor activities to your resources from these IP addresses. What service can you use for this?

```
Use GuardDuty and specify suspicious IPs as part of the threat list. 
```
44. The security engineer must implement a complex set of firewall rules for an EC2 instance. The security group and NACL firewall only meet part of the requirement. What options can the engineer use? 

```
You can use host-based firewalls such as iptables and windows firewalls for complex requirements that cannot be met using NACL and Security groups.  WAF is useful for web application traffic, and this service integrates with Application Load Balancer, CloudFront, API Gateway, and so forth. It cannot be used as a firewall for EC2 instances.  Shield Advanced offers automatic DDoS protection.  Inspector only inspects for vulnerabilities and network reachability. It is not a firewall.
```

45. We need a tool to monitor and alert if services like FTP are running on our servers. Besides, if someone changes the security group or network ACL that allows FTP access from the internet, the tool needs to pinpoint the impacted servers and any process listening on the port.  What service can I use for this purpose?
```
Amazon Inspector can inspect an instance and find out if a process is listening on the ports.You can also trigger an inspector assessment from CloudWatch Events in response to a resource change. For example, monitor for security group or NACL changes using CloudWatch Events and trigger an inspector assessment for new exposures. 
```

46. our application is receiving several requests with malformed payload, and you would like to capture the network packets for analysis. What options do you have?

```
With VPC Traffic mirroring, you can capture and send the packets to a configured destination. For example, you can capture packets in different VPCs and send them to a central VPC for inspection and analysis.  You can also use an approved third-party marketplace AMI for packet capture. 
```

### Not Sure

During a recent internal investigation, it was discovered that all API logging was disabled in a production account, and the root user had created new API keys that appear to have been used several times.
What could have been done to detect and automatically remediate the incident?

A. Using Amazon Inspector, review all of the API calls and configure the inspector agent to leverage SNS topics to notify security of the change to AWS CloudTrail, and revoke the new API keys for the root user.
B. Using AWS Config, create a config rule that detects when AWS CloudTrail is disabled, as well as any calls to the root user create-api-key. Then use a Lambda function to re-enable CloudTrail logs and deactivate the root API keys. Most Voted
C. Using Amazon CloudWatch, create a CloudWatch event that detects AWS CloudTrail deactivation and a separate Amazon Trusted Advisor check to automatically detect the creation of root API keys. Then use a Lambda function to enable AWS CloudTrail and deactivate the root API keys.
D. Using Amazon CloudTrail, create a new CloudTrail event that detects the deactivation of CloudTrail logs, and a separate CloudTrail event that detects the creation of root API keys. Then use a Lambda function to enable CloudTrail and deactivate the root API keys.

Option B AWS Config seems correct. But AWS config remediation works by SSM document. SSM automation document can call  remediation Lambda function that removes the non-compliants.


A company has multiple production AWS accounts. Each account has AWS CloudTrail configured to log to a single Amazon S3 bucket in a central account. Two of the production accounts have trails that are not logging anything to the S3 bucket.
Which steps should be taken to troubleshoot the issue? (Choose three.)

A. Verify that the log file prefix is set to the name of the S3 bucket where the logs should go.
B. Verify that the S3 bucket policy allows access for CloudTrail from the production AWS account IDs.
C. Create a new CloudTrail configuration in the account, and configure it to log to the account's S3 bucket.
D. Confirm in the CloudTrail Console that each trail is active and healthy.
E. Open the global CloudTrail configuration in the master account, and verify that the storage location is set to the correct S3 bucket.
F. Confirm in the CloudTrail Console that the S3 bucket name is set correctly.

A - ??
B - Should be Correct
F - Should be Correct


D or A ( Should be A)

There is no CloudTrail console which shows trail is healthy. It shows if trail is enabled or not.

An employee accidentally exposed an AWS access key and secret access key during a public presentation. The company Security Engineer immediately disabled the key.
How can the Engineer assess the impact of the key exposure and ensure that the credentials were not misused? (Choose two.)

A. Analyze AWS CloudTrail for activity. Most Voted
B. Analyze Amazon CloudWatch Logs for activity.
C. Download and analyze the IAM Use report from AWS Trusted Advisor.
D. Analyze the resource inventory in AWS Config for IAM user activity.
E. Download and analyze a credential report from IAM. 

A - Sure
E - Not sure. Should be correct as it helps in assessing if credentials are used or not.

IAM Access Advisor - Helps to see what services a user/role accessed.

You can generate and download a credential report that lists all users in your account and the status of their various credentials, including passwords, access keys, and MFA devices. Reports password_last_used, access_key_1_last_used_date, access_key_2_last_used_date

A distributed web application is installed across several EC2 instances in public subnets residing in two Availability Zones. Apache logs show several intermittent brute-force attacks from hundreds of IP addresses at the layer 7 level over the past six months.
What would be the BEST way to reduce the potential impact of these attacks in the future?

A. Use custom route tables to prevent malicious traffic from routing to the instances.
B. Update security groups to deny traffic from the originating source IP addresses.
C. Use network ACLs.
D. Install intrusion prevention software (IPS) on each instance.

D-??
B- No as SG cant deny.
C - A subnet can have only one NACL. you can have 20 inbound and 20 outbound rules per NACL.
A Route table controls outgoing traffic from VPC

Note: Packet Sniffing : promiscuous mode is not allowed
Host Based Firewall – Forward Deployed IDS
Host Based Firewall – Traffic Replication: Configure each host with an agent that collects all network traffic and sends that traffic to the IDS/IPS platform for inspection.
In-Line Firewall – Inbound IDS Tier

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

Real-time Application Monitoring with Amazon Kinesis and Amazon CloudWatch

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

May be C (There is no configuration called consecutive periods. We configure - Period, Evaluation Period and dataPoints)

https://aws.amazon.com/premiumsupport/knowledge-center/sns-sms-spending-limit-increase/ - is only foe sms ,not for email

When you create an alarm, you specify three settings to enable CloudWatch to evaluate when to change the alarm state:

**Period** is the length of time to evaluate the metric or expression to create each individual data point for an alarm. It is expressed in seconds. If you choose one minute as the period, the alarm evaluates the metric once per minute.
**Evaluation Periods** is the number of the most recent periods, or data points, to evaluate when determining alarm state.
**Datapoints to Alarm** is the number of data points within the Evaluation Periods that must be breaching to cause the alarm to go to the ALARM state. The breaching data points don't have to be consecutive, but they must all be within the last number of data points equal to Evaluation Period.


When you configure Evaluation Periods and Datapoints to Alarm as different values, you're setting an "M out of N" alarm. Datapoints to Alarm is ("M") and Evaluation Periods is ("N"). The evaluation interval is the number of data points multiplied by the period. For example, if you configure 4 out of 5 data points with a period of 1 minute, the evaluation interval is 5 minutes. If you configure 3 out of 3 data points with a period of 10 minutes, the evaluation interval is 30 minutes.


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

B - Almost sure

IAM policy required for CloudWatch

{
  "Version": "2012-10-17",
  "Statement":[{
      "Effect":"Allow",
      "Action":["cloudwatch:GetMetricData","cloudwatch:ListMetrics"],
      "Resource":"*",
      "Condition":{
         "Bool":{
            "aws:SecureTransport":"true"
            }
         }
      }
   ]
}
GetMetricData: Required to graph metric data in the CloudWatch console, to retrieve large batches of metric data, and perform metric math on that data.
GetMetricStatistics: Required to view graphs in other parts of the CloudWatch console and in dashboard widgets.
cloudwatch:DescribeAlarmsForMetric : Required to view alarms for a metric.
```


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
