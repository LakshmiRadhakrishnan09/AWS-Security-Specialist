**VPC**

VPC : 
* CIDR: 10.0.0.0/16. This is a private address block.
* Main Route Table: Service in VPC that enables communicate each other within VPC.
  * 10.0.0.0/16 : local
* PublicSubnet
  * CIDR: 10.0.1.0/24
  * Availability Zone : us-east-2a
  * Route Table
     * 0.0.0.0/0 : internet gateway
     * 10.0.0.0/16 : local
* PrivateSubnet
  * CIDR: 10.0.11.0/24
  * Availability Zone : us-east-2a
  * Route Table
     * 10.0.0.0/16 : local
* Internet Gateway: 
  * Attached to VPC. 
  * Without an Internet Gateway, nothing in a VPC can access the internet, even with a public IP address. 
  * To send and receive traffic, server must have public ip. Instance should have a public address for communicating with internet. 
* NAT Gateway: 
     * Attached to a subnet. Either public or private
     * For instances in private subnet to have internet connectivity, configure NAT gateway as follows
          * NAT Gateway in public subnet
* Load Balancers are for EC2
   * ALB can be either internet-facing(need public subnet) or internal
   * Associate with a VPC
   * Choose Availability Zones ( Atleast two)
* EC2
  * When u create an EC2, it is assigned a private Ip that belongs to CIDR range of the subnet.
  * If the subnet "Auto-assign public IPv4 address" attribute is true or providinf while creating a instance , a public IP address is assigned to your instance from Amazon's pool of public IPv4 addresses, and is not associated with your AWS account. When a public IP address is disassociated from your instance, it is released back into the public IPv4 address pool, and you cannot reuse it. By default an instance is not assigned a public IP
  * Private address is attached to primary ENI. Disassociated when instance is terminated.
  * A private DNS hostname is also given that resolves to private ip
  * Secondary interfaces can also be attched. Can be detached and attch to another instance.
  * Public Ip is released when u stop a instance.
  * Elastic IP: Remain attached until u detach.
* Security Group
   * Allow rules only
   * Both inbound and outbound
   * Default SG: Allow all outbounds
   * Statefull firewall : **If inbound is allowed, outbound is allowed**
   * Outbound rule: When server need to talk to external services
* Network ACL
   * At subnet level
   * Both allow and deny
   * Stateless : Allow both request and response
   * Ordered: First rule that matches will allow or deny. From lowest number
   * Default NACL: Allow all inbound and outbound traffic
   * Response traffic is send to an ephemeral port. Servers listen on well known ports. But listenes on ephemeral ports for reciving response. Solution: only allow epheral port for VPC ports only.
   * Tricky to configure



Question: If we have REST API deployed in an EC2 instance. Should EC2 be in public subnet or private subnet? \
Option1: ALB in public subnet. EC2 in public subnet. \
Option2: LB in public subnet and EC2 in private subnet. As per documentation this is possible...Try this out!! \ 
Option2: EC2 in private subnet. API Gateway - NLB - ALB - EC2?? Read more


Public ip is **globally unique**. Two devices cannot have same public ip. They are managed by ISPs.\
Private IP need to be unique within private network. Private ips can communicate to internet using Routers. Routers has private and public IP. The public ip of route is unique globally. It is issued by ISP. Routes by default come with firewall that prevents unsolicited incoming requets. So hackers cannot attach routers.\

Private IP address block: \
* 10.0.0.0/8 : Fisrt 8 bits represents 10. All Ips starts with 10.
* 172.16.0.0/12: First 8 bits represent 172. Next 4 bits represent 10.
* 192.168.0.0/16: First 8 bits represent 192. Next 8 bits represent 168. All Ips starts with 192.168.

Public IP address is organized by country, organization and device( as a hierarchy). So from IP adress we can identify to which country this message needs to be send. Routers use this information to transfer packets. 

Question: How organizatio manage IPv4 private address in VPC? Can subnets in different accounts share same IP blocks? : Results in conflict if we do VPC peering??
Can we use same private IP in **different VPC** in same AWS account? : Should be yes. Because private ip should be unique only within a VPC. But for an organization with VPC peering, conflicts can happen if we use same private ip even across different VPC.
Do ALBs have public ip? It has public Ip. But it will change. Use public DNS name instead.
Within same VPC we cannot share private IP. \
Your VPC can interact with internet using Public IP assigned to instances. \
When we create a VPC why we cant give the largest set? Why we should restrict the number of IPs in a VPC? \ 

