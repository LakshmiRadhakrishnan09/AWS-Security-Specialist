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


**AWS Client VPN**:End Users connect to VPC Subnet using Clinet VPN endpoint.Use case: Remote users(Developers/Employees) want to connect to AWS account. Provision a different VPC for clients to connect. VPC Peer this VPC to other VPCs.

Traditional network first address: used for identying the network \
last address: Used for broadcast

/23 -> 500 address
/22 -> 1000 address
/24 -> 255 address

Client Authenticates before connecting to VPN.

**AWS Site to Site VPN**: Usecase: Onpremise to AWS. Onpremise has to initiate transfer and keep the tunnel open.
* Option1: You manage. Install VPN server from Cisco in an EC2 instance.
* Option2: AWS Managed. Virtual Private Gateway. Two VPN tunnels per one VPN connection terminating in different AZ. Customer data center has Customer Gateway --> VPN Connection --> Virtual Private Gateway in AWS end. VPN Connection use **Public internet.** 
* Option3: Transit Gateway: Can connect to **multiple VPC**. Per region
* Option4: Accelerated using Global Accelerators


VPN Tunnels : 1.25 Gbps

ECMP: To scale beyond 1.25 Gbps. ECMP enabled transit gateway. Use multiple VPN tunnels.

**AWS Direct Connect**: Usecase: Onpremise to AWS. Consistent Bandbidth and avoid risk. **Private Network.**.Is not encrypted. Create fiber link from customer data center to Direct Connect location. Partner networks(They lied network to AWS Data centers. You link to Partner network hub).

Private VIF: \
Transit VIF: \
Public VIF: \

Direct Connect seems to be cost effictive as it is not using Public network.

**Direct Connect with VPN Backup**

DNS Resolution: Onpremise and AWS. Private hosted zone. Route 53 Resolvers. On premise DNS resolvers.

















