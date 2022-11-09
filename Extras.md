https://www.youtube.com/watch?v=QMBkq6MrT2w

In AWS Security is controlled by
- IAM
- KMS
- VPC


# IAM

"I" -> Authentication. Users and passwords
"AM" -> Authorization. Using roles and permissions

Every AWS Services uses IAM to authenticate and authorize API calls.

**IAM User**: AWS identifies human users. By username and password(Long term credentials). 
**Federated Identities**: If you are in a larger organization, these identities may be coming from active directory.
Someone in your organization(admin) maps employee in active directory to a role in AWS IAM. When you authenticate in AD, then u are mapped to a IAM role. 
This is called Federated Identities. In this case the IAM role will have temporary credentials.

My empid is given access to a role in an AWS account. When I try to connect with my empid, it first authenticates with corporate AD directory(on-premise), then get the mapped IAM role,
generate a token for that IAM role(eg: developer, engineeer).temporary credentials will be set.

**AWS identities for non human ( for services)**: Eg Lambda reading data from DB.Associate an IAM role with services.

An organization may have many AWS accounts. 

Create a Role:

- For AWS Services(for non human process)
- Role for Cross account access
- Web Identity : For federated Human identities
- SAML 2.0 federation ( Corporate Identity) : For federated Human identities


**How authentication works in AWS**: Credentials taken. Request signed with hMac(or hmax ??) signature. Service validates the signature(Authentication). Validates Policies( Authorization). Caller account and Destination Account both need to allow access.

**Policies**
IAM policies attached to IAM principal or identities(users, roles) . What can this identity do? Policy Statement: Effect(Allow or Deny), Action(sqs:*), Resource( arn of resource this policy is applicable)

**Working across accounts**

Two approches:

- Resource Based Policy: IAM policies attached to a resource.Who can access this resource? eg : S3 bucket policy. Policy Statement: Effect, **Principal**( Who can take this action), Action, Resource
- Assume role: Role trust policy attached to an IAM role in destination account. It states who can assume this role. 
  Effect: Allow, Action: stsAssumeRole, Principal: Account B. Source Account A ->  Caller IAM role should allow to assume cross account role in B. 
  
https://aws.amazon.com/premiumsupport/knowledge-center/cross-account-access-s3/  

Scenario: S3 bucket in Account A 

Resource Based Policy: Step1: Create an IAM role in Account B with premission for Account A bucket. ( Keep resource as Bucket in Account A). \
Step2: Bucket Policy of S3 should allow IAM role in Account B.( Use resource as Bucket in Account A and Principal as IAM role in Account B).


Assume Role: Step1: Create IAM role in Account A with permission for bucket. Role's Trust Policy to allow role to be assumed by role in Account B. \
Step2: Create a IAM role in Account B, and grant pemission to assume role in Account A.

Assume role is better, because, it is more centralized. If multiple Buckets are there first approach is difficult.
  
  **AWS Organizations**
  To manage multiple accounts
  
  Master Account owns AWS Organization Service.They pay the bills.
  Organization units under AWS Organization.
  Accounts with OUs.
  AWS Single Signon : Allows mapping to accounts in OUc.
  Use Organizations **Service Control Policy** to control access across all organizations. They deny what people cannot do.
  
  # KMS
  
  - SSE-S3: AES-256. Server Side Encrytption with AWS managed keys. AWS manages the encryption key.
  - AWS-KMS: SSE-KMS. An AWS KMS key in your account. Customer managed key(CMK). Role that Reads S3, should have permission for key as well.
  
  Envelop Encryption: 
  
  # VPC
  
  VPC is bound to a Region.
  Each Region has Availability Zones. VPC Network spans AZ.
  Each AZ has one public subnet and one private subnet.
  Each subnet is non-overlapping ip range.
  Application Load Balancers will be in Public subnet. One ALB in each AZ(??)
  VPCs are normally for one account. You can share VPC between accounts.
  Ec2 instances and RDS in private subnet.
  
  Security Groups
  - Stateful goverance. Reply traffic is allowed.
  
  Routing
  
  Public subnet: Have publicaly routable IPs. Rouing table -> Any local ip : local VPC, any other ip : Internet Gateway
  Private subnet:No direct route to internet.Packet cannot leave VPC. By default only one route which sets route to "local VPC".
  
  Internet Gateway: Attached to VPC. 
  
  NAT Gateway: Is not reachable from internet. But can reach Internet.
  
  Some AWS Services are not in VPC- > S3, CloudWatch Logs. How private subnets reach to them? VPC end points.
  VPC end point configured in ur private subnet allows to configure routes from private subnet to AWS services.
  VPC Endpoint Policy: Attached to a network.
  
  https://www.youtube.com/watch?v=SGntDzEn30s
  
  IAM : Controls who can do what in AWS account.
  
  Security -> Deny by default

An endpoint policy specifies the following information:

- The principals that can perform actions.
- The actions that can be performed.
- The resource on which the actions can be performed.
  
  ## IAM Best Practices
  
  ### Identity and Credential Management
  
  - Create Individual Users
  - Do not use root account. Reduce or Remove root user access(Delete access keys).
  - Configure a strong password policy
  - Rotate credentials regularly. Use **Credentail Report** to audit rotation
  - Enable MFA for priviledged users.( Virtual MFA or Hardware MFA)
  - Use **groups** to manage permissions(instead of managing permission at user level)
  - Grant least access
  - Use IAM role when u want to delegate access across accounts
  - Use IAM role to delegate access within an account. ( Eg: very sensitive actions )
  - Use roles for federated users
  - IAM roles: Short lived credentials
  - IAM role: What it can access, Who can assume it. Permission should be given for resource to assume role
  - Use IAM roles for EC2 instances.
  - Enable CloudTrails to get logs of all API calls. Enable all  regions even in regions u dont operate. Logs are stored in S3. This S3 should not be publicaly accessible.
  - Centralize CloudTrail logs
  - Do not store secrets in code
  
  
  Control access using AWS resource tag. Use **tag-based access control**. When new resources are created, users will have automatic access.
  
  Account Management for organization: Two options : Single account or Multiple accounts
  
  https://www.youtube.com/watch?v=oam8FDNJhbE
  
  ## Security Services
  
  - AWS GuardDuty: 
  - Amazon Inspector: 
  - Amazon Macie:
  - AWS Sheild:
  - AWS WAF:


