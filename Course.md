
Web -- App -- DB \

Web(Public Subnet). APP, DB ( Private Subnet)

To be resilient: Application should be spread across multiple AZ. Use ELB for web server in public subnet. ELB for app server in private subnet. 
IS ELB across multiple AZ? \
RDS in 3 AZ ( Primary, Secondary and StandBy) \
Auto Scaling Group:  Dynamic(Target, Step and Simple), Fixed, Scheduled \
Database Scaling: Read replicas \

Decouple layers: WEB - Queue - App -DB \
WEB - SNS - Queue(multiple. fan out) - App -DB \

Certificates at ELB. \
Authentication at ALB. ( With Cognito + SAML + OpenID Connect ) \
WAF at ALB \
Secret Manager \
AWS Config for configuration drift \
Inspector for server vulnerabilities \
Trusted advisor - best advices
Systems manager - for applying patch
Monitoring - CloudWatch \
Auditing - CloudTrail

Serverless implementation: API Gateway - Quque - Lambda - RDS \
Step FUnctions to orchestrate workflows.
WAF at API Gateway

Serverless :pay for what u use. Server based Solution: U need to pay for idle solutions. 

Route 53: For custom domains and routing




