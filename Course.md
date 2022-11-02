
Web -- App -- DB \

Web(Public Subnet). APP, DB ( Private Subnet)

To be resilient: Application should be spread across multiple AZ. Use ELB for web server in public subnet. ELB for app server in private subnet. 
IS ELB across multiple AZ? \
RDS in 3 AZ ( Primary, Secondary and StandBy) \
Auto Scaling Group:  Dynamic(Target, Step and Simple), Fixed, Scheduled \
Database Scaling: Read replicas \

Decouple layers: WEB - Queue - App -DB \
WEB - SNS - Queue(multiple. fan out) - App -DB \

Certificates at ELB. \
Authentication at ALB. ( With Cognito + SAML + OpenID Connect ) \
WAF at ALB \
Secret Manager \
AWS Config for configuration drift \
Inspector for server vulnerabilities \
Trusted advisor - best advices
Systems manager - for applying patch
Monitoring - CloudWatch \
Auditing - CloudTrail

Serverless implementation: API Gateway - Quque - Lambda - RDS \
Step FUnctions to orchestrate workflows.
WAF at API Gateway

Serverless :pay for what u use. Server based Solution: U need to pay for idle solutions. 

Route 53: For custom domains and routing


CloudFront does not support resource-based policies, but it does support resource-level policies. 

  <img width="956" alt="Screenshot 2022-10-29 at 9 56 28 PM" src="https://user-images.githubusercontent.com/33679023/198842540-23a7729f-5e5a-40f9-ab25-761d6e7f6f46.png">

<img width="760" alt="Screenshot 2022-10-29 at 10 00 30 PM" src="https://user-images.githubusercontent.com/33679023/198842793-3e420a60-7d47-4463-a3c4-9e30ec20b560.png">

EC2: Guest OS is controlled by customer.

Different instances running on the same physical machine are isolated from each other via the Xen hypervisor. 


How to Receive Notifications When Your AWS Account’s Root Access Keys Are Used

- Turn on cloudTrail cloudWatch Integration
- Create a metric in cloudwatch
- Automatically create role for CreateLogStream and PutLogEvent
```
{ $.userIdentity.type = "Root" && $.userIdentity.invokedBy NOT EXISTS && $.eventType != "AwsServiceEvent" }
```
- Create Alarm and send SNS topic

#### SSSM Session Manager

- Verify that SSM Agent is installed on the instance.
- Create the following VPC endpoints:
  - System Manager: com.amazonaws.region.ssm
  - Session Manager: com.amazonaws.region.ssmmessages
  - com.amazonaws.[region].ec2messages
  - KMS: com.amazonaws.region.kms (Optional. This endpoint is required only if you want to use AWS Key Management Service (AWS KMS) encryption for Session Manager.)
  - Amazon CloudWatch Logs (Optional. This endpoint is required only if you want to use Amazon CloudWatch Logs for Session Manager, Run Command).
 - The security group must allow inbound HTTPS (port 443) traffic from the resources in your VPC that communicate with the service.