Scenario: A Vpc with ip range : 193.239.32.0/20. If you want to create 4 subnets , then 2 bits are used for subnet identification. If u want to create 16(2 ^ 4) subnets, then 4 bits are used for subnet identification. \
Assuming we need 4 subnets, then we have 193.239.32.0/22, 193.239.36.0/22, 193.239.40.0/22, 193.239.44.0/22 allocate to each subnet. \
/22 means 2 ^ (32-22) = 2 ^ 10 = 1024 IPs .\
5 address are reserved for each subnet. \
So only 1019 address.

Question: What is difference between public and private subnet. \
Answer: A public subnet has a route to the Internet Gateway. So if an instance in public subnet tries to connect to internet, it is routed to internet gateway and then to internet. 

VPC is for a region. \
Subnet is for a AZ. \
Instances are deployed in a subnet. \

| Scenario                     | Behaviour |
| ---------------------------- | ------------- |
| Public ip in public subnet   | Accessible from internet. Can connect to internet  |
| Private ip in public subnet  | Not accessible from internet. Can connect to internet  |
| Public ip in private subnet with NAT  | Not accessible from internet. Can connect to internet  |
| Private ip in private subnet with NAT | Not accessible from internet. Can connect to internet |

To be accessible from internet: Should have a public IP. Should have a route to Internet Gateway.

Deployment for a Web application \
* Web layer on public subnet in multiple AZs
* App layer on private subnet in multiple AZs
* DB layer on private subnet in multiple AZs
* ELB on public subnet for web server
* ELB on private subnet for app server

Question: how can instances in private network use AWS resources? \
Answer: Use NAT. NAT gateway should be in public subnet. Update route table of private subnet to allow traffic 0.0.0.0/0 to Nagw-id.  \
Option2: End point. Add endpoint in your VPC. For same region. For different region should use NAT gateway. With endpoint S3 and DynamoDB will have routing by private IP. So it will appear like they are part of your VPC. 

https://www.uturndata.com/2021/02/23/aws-quick-tips-internet-gateways-nat-gateways-and-nat-instances/

NAT Gateway 
* Managed Service- Automatically scale
* Elastic IP
* Is bound to a subnet(AZ)
* Single point of failure
* Deploy NAT in multiple AZ

EndPoints
* Gateway Endpoints
  * S3, DynamoDb
  * Create endpoint in your VPC
  * Update Route table of private subnet and public subnet
    * prefix_list_id	-> gateway_endpoint_id
* Interface Endpoints
  * All newer services use Interface Endpoint
  * Create Interface endpoint in your subnet. One for one AZ.
  * Service will appear as a local service
  * No need to update route table
  * Private DNS Host name option in ur VPC should be enabled. This automatically maps to Endpoint IP address.So even though services use SQS DNS name, requests are routed through interface endpoints.
  * One Interface Endpoint per AZ. 
  * Interface Endpoints are also called PrivateLinks
  * Interface EndPoint can be used not only for interacting with AWS resources but also for interacting with your own services.

Availability Zone and Availability Zone ID \
Scenario: SQS in AZ us-east-1. Interface Endpoint in us-east1 and use-east-2 to map to SQS. But us-east-1 used by SQS(AWS account) may logically map to a different zone. Use Availability Zone ID instead.

Read more:
* Gateway Endpoint
* Interface Endpoint

Scenario: EC2-1 in public subnet with Internet gateway. EC2-2 in private subnet. \
AWS Session Manager. Outside VPC. A AWS Service. By default AWS Session Manager can connect to EC2-1. \
For AWS SSM to connect to private subnet, need to create a interface end point. Create an interface end point and add it to private subnet. Add service as ssm in same region and AZ. \
After this u can connect to rpivate EC2-2 using SSM. \

Secanario: EC2-1 in public subnet with Internet gateway. EC2-2 in private subnet. \
Public S3. \
S3 is outside VPC. Only Public instance can access S3. \

Public Instance can access S3. \
aws s3 cp s3://labstack-940b008d-c20a-4b52-b4d3-31a1d9-labbucket-vmksv0anfyt9/demo.txt ~/. \
Private Instance does not have a route to S3.To connect, create vpc end point. \


Scenario: If S3 is private, then can it be accessed via internet? Can it be accessed only NAT? Can it be accessed by only VPC end point. \
Answer: Only using VPC endpoint.

SQS Security: SQS by default is publicaly accessible. You can control access by policies. Security best practice - Use VPC end point for accessing quque. use queue policies to control access to queues from specific Amazon VPC endpoints or from specific VPCs.

Scenario: Is there a change in end point for S3 and SQS with and without VPC endpoint? How to confirm we are using VPC end point?


****************************************************************************************************************************************************

## Integrating between VPC

Why should two applications in same organisation over intenet? \
Two VPC into a single logical network: VPC peering. Once connected they can communicate with private IP. No Single point of failure. CIDR should not overlap. Need to update route table in both VPC. this-vpc-cidr-block -> local. other-voc-cidr-local -> peering-connection. Peering connection is bi-directional(both side traffic)  and both parties need to accept. Not transitive. 
We can peer VPC
* In same region
* Different region
* Across accounts

