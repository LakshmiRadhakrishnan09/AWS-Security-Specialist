# Route 53

Domain Name Service in AWS.

What is a domain name server? Is a look up table that maps domains to IPs. \
Who maintains this table and where is it kept? When we type a domain name in browser, browser consults the DNS Resolver(that is part of OS), checks local cache.
If not found or expired(ttl), it talks to Name Servers. Name Server is a hierarchy of servers which are part of global DNS Service. \
Root servers (well known servers) ---> Returns name servers for ".com"  --- > To Authoritative name server( Enterprise Name Server)


Mapping is cached in Name servers.

Select appropriate TTL name.

Is a critical service. Multi-validation step to verify that you are the domain owner before u update mapping.

DNS Record Types:
- A record: domain to IP address
- AAAA record: IPV6
- Alias record (only for Route 53) : domain to AWS resource. Domain to AWS DNS name. **Support for zone apex(without www)**
- CNAME record: domain to domain. Abbrevated names to full name.
- MX servers: to traffic email traffic


Route 53 Alias record - inherits TTL settings from the target service.You cannot modify TTL for Route 53 record. 

DNS entries are public data.


Main Responsibility:
- Domain Registeration: Purchase a domain. For public certificate, use AWS Certificate Manager ( free certificate)
    - ACM verify that u are owner of domain.
    - Ask u to put a specific cname record and value in DNS. Then ACM will do a dns and thus it validate that u have permission to configure DNS(u are owner of DNS).
- Manage DNS record management

DNS records are organized as hosted zones.
- Public HZ: for internet. Need to register the domain
- Private HZ: to route traffic betwwen VPC. No need to register domain

Domain needs to be registered either with Route 53 domain registrar or any other registrars like Go Daddy.

When u create a hosted zone, Route 53 assign 4 name servers. There are Authoritive name server that answers DNS queries for your domain.
Uses a Global anycast network. DNS queries are answered by optimal name server based on network condition.

Global Anycast: a collection of servers share the same IP address and send data from a source computer to the server that is topographically the closest. 

- Simple
- Failover
- Weighted
- Geolocation: Based on users location
- Latency
- Geoproximity
- Multi value Routing : Client decides which to use. Route 53 can return upto 8 ip/resource.

Charged by number of DNS queries

Alias Records:  Route 53 automatically detects changes to the resource. For example, if the IP address of the elastic load balancer changes, Route 53 will automatically respond to queries using the new IP address.Alias record, on the other hand, can be used for **zone apex and subdomains**. Also, there is **NO charge** for alias records.

NOTE: for Alias records, TTL setting is inherited from the target service. For example, if you point Alias record to an elastic load balancer, TTL of the load balancer is used for the alias record. You cannot modify them in Route 53.

Pay for Health Checks
