

VPC : 
* CIDR: 10.0.0.0/16
* Route Table:
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
* Internet Gateway: Attached to VPC
* NAT Gateway: 
     * Attached to a subnet. Either public or private
     * For instances in private subnet to have internet connectivity, configure NAT gateway as follows
          * NAT Gateway in public subnet
* Load Balancers are for EC2
   * ALB can be either internet-facing(need public subnet) or internal
   * Associate with a VPC
   * Choose Availability Zones ( Atleast two)


Question: If we have REST API deployed in an EC2 instance. Should EC2 be in public subnet or private subnet? \
Option1: ALB in public subnet. EC2 in public subnet. \
Option2: EC2 in private subnet. API Gateway - NLB - ALB - EC2?? Read more
