

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

### Origin

Static Content -> S3 \
Dynamic Content -> EC2, ELB

Signed URL and Signed Cookies

GeoRestriction

Invalidation Option

Personalized : HTTP Cookies, Query String parameters

Backup origin: If origin is down, route request to backup.

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


