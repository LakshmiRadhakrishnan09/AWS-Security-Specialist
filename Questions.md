1. A financial company needs to limit access to the web application only for users from specific regions where they have a presence.  
Which service can you use for this requirement?

 ``` AWS WAF has the option to allow or block based on the request originating countries. ```
 
 2. Which two services can generate findings when an S3 bucket allows access to an external account [outside of an AWS Organizations]?

``` IAM Access Analyser and Macie ```

3. Amazon Machine Images are centrally available and can be used in any region of your choice

``` False. AMI s are regional ```

4. Can u attach EBS volume to another instance in same region?

``` No. EBS volume is availability zone specific; so, you can attach an EBS volume to an instance in the same availability zone. To copy the volume to a different availability zone or a region, you need to first take a snapshot of the volume. Snapshot is stored in S3 in the same region and you can use the snapshot to create a new volume in any of the availability zones in that region. You can also copy snapshot to a different region and restore it – this is typically used for disaster recovery or for expansion to a new region

5. File system that can be used for both Linux and Windows:

```EFS is a file share for Linux instances. FSx for windows is a file share for windows instances and also now supports Linux and MacOS. FSx for Lustre is a high performance file share and it currently supports only Linux instances.```

6. The requests for a web application need to be routed to the webserver close to the user.  Which of these options would you use?  ( CF or Route 53 or ELB)

``` Route 53 latency-based routing.  With Route 53, you can configure region-specific endpoints. Route 53 will respond with the nearest endpoint address for a DNS query. Elastic Load Balancers are regional resources and are meant for routing traffic to web servers in the same region.  While private IP based cross-region registration is possible, it will not guarantee to route the request to the nearest webserver.  All webservers in an elastic load balancer receive equal traffic.  CloudFront routes request to the edge and the configured origin.CloudFront does not support region-specific webservers.```


7. CloudFront offers protection against DDoS attack

``` True.CloudFront uses global edge network to block certain types of infrastructure level DDoS attacks.You can also whitelist acceptable query parameters and headers to block cache-busting attacks.```

