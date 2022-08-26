Complaince of AWS \
AWS Artifact : Documents from AWS on how they are compliant. \
AWS Config: Monitor complaince status of workloads on real time. CloudTrail log all API logs. AWS Config review keep track of all resources and their state. We can define rules.
You can set lambda functions to remediate this non-compliant issue. How to configure rules across multiple accounts? - AWS Organizations will allow you filter AWS Config rules down to multiple acocunts/Organizations units.
Can be aggregated across region. You can push config rules acorss OU's in Orgs
Conformance Pack for complaince rules from AWS.

Aggregating results:
https://docs.aws.amazon.com/config/latest/developerguide/aggregate-data.html

https://aws.amazon.com/organizations/features/

* AWS IAM 
* AWS VPC
* AWS WAF
* AWS Shield
* Amazon Inspector

Ec2 runs on Virtual machines with Hypervior. If you want any kind of manager you have to run an agent( Amazon Inspector, System Manager, CloudWatch Agent).
Even AWS cannot read about your memory(Security reason: Otherwise from Ec2 --> Hypervisor ---> Another Ec2). U need to install agent. No backdoor. Only frontdoor.

* AWS Secrets
* AWS Secret Manager : Rotate keys. Clients can get new keys from KMS. So clients will not break.


* AWS CloudTrail: Record every API call made it to our account. Will look at all requests. Always turn on. Logs will go S3. S3 bucket is replicated. Will aggrgate to a master security account that only security admins have access. They can be aggregated to a master account/master S3 bucket, that is considered a best practice
* AWS CloudWatch:  
* Amazon EventBridge:

* Incident Manager:
* AWS Lambda: 





