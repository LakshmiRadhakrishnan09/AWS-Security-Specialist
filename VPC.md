

VPC : 
* CIDR: 10.0.0.0/16. This is a private address block.
* Main Route Table: Service in VPC can communicate eachother with VPC.
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
* Internet Gateway: Attached to VPC. VPC has connection to internet. 
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

Deployment for a Web application \
* Web layer on public subnet in multiple AZs
* App layer on private subnet in multiple AZs
* DB layer on private subnet in multiple AZs
* ELB on public subnet for web server
* ELB on private subnet for app server


P
