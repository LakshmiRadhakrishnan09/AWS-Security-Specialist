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
  * If the subnet "Auto-assign public IPv4 address" attribute is true or providing while creating a instance , a public IP address is assigned to your instance from Amazon's pool of public IPv4 addresses, and is not associated with your AWS account. When a public IP address is disassociated from your instance, it is released back into the public IPv4 address pool, and you cannot reuse it. By default an instance is not assigned a public IP
  * Private address is attached to primary ENI. Disassociated when instance is terminated.
  * A private DNS hostname is also given that resolves to private ip
  * Secondary interfaces can also be attched. Can be detached and attch to another instance.
  * Public Ip is released when u stop a instance.
  * Elastic IP: Remain attached until u detach.
* Security Group
   * Allow rules only
   * Both inbound and outbound
   * Default SG: **Allow all outbounds**
   * Statefull firewall : **If inbound is allowed, outbound is allowed**
   * Outbound rule: When server need to talk to external services
* Network ACL
   * At subnet level
   * Both allow and deny
   * Stateless : Allow both request and response
   * **Ordered**: First rule that matches will allow or deny. From lowest number. "1" has highest order. Starts evaluating from "1".
   * Default NACL: **Allow all inbound and outbound traffic**
   * Response traffic is send to an ephemeral port. Servers listen on well known ports for requests that server process. But listenes on ephemeral ports for reciving response. Solution: only allow epheral port for VPC ports only.
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
When we create a VPC why we cant give the largest set? Why we should restrict the number of IPs in a VPC? - To avoid conflict\ 

Scenario: A Vpc with ip range : 193.239.32.0/20. If you want to create 4 subnets , then **2 bits are used for subnet identification**. If u want to create 16(2 ^ 4) subnets, then 4 bits are used for subnet identification. \
Assuming we need 4 subnets, then we have ( **/22 only**) i.e  193.239.32.0/22, 193.239.36.0/22, 193.239.40.0/22, 193.239.44.0/22 allocate to each subnet. \
/22 means 2 ^ (32-22) = 2 ^ 10 = 1024 IPs .\
**5 address are reserved for each subnet.** \
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
While creating interface endpoint, you should specify the service(eg: s3). 
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

Interface - Create an interface endpoint to send traffic to endpoint services that use a Network Load Balancer to distribute traffic. Traffic destined for the endpoint service is resolved using DNS.

GatewayLoadBalancer - Create a Gateway Load Balancer endpoint to send traffic to a fleet of virtual appliances using private IP addresses. You route traffic from your VPC to the Gateway Load Balancer endpoint using route tables. The Gateway Load Balancer distributes traffic to the virtual appliances and can scale with demand.

Gateway - Create a gateway endpoint to send traffic to Amazon S3 or DynamoDB using private IP addresses. You route traffic from your VPC to the gateway endpoint using route tables. Gateway endpoints do not enable AWS PrivateLink.

With a DNS public hosted zone, the records specify how to route traffic on the internet. With a DNS private hosted zone, the records specify how to route traffic in your VPCs.

Availability Zone and Availability Zone ID \
Scenario: SQS in AZ us-east-1. Interface Endpoint in us-east1 and use-east-2 to map to SQS. But us-east-1 used by SQS(AWS account) may logically map to a different zone. Use Availability Zone ID instead.

Read more:
* Gateway Endpoint
* Interface Endpoint

How to Configure Gateway Endpoint for S3? \
To create a gateway endpoint that connects to Amazon S3: Go to same VPC in the region as S3. Create gateway end point(Select VPC and Route Tables). AWS automatically add a route that points traffic destined for the service to the endpoint network interface. Note: So for each subnet, that have a route configured, traffic to S3 use VPC end point route.

How to create Interface End Point? \
For each subnet that you specify from your VPC, we create an endpoint network interface in the subnet and assign it a private IP address from the subnet address range. Create Endpoint.Select VPC.  For Subnets, select one subnet per Availability Zone from which you'll access the AWS service. Note: So for each subnet, traffic uses endpoint network interface.

https://docs.aws.amazon.com/images/vpc/latest/privatelink/images/endpoint-services.png![image](https://user-images.githubusercontent.com/33679023/197662277-ec6502c0-bfef-4161-890f-d23782012a1e.png)

1. When a service provider creates a VPC endpoint service, AWS generates an endpoint-specific DNS hostname for the service.Eg: vpce-svc-071afff70666e61e0.us-east-2.vpce.amazonaws.com. A service provider can also associate a private DNS name for their endpoint service, so that service consumers can continue to access the service using its existing DNS name.
2. When a service consumer creates an interface VPC endpoint, we create Regional and zonal DNS names that the service consumer can use to communicate with the endpoint service. If the service provider associated a private DNS name with the endpoint service, the service consumer can enable private DNS names for the interface endpoint. 


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

