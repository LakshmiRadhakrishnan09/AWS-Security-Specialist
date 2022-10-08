

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

### Origin

Static Content -> S3 \
Dynamic Content -> EC2, ELB

Signed URL and Signed Cookies

GeoRestriction

Invalidation Option

Personalized : HTTP Cookies, Query String parameters

Backup origin

### Security

- Configure S3, EC2, ELB only to process request from CloudFront
- When using S3 as origin
      - Configure CloudFront to use Origin Access Identity Credentials
      - Configure S3 bucket policy to allow only OAI
- When using Web application as Origin
      - Add Custom Header. Application can process only if header matches
      - Use Lambda@Edge. Sign the Requests. Origin can verify request.
      - Enable Field Level Encryption
- AWS Shield Standard and Advanced
- WAF

### Lambda@Edge

- Customize application behaviour, without impacting origin.
- improve responsiveness
- Request/Response modification
- Verify Credentials at Edge
- Bot detection