Transit Gateway: Connect all VPC to transit gateway. One connection per VPC. To route traffic between region, u need to peer transit gateway in both regions. TG1 in region 1 ---- TG Peering ---- TG2 in region2

## Third Party Integration

Third party is not in cloud : ALB end point. Traffic over internet. \
Third party in AWS Cloud: We cannot peer VPC . It is a security issue. To share application service privaltely. use **interface endpoints**. \
Steps for configuring the service( expose the service):
* Network Load balancer
* Create end point service configuration 
* Share configuration with third party
Steps for consumning service: 
* Add endpoint
Communication will be through AWS internet.Can we used by enterprsie to share applications.

## Log in into systems

Public instances are accessible. Public instances inbound Security Group must allow SSH and RDP. Allow only from corporate IP. \
Private instances: Using Bastion Host or System Manager. \
Bastion Host: Deployed in public subnet. Assigned with Elastic IP. \
  * Bastion Host SG: SSH and RDP from corporate network
  * Private Instance: SSH and RDP from Bastion Host
Systems Manager: Session Manager. Agent required. Can establish a remote session. Private instances need to create interface endpoints. Much safer. Do not use SSH or RDP ports. \

## End to end flow

Scenario: EC2 with public ip in public subnet. From internet request send with public ip. Internet Gateway has a lookup table which maps all public address in the VPC to corresponding private address. Once EC2 instance send the request back, IG does an address translation to remove private ip with public ip and forward the request. \
EC2 instance in private subnet. Cannot receive request from internet. It can make outbount calls to internet using NAT. NAT will do address transaltation and replace private ip of EC2 instance with its own public ip and fwd to destination machine. IG will send the request with elatic ip of NAT as origin address.

## Hybrid Network

Question: How to connect AWS cloud of an enterprise with its onpremise data center? \
Answer: VPN connectivity.IPSec based encrypted communication. Cloud becomes an extension of ur data center. Use private ip over encrypted channel. Channel is over internel. Inconsistent performance issue. \
Direct Connect: Dedicated link. Use private ip. But not over intenet. Consistent performance. 

VPC Connectivity Options:
* Site to Site: AWS and On Premise corporate network
* Cloud Hub: AWS to multiple branches. Multiple site to site vpn. Branch offices can communicate with each other and with AWS
* Client VPN: AWS and laptop. Remote access from any location.

How to set up site to site VPN? \
Add Virtual Private Gateway(VGW) to VPC. Add Customer Gateway(CGW- software or hardware) on on-premise network. Virtual Private Gateway is configured with customer gateway details. Is there a single point of failure? At AWS side VPN uses 2 tunnels each ending at different AZ. Customer Gateway is a single point of failure. For HA, we need to customer gateways in different data center. Two VPN connections from the same VGW but to different CGW. \


Add Virtual Private Gateway(VGW) to VPC. Add Customer Gateway(CGW- software or hardware) on on-premise network. Virtual Private Gateway is configured with customer gateway details. Is there a single point of failure? At AWS side VPN uses 2 tunnels each ending at different AZ. Customer Gateway is a single point of failure. For HA, we need two customer gateways in different data center. Two VPN connections from the same VGW but to different CGW. \--

VPN1: AWS VPC --- VGW(AZ1) --- Datacenter-1, AWS VPC ---- VGW(AZ2) ---- Datacenter -1 \
VPN2: AWS VPC --- VGW(AZ1) --- Datacenter-2, AWS VPC ---- VGW(AZ2) ---- Datacenter -2 \

If there are multiple VPCs, lot of connections to maintain. Instead of using Virtual Private Gateway use a Transit Gateway. Terminate VPN connection at transit gateway. 

Cloud Hub can also use Transit Gateway. 

How to set up Direct Connect? \
Multiple Direct Connect locations around the world. Work with ISP to set up connection to one of this DX location. For ctitical workloads use 2 Direct connect locations. \
Add Virtual Private Gateway(VGW) to VPC. Configure it to use direct connect link. Work for only single VPC. \
For multiple VPC, use Direct Connect Gateway.Multiple VPCs across regions to share direct connect link. \
Better option. Use Transit Gateway also. \

VPC1, VPC2, VPC3 (in same region) -- TGW1 , VPC1, VPC2, VPC3 (in another region) -- TGW2 \
Connect both to Direct Connect Gateway.

To connect to AWS Services( S3) from onpremise - Direct Connect with Public virtual interface.  

**VPN over Direct Connect** : Encryption over consistent network. \
USe VPN as backup for Direct Connect. 


