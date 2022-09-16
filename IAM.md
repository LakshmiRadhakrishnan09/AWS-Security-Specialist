
## IAM

### Identity
User or system that need access to resources in an AWS account.

* Identity based Policy
* Resource based Policy
* IAM Roles: temporary credentials

U can attch a policy to user, group, resource, role

Identity based Policy
* Attached to a identity
* What **actions** this user is allowed/denied on a **resource**

Resource based Policy
* attched to a resource
* What actions is allowed/denied for a **resource** for a **principal**(iam of the requestor)

* **IAM groups** are not supported as principals in any policy
* Principals : Not required in identity based policy. Needed for resource based policies and IAM role trust policy.
* Principal : Can be an arn of iam user or a service . Or a feratated user(company.com). Or Assumed role session(in another account, sts). 
* Privide account number -- means root user
* Anonymous user -- *

In general Identity policies are recommeneded than resource based policies. Resource based polcies support cross account access.

ARN:
* need region and accoun-id as part of most resouces
* for global resources like iam, s3 skip region
* for s3,skip region and account-id, since s3 is globally unique

* NotAction
* NotResource
* NotPrincipal
* Condition
  * NotIpAddress 
  * StringNotEquals
  * aws:CurrentTime
  * aws:RequestedRegion: region action need to be performed
  * aws:PrincipalTag : tag based access
  * aws:SecureTransport
  * aws:sourceIP
  * aws:sourceVpc
  * aws:sourceVpce
  * ec2:ResourceTag

**Attribute based Access Control** : Tag based. In RBAC(Role based) model , when new resources are added, policy need to be updated.In ABAC, require fewer polcies and scale nicely(eg: cost center matches)

**By default all requests are denied**
**Should be allowed atleast once**
**Explicit Deny overrides explicit allow**
**Permission Boundary, SCM policy, Session polciy may override allow with a deny**

### IAM role

Scanario: EC2 instance accessing S3. 

Attach role to Ec2 instance. Instance use ec2 meta data service to get temporary credentials.No need to maintain long term credentials.

STS: Service responsible for generating temporary credentials.

Role
* Attach access policy to Role. What can this role do
* Trust policy: who can use this role

### Cross account access
* Resource based policy
* IAM roles assume role

What is the effect of specifying a root user or account as the principal in a resource-based policy? Who gains access?
Root will get access. Root can delegate to other users or roles.

There are scenarios when you need to give access to your resources for a third-party account: **IAM Role External ID**. When enabled, in order to assume the role, the third party needs to have the Role ARN, and they also need to provide the External ID value. The assume role will succeed only if the third-party account is a trusted entity and the external ID values match.The use of the external ID prevents the "confused-deputy" problem when the third-party provides similar service to other customers. 


### Shared responsibility

EC2
* U are responsible for updating and patching OS, Firewalls, Security Group, Network ACL
Managed Product- S3
* AWS responsible for Hardware, Software, Scalability



