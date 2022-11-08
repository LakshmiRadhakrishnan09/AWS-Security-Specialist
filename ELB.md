
## Elastic Load Balancer

* Classic
* Application
* Network


* Target pool of services
* Health check of services
* ELB is managed. Auto scale. No single point of failure.
* Client has a single well known ip
* ELB - Traffic across AZ.
* DDos protection

* internet-facing : Only ALB in public subnet. EC2 instances in private subnet. 
      * If EC2 is in private subnet
      * Create a public subnet in same AZ
      * Configure ELB with public subnet
      * Configure SG of instances and LB.
* internal: LB in private.

### ALB vs NLB

- ALB supports only HTTP/1.1, HTTP/2, or gRPC. 
- NLB is faster
- NLB can handle traffic spikes
- NLB provides static IP address for clients to connect. ALB Ip changes.

The ALB is like a fully-managed, scalable, and highly available version of NGINX, HAProxy, or Caddy.

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
    * Can be used for distributing traffic across ec2, lambda, on-premise and containers
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
 Gateway Load Balanacer   \
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
- **Alias Record** to route traffic to LB in Route53. Eg site1.example.com -> elb.us-west-1.amazon.com

ALB need to be configured with Security Cyphers. List of cyphers that will be used with client. 

Certificate is Regional. One certificate in each region.

### End to end encryption
If you need end-end encryption, then you need to configure certificates in the webserver. Since load balancer to webserver/target is using the private IP, you can also use self-signed and expired certificates in the webserver. The load balancer does not check the validity of the certificates.

HTTPS Listener: Uses SSL offload. This feature enables traffic encryption between your load balancer and the clients that initiate SSL or TLS sessions.

**If you need to pass encrypted traffic to targets without the load balancer decrypting it, you can create a Network Load Balancer or Classic Load Balancer with a TCP listener on port 443. With a TCP listener, the load balancer passes encrypted traffic through to the targets without decrypting it.**

ALB HTTPS Listener: Need one X509 certificate.\
Uses security policy for cypher and protocols. \
NLB TLS Listener: need certificate \
NLB TCP Listener: no certificate. end to end encryption


To use a TLS listener, you must deploy at least one server certificate on your load balancer. The load balancer uses a server certificate to terminate the front-end connection and then to decrypt requests from clients before sending them to the targets. **Note that if you need to pass encrypted traffic to the targets without the load balancer decrypting it, create a TCP listener on port 443 instead of creating a TLS listener. The load balancer passes the request to the target as is, without decrypting it.**

### ALB and SNI

You can host multiple TLS secured applications, each with its own TLS certificate, behind a single load balancer. In order to use SNI, all you need to do is **bind multiple certificates to the same secure listener on your load balancer.** ALB will automatically choose the optimal TLS certificate for each client. These new features are provided at no additional charge.

The most common reason you might want to use multiple certificates is to handle different domains with the same load balancer. With SNI support we’re making it easy to use more than one certificate with the same ALB. 

#### SSL/TLS 

SSL technically refers to a predecessor of the TLS protocol. SSL/TLS can be used interchangebly. TLS is a protocol for secure transmission. TLS uses certificate based authentication.

**HTTP - TLS - TCP - IP**


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

**If u want to use SSL with S3 , u need to use cloudfront.**

Note:

Certificates in ACM is regional.

For certificate in cloudFront -> Only in N.virgiia. Not in all edge regions.

Security for ALB

- SG for instance with port for application port and health check port( 2 SG)
- SG and NACL should allow emphmeral ports
- ALB do not support mutual TLS authentication
- HTTPS Listener - Use certificate between client and LB. Decrypt at LB.

Security for NLB

- **NLB does not support Security Group**
- Instance SG should allow client IP
- For health check should allow NLB private ip or VPC CIDR range
- Instance subnet NACL should allow inboud and outbound for client ips and ports. ( For an ALB integrated intance, only LB  needs to be allowed)


<img width="925" alt="Screenshot 2022-11-08 at 4 12 01 PM" src="https://user-images.githubusercontent.com/33679023/200544090-43098222-006f-4907-a70c-3fd8e116ac39.png">

 
    
Security Policies

- You can choose the security policy that is used for front-end connections. 
- The ELBSecurityPolicy-2016-08 security policy is always used for backend connections. 
- **Application Load Balancers do not support custom security policies.**
- You can use one of the ELBSecurityPolicy-TLS policies to meet compliance and security standards that require disabling certain TLS protocol versions, or to support legacy clients that require deprecated ciphers.


Logging

- CloudWatch Metrics
- Access Logs
- CloudTrail Logs
- Request Tracing


ELB CloudWatch Metric: Metrics are collected Only when requests are flowing thr ALB. If there are no requests flowing through the load balancer or no data for a metric, the metric is not reported. ActiveConnectionCount, ConsumedLCUs, ProcessedBytes, ClientTLSNegotiationErrorCount, HealthHostCount, RequestCountPerTarget, TargetResponseTime

Access Logs - Optional feature. Elastic Load Balancing captures the logs and stores them in the Amazon S3 bucket that you specify as compressed files. You can disable access logs at any time. Each log contains information such as the time the request was received, the client's IP address, latencies, request paths, and server responses. 

Elastic Load Balancing access logs requests sent to the load balancer, **including requests that never made it to the targets.** For example, if a client sends a malformed request, or there are no healthy targets to respond to the request, the request is still logged. Elastic Load Balancing does not log health check requests.

When the load balancer receives a request from a client, it adds or updates the X-Amzn-Trace-Id header before sending the request to the target. 