Why should two applications in same organisation connect over intenet? \

Use VPC Peering so that u can connect using AWS Global network( not internet)
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


VPN1: AWS VPC --- VGW(AZ1) --- Datacenter-1, AWS VPC ---- VGW(AZ2) ---- Datacenter -1 \
VPN2: AWS VPC --- VGW(AZ1) --- Datacenter-2, AWS VPC ---- VGW(AZ2) ---- Datacenter -2 \

If there are multiple VPCs, lot of connections to maintain. Instead of using Virtual Private Gateway use a Transit Gateway. Terminate VPN connection at transit gateway. 

Cloud Hub can also use Transit Gateway. 

How to set up Direct Connect? \
Multiple Direct Connect locations around the world. Work with ISP to set up connection to one of this DX location. For critical workloads use 2 Direct connect locations.\
Add Virtual Private Gateway(VGW) to VPC. Configure it to use direct connect link. Work for only single VPC. \
For multiple VPC, use Direct Connect Gateway. Multiple VPCs across regions to share direct connect link. \
Better option. Use Transit Gateway also. \

VPC1, VPC2, VPC3 (in same region) -- TGW1 , VPC1, VPC2, VPC3 (in another region) -- TGW2 \
Connect both to Direct Connect Gateway.

To connect to AWS Services( S3) from onpremise - Direct Connect with Public virtual interface.  

Direct Connect Resiliency - (https://aws.amazon.com/directconnect/resiliency-recommendation/) 
- For High Resiliency(provides resilience to connectivity failure due to a fiber cut or a device failure as well as a complete location failure. ) - two direct connection for two customer data center
- For Maximum Resiliency(provides resilience to device failure, connectivity failure, and complete location failure.) - two direct connection for per data center( 2 data center)
- Non critical workloads - at least two direct connect connections terminating on different devices at a single location. helps in the case of the device failure at a location but does not help in the event of a total location failure.


**VPN over Direct Connect** : Encryption over consistent network. \
USe VPN as backup for Direct Connect. 

Private IP VPN over AWS Direct Connect ensures that traffic between AWS and on-premises networks is both secure and private, allowing customers to comply with their regulatory and security mandates.

Direct Connect + VPN

You can create a transit virtual interface to connect to a transit gateway, a public virtual interface to connect to public resources (non-VPC services), or a private virtual interface to connect to a VPC.

AWS Direct Connect **public VIF** establishes a dedicated network connection between your network to public AWS resources, such as an Amazon virtual private gateway IPsec endpoint. 

This solution combines the benefits of the end-to-end secure IPSec connection with low latency and increased bandwidth of the AWS Direct Connect to provide a more consistent network experience than internet-based VPN connections. 

A BGP connection is established between the AWS Direct Connect and your router on the public VIF. Another BGP session or a static router will be established between the virtual private gateway and your router on the IPSec VPN tunnel.



## VPC Flow Log

Once configured appear in CloudWatch Log Groups.

- ACCEPT/REJECT
- Start addr/port and Destination addr/port
- Protocol(ICMP for Ping)

VPC flow log do not capture packet content. If u want to capture packet content:
1. VPC traffic mirroring for EC2 instances: allowing customers to natively replicate their network traffic without having to install and run packet-forwarding agents on EC2 instances

2. Third-party AMIs in the marketplace approved for packet capture

**IAM Role for VPC Flow Capture**

VPC service requires your permission to capture the flow log; this permission is granted using the IAM role.
```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "logs:CreateLogGroup",
        "logs:CreateLogStream",
        "logs:PutLogEvents",
        "logs:DescribeLogGroups",
        "logs:DescribeLogStreams"
      ],
      "Resource": "*"
    }
  ]
}
```

Extras

ALB supports Security Group. NLB does not support Security Group.

Edge services like CF, Lambda@Edge, GA has inbuild DoS attack protection. ELB also has DoS attack protection.

Default VPC CIDR block is fixed to 172.31.0.0/16

You can  share a transit gateway with another account using RAM. You can share subnet with other account using RAM.

NAT Instance Source Destination Check: Check if source and destination is actually the correct EC2 instance that is sending and receiving the packet.
This check protect ur instances. 
But 
In order to specify a **NAT instance as a target in the route table, you must disable Source and Destination Check on a NAT instance.**
If the source/destination check is not disabled, you cannot specify a NAT instance as a target in the route table.
This applies to any other security appliance that performs inline packet inspection before forwarding to the destination.

NAT Gateway and NAT Instance does not support IPv6 traffic. For IPv6, you would need to use an **egress-only internet gateway**. This is a highly available, horizontally scaled, redundant, VPC component that allows outbound communication over IPv6 and blocks unsolicited inbound IPv6 requests to your instance.


