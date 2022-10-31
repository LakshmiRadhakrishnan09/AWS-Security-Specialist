

## CloudFront

Content Delivery Network. CloudFront is a globally distrbuted infrastructure. Cache Content at Edge Locations. Edge Cache and Regional Edge Cache.

Traffic flows over AWS network.

Persist Connection to Origin

Why? \
For Global applications. Low latency for global audience apps. Solution -> App in one region. Use CloudFront. 

Types of content . Controlled by HTTP Cache Control Headers.
- Static : images, JavaScript
- Dynamic : weather(valid for some time)
- User data : photo, health report
- Non-cacheable : Payment card, OTP

CloudFront - Distribution Types
- web( for live straming
- rtmp( adobe media files)

Encryption at Transit

- HTTPS between viewer and CF
      - Update Viewer Protocol Policy( Redirect HTTP to HTTPS, HTTPS Only)
      - Redirect HTTP to HTTPS - charged two times
      - Can use SSL provided by AWS
      - Or using Custom Domian
- HTTPS between CF and origin
      - For S3
           - If your Amazon S3 bucket is configured as a website endpoint, you can't configure CloudFront to use HTTPS to communicate with your origin because Amazon S3 doesn't support HTTPS connections in that configuration.
           - When your origin is an Amazon S3 bucket that supports HTTPS communication, CloudFront always forwards requests to S3 by using the protocol that viewers used to submit the requests. The default setting for the Protocol (custom origins only) setting is **Match Viewer and can't be changed.**. So if u want https betwwen CF and S3, then u need to configure https between viewer and CF.

If you want your viewers to use HTTPS and to use alternate domain names for your files, choose one of the following options for how CloudFront serves HTTPS requests:

- Use Server Name Indication (SNI) – Recommended
- Use a dedicated IP address in each edge location( for old browsers)

SNI with CloudFront: If you configure CloudFront to serve HTTPS requests using SNI, CloudFront associates your alternate domain name with an IP address for each edge location. When a viewer submits an HTTPS request for your content, DNS routes the request to the IP address for the correct edge location. The IP address to your domain name is determined during the SSL/TLS handshake negotiation; the IP address isn't dedicated to your distribution.

The SSL/TLS negotiation occurs early in the process of establishing an HTTPS connection. If CloudFront can't immediately determine which domain the request is for, it drops the connection. When a viewer that supports SNI submits an HTTPS request for your content, here's what happens:

- The viewer automatically gets the domain name from the request URL and adds it to a field in the request header.
- **When CloudFront receives the request, it finds the domain name in the request header and responds to the request with the applicable SSL/TLS certificate.**
- The viewer and CloudFront perform SSL/TLS negotiation.
- CloudFront returns the requested content to the viewer.

When you configure CloudFront to serve HTTPS requests using dedicated IP addresses, CloudFront associates your alternate domain name with a dedicated IP address in each CloudFront edge location. When a viewer submits an HTTPS request for your content, here's what happens:

- DNS routes the request to the IP address for your distribution in the applicable edge location.
- **CloudFront uses the IP address to identify your distribution and to determine which SSL/TLS certificate to return to the viewer.**
- The viewer and CloudFront perform SSL/TLS negotiation using your SSL/TLS certificate.
- CloudFront returns the requested content to the viewer.

To require HTTPS between CloudFront and your origin, follow the procedures in this topic to do the following:

- In your distribution, change the Origin Protocol Policy setting for the origin. HTTPS Only or Match Viewer
- Install an SSL/TLS certificate on your origin server (this isn’t required when you use an Amazon S3 origin or certain other AWS origins).



### Origin

Static Content -> S3 \
Dynamic Content -> EC2, ELB

Signed URL and Signed Cookies: 

Use signed URLs in the following cases:

- You want to restrict access to individual files, for example, an installation download for your application.
- Your users are using a client (for example, a custom HTTP client) that doesn't support cookies.

Use signed cookies in the following cases:

- You want to provide access to multiple restricted files, for example, all of the files for a video in HLS format or all of the files in the subscribers' area of website.
- You don't want to change your current URLs.
      

GeoRestriction : can configure approved countried or block banned countries. CloudFront determines the location of your users by using a third-party database.

Invalidation Option

Personalized : HTTP Cookies, Query String parameters

Backup origin: If origin is down, route request to backup.

Restricting access to origin so that users access data only using CF

- S3 : Origin Access Identity
- ELB: Using Custom HTTP Header. 

### Security

- Configure S3, EC2, ELB only to process request from CloudFront
- When using S3 as origin
      - Configure CloudFront to use Origin Access Identity Credentials
      - Configure S3 bucket policy to allow only OAI
- When using Web application as Origin
      - Add Origin Custom Header(COnfigured when u create a CF distribution). Application can process only if header matches
      - Use Lambda@Edge. Sign the Requests. Origin can verify request.
      - Enable Field Level Encryption
- AWS Shield Standard and Advanced
- WAF

Set up OAI \
From CF Distrbution -> Origin -> Select "Yes" for Restrict bucket access -> Select CloudFront Identity -> Grant Read permission and update bucket policy.

### Lambda@Edge

- Customize application behaviour, without impacting origin.
- improve responsiveness
- Request/Response modification
- Verify Credentials at Edge
- Bot detection

Cache Settings can be controlled : in origin, override by lambda@Edge, CF distribution configuration.

Cache Setting in origin(S3) \
- Go to S3 -> Properties -> MetaData -> CacheControl max-age=300

### CloudFront with S3 Origin

https://aws.amazon.com/premiumsupport/knowledge-center/cloudfront-serve-static-website/

1. REST API : Private s3. Enable OAI. \
Limitation: You need to type the home page. Note: You don't need to enable static website hosting on your bucket for this configuration.

2. Public Website: Public S3.The benefit of this approach is S3 will serve the index page as the home page, and you can also configure an error page. \
Limitation: S3 is public. Anyone can access. Only HTTP. Note: enable static website hosting on the bucket.

3. Public Website with Referrer Header : Configure S3 to allow read to requests that hava a specific header. Note: enable static website hosting on the bucket.

Note \

To make your bucket publicly readable, you must disable block public access settings for the bucket and write a bucket policy that grants public read access. 

CloudFront does not support region-specific origins.

## Global Accelerator

ELB -> Regional Load Balancing \
GA -> Cross region load balancing

Global Applications:

CF does not support region specific origin. \

Route 53 allows global applications. Limitation: 
      - DNS records are cached in client and DNS resolvers for TTL.  Assume app deployed in 4 regions. Route 53 route request to nearest application. If that server is down, route 52 redirects to another healthy nearest one. But due to caching this rerouting traffic to new endpoint will be delayed.
      - Infrastructure action results in IP address change. ALB adds and removes nodes. ALB nodes ip address are configured in Route 53. Delay in responding to these scaling actions by applications


Global Accelerator -> Single Entry point for your global applications. **Two Static anycast IPs**. Route 53 for your domain will have only these 2 static IPs.\

Client example.com -> Route 53 -> Returns Static IP og GA. \
Client connect to GA in nearest Edge location -> GA decides how to route request to web server( to nearest server). **All requests use AWS network**.\
GA monitor health of app servers. 

In case of Route 53 , one DNS is resolved, client to web server request goes via Internet.

End point can be private. Eg: ALB is private. GA takes to endpoint using private IP address

Client affinity : Request send to same end point. like sticky sessions

DDos protection at edge.

AWS Shield Integration. **No WAF integartion**

Traffic Dial : Percentage of requests routed between regions.  \
Weight: Traffic to end point. Blue Green Deployment.

Note:

GA is a global resource, but service is configured in Oregon region.

You can hit GA using static IPs or DNS name.


#### Resource Ownership

AWS account is the resource owner ( irrespective of who created the resource)
