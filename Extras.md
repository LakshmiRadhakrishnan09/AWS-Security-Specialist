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
- Resource Based Policy: IAM policies attached to a resource.Who can access this resource? eg : S3 bucket policy. Policy Statement: Effect, **Principal**( Who can take this action), Action, Resource
- Assume role: Role trust policy attached to an IAM role in destination account. It states who can assume this role. 
  Effect: Allow, Action: stsAssumeRole, Principal: Account B. Source Account A ->  Caller IAM role should allow to assume cross account role in B. 
  
  **AWS Organizations**
  To manage multiple accounts
