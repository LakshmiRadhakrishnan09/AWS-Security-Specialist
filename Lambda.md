What are security headers?

Security headers are a group of headers in the HTTP response from a server that tell your browser how to behave when handling your site’s content. For example, X-XSS-Protection is a header that Internet Explorer and Chrome respect to stop pages loading when they detect cross-site scripting (XSS) attacks. The following is a list of each header we’ll be implementing with a link to more information.

Strict Transport Security

Content-Security-Policy

X-Content-Type-Options

X-Frame-Options

X-XSS-Protection

Referrer-Policy

Lambda@Edge

Lambda@Edge provides the ability to execute a Lambda function at an Amazon CloudFront Edge Location. 

Origin Response – This event is triggered after the origin returns a response to a request. It has access to the response from the origin.
