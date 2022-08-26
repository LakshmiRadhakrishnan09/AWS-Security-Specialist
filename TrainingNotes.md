### Complaince of AWS 
* AWS Artifact : Documents from AWS on how they are compliant. \
* AWS Config: Monitor complaince status of workloads on real time. CloudTrail log all API logs. AWS Config review keep track of all resources and their state. We can define rules.
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
* Amazon EventBridge: To send the logs outside AWS. Integration with third party SaaS apps. Partner services.. Data Dog..

* Incident Manager:
* AWS Lambda: 

How s3 is 99.99 durable? - Answer: Replication. Every time u put a data in S3, AWS make 5 copies and 3 copies will be in 3 different AZ. 

### Shared Responsibilty Tools
Give you tools. You need to implement it. \
You are responsible for patching AMIs for Ec2. Use system manager for patching EC2 instances. \

An AZ is a collection of data centers( 6 data centers in one AZ). Region has multiple AZ( 10 - 60 miles apart). 1-2 millisecond latency between AZs. AZ-a in Account1 can be different from Az-a in Account2. So if u are working between accounts using AZ-a doesnt menas they refer to same AZ.

Selecting an AWS Region: Complaince, Latency, Cost.

AMI : Organizations take base AMI , harden it using EC2 Builder, and create a new AMI and release it for developers to use.

AWS Complaince Center:https://aws.amazon.com/financial-services/security-compliance/compliance-center/?country-compliance-center-cards.sort-by=item.additionalFields.headline&country-compliance-center-cards.sort-order=asc&awsf.country-compliance-center-master-filter=*all

AWS Complaince Programs: https://aws.amazon.com/compliance/programs/

**AWS will not move data between regions**. AWS move it between AZ. You need to ask AWS to move it between regions.









