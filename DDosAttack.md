
computer-network-osi-model-layers.png

https://s7280.pcdn.co/wp-content/uploads/2018/06/osi-model-7-layers-1.png

1. Application : HTTP
2. Presentation: SSL
3. Session
4. Transport: TCP/UDP
5. Network: IP
6. Data Link
7. Physical

Layer 3,4 -> Network and Transport Layer -> infrastructure layer attacks -User Datagram Protocol (UDP) reflection attacks and synchronize (SYN) floods. \
        -> Easy to Identify. To mitigate resources need to scale up the capacity.\
Layer 6,7 -> Presentation and Application layers -> application layer attacks . HTTP floods, DNS query floods, TLS abuse \
        -> looks like a valid request. Difficult to identify and mitigate

UDP reflection attack -> Spoof Target IP. Large Package Size. \
SYN Flood ->a malicious client sends a large number of SYN packets, but never sends the final ACK packets to complete the handshakes. \


DDos Attack Mitigation

All AWS customers can benefit from the automatic protections of **AWS Shield Standard** at no additional charge. 
It is offered on all AWS services and in every AWS Region.

AWS services that operate from edge locations - Amazon CloudFront, AWS Global Accelerator, and Amazon Route 53 can improve the DDoS resiliency of your application as AWS Shield DDoS mitigation systems are integrated with AWS edge services.

There is no charge for inbound data transfer on AWS and you do not pay for DDoS attack traffic that is mitigated by AWS Shield.


Using **AWS Shield Advanced** with Elastic IP addresses allows you to protect Network Load Balancer (NLBs) or Amazon EC2 instances.


https://d1.awsstatic.com/whitepapers/Security/DDoS_White_Paper.pdf

