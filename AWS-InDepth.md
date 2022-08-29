https://explore.skillbuilder.aws/learn/course/1283/advanced-architecting-on-aws-online-course-supplement

https://us-east-1.mango.aws.training/class/uu6qvRNkMiUVFE9sTUmddg

**VPC is per REGION**

**NAT Gateway**: For private instances to connect to internet. Instances cannot be accessed from internet.

**Load Balancer**: Distributes traffic to different AZ. 

Web Servers, App Servers and databases : Should be in **private network**. Use ALB to expose them to internet.

**Public Subnet**: Route table directs non local traffic to Internet Gateway. Public Subnet -> 0.0.0.0/0 to Internet Gateway.
An internet gateway enables resources in your public subnets (such as EC2 instances) to connect to the internet if the resource has a public IPv4 address 
or an IPv6 address. Similarly, resources on the internet can initiate a connection to resources in your subnet using the public IPv4 address or IPv6 address. 


Route 53 : Across Region Load balancing.

VPC Gateway End Point: Add a entry in Route table. Use private network. Cost will be less. \
VPC Interface End Point:DNS Entry. ENI. 

S3 DNS name with and without VPC end point:??

Centralized Log Managmenet: Application Account and Logging Account. Cross account Role. 

AWS IAM Identity Center: AWS SSO.

SCP Policies can be applied at root, OU or account. Only DENY. 

Without AWS SSO: Active Directory, AWS access, External services. Multiple logins required.'

AWS Control Tower: Automated safeguards for complaince and governance. 20 Preventative and 6 Detective GuardRails. With best practices - Audit Account, Logging Account,
CloudFormation StackSets, AWS Organizations. AWS SSO. Preventative Guardrails using SCP in AWS Organizations. Eg : Disallow public S3 access. Detective
guard rails using AWS Config and rules. AWS Control Tower Dashboard with complaint posture.

AWS Control Tower vs AWS Organizations: AWS Control tower is automated. Use AWS organization if u want more control.














