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
