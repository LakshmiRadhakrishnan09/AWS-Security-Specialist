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
Which of the following options will mitigate the threat? (Choose two.)

```
Bypass the proxy and use an S3 VPC endpoint with a policy that whitelists only certain S3 buckets within Account 1.
Block outbound access to public S3 endpoints on the proxy server.
Configure Network ACLs on Server X to deny access to S3 endpoints.(Not ccorrect)
```





