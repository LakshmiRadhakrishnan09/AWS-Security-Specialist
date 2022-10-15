
## Elastic Load Balancer ##

* Classic
* Application
* Network


* Target poll of services
* Health check of services
* ELB is managed. Auto scale. No single point of failure.
* Client has a single well known ip
* ELB - Traffic across AZ.
* DDos protection

* internet-facing : Only ALB in public subnet. EC2 instances in private subnet. 
* internal: LB in private.

### Security
* Off load SSL/TLS encryption and decryption. Terminate SSL termination.
*     Manage certificate at LB
*     More compute power at EC2
* Offload Authentication
* Connection Draining
* Skicky sessions
* HTTP/2 : Multiple requests in single connection.
* Websockets: Long lived connections and bi directional
* Cross Zone Load Balancing: Traffic evenly across all EC2 instance. By default traffic is distrbuted across AZ. 

Classic: HTTP(layer 7) and TCP(layer 4). \
Application: 
    * Can be used for distributing traffic across ec2, lambda, on premise and containers
    * Layer 7
    * Can route based on content
    * We can have multiple services( micro services) under an ALB.
    * To host multiple web sites
    * Routing : IP based, source, query parameters.
 NLB:
    * Layer 4 lb - TCP and UDP
    * Can be used for distributing traffic across ec2, on premise and containers(Lambda not supported)
    * Exterme performance workloads
    * Single Static IP address per AZ.Used by clients for whitelisting. For eg: If a coroprate policy restricts client can talk to a specific list of IP address. ALB IP address changes.
    * Handles millions of requetsts.
    * Preserve client IP(Source IP). With ALB target application sees LB ip address
    * Can be used for building Private Link services( traffic stays in AWS network).
    
    
 Two applications in organization . Different VPC.
 Option1: Internet \
 Option2: VPC peering. Exposes all resources. \
 Option3: NLB in shared service. Create a private link powered service. VPC end point at consumer side.\
  
  What if we need the capability to perform an inline inspection of all inbound and outbound traffic from our VPC?
 Gateway Load Balnacer   \
 Gateway Load Balancer and network appliances are kept in a separate VPC.

You attach the gateway load balancer endpoint to your VPC.

Configure route table in your VPC such that all traffic is routed through the load balancer endpoint.

### Auto Scaling

* Across AZ.
* Lifecycle hooks
* Min, Max, Desired

Types
* Scheduler
* Dynamic: Based on cloudwatch Metric
* Predective: ML based

Used by
* EC2
* EC2 Spot Fleets
* ECS
* DynamoDB
* Aurora


### Enabling SSL at Load Balancer

- Register Domain using Route53Eg. www.example.com
- Certificate for Domain using Certificate Manager
- Attach certificate to LB
- Alias Record to route traffic to LB in Route53. Eg site1.example.com -> elb.us-west-1.amazon.com

ALB need to be configured with Security Cyphers. List of cyphers that will be used with client. 

Certificate is Regional. One certificate in each region.

### End to end encryption
If you need end-end encryption, then you need to configure certificates in the webserver. Since load balancer to webserver/target is using the private IP, you can also use self-signed and expired certificates in the webserver. The load balancer does not check the validity of the certificates.

### Enabling SSL for CloudFront

- Certificate in N.Virginia
- Attach certificate to CF
- Alias Record to route traffic to CF in Route53

Clinet ->(SSL)-> CloudFront ->(SSL)- ELB as Origin

CloudFront certificate in N.Virginia.

### S3 and SSL

Amazon S3 website endpoints do not support HTTPS. http://bucket-name.s3-website-Region.amazonaws.com. To use SSL end point:
1. Use REST API endpoint. Will not support custom domian
2. Use CloudFront

Note:

Certificates in ACM is regional.

For certificate in cloudFront -> Only in N.virgiia. Not in all edge regions.
    
