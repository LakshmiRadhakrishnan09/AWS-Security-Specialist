

# AWS Organizations

Consolidated billing. Cental Management of identities. One Master Account(Root level). Many Organization Units. Accounts under OU.Enforce policies at OU and accounts. 
Can enable CloudTrail centally.

Create Accounts
Create OU

Policies at AWS organization(available at master account)
- Service Control Policies
- Tag Policies
- AI services opt out
- Backup policies

## Service Control Policies

Only the actions permitted by both inherited policy and policy that is attached to OU are allowed.

SCPs can be applied at root, OU, or individual account.

```
“You can attach policies to organization entities (organization root, organizational unit (OU), or account) in your organization:

When you attach a policy to the organization root, all OUs and accounts in the organization inherit that policy.

When you attach a policy to a specific OU, accounts that are directly under that OU or any child OU inherit the policy.

When you attach a policy to a specific account, it affects only that account.”  

```

The permissions can be managed using either of the following strategies:

Deny list strategy – here FullAWSAccess policy is attached to the root – so, all accounts are by default permitted all actions. We tighten the control by creating new SCPs that Deny access to specific services or actions. These SCPs are then attached specific OU or account

Allow list strategy – here, the FullAWSAccess policy is removed from the root. So, no API actions are permitted anywhere unless you explicitly allow them. You must create SCPs that allow access to specific services and actions. You need to attach the SCP to the entire hierarchy all the way to the root.

Master account is not affected by SCP. Even for Root user of child accounts policies will be applied.

Use case: Restrict some services to all accounts. Restrct regions to all accounts.

Apply Trail to my organization: Will consolidate activities to a central bucket in master account.

## AWS Sigle Sign on

Single Sign on is available only if u allow AWS organisations

- Configure Sigle Sign on for AWS organisation from master account
    - Specify Identity source ( Can be AWS SSO, on premise Active Directory, External identity provider)
    - SSO provides a personalized UI portal for login - sign in
- Define SSO permission set
    - To manage access to accounts
    - Based on job functions - Administrator, Read only
    - Permissions are created at account level
- Manage users and groups
    - EgL LabAdmin, ProductionAdmin, ReadGroup
- COnfigure access to member accounts

AWS SSO users are in AWS SSO not in AWS IAM.


## AWS Control Tower

Guidance for multi account environment.Blueprint for Security and Compliance Best Practices.

Root Account - Parent account \
Security OU
  - Audit Account
  - Log Archive Account
 SandBox OU
  - Dev/Prod OU


Single Sign on enabled. \
20 mandatory preventative guardrails. Implemented using SCPs. \
  - Disallow changes to log archieval
Optional guardrail
  - MFA for users
2 detective guradrails. Using AWS Config. \

Account Factory for provisioning new accounts. Configurable template. Specify network config, region selection, service catalog.. 

Can be set up for existing AWS org and new AWS org.

AWS recommendation
- Multiple accounts
- Multiple OUS
    - Infrastrcuture OU - shared infra
    - SandBox OU
    - Workload OU
        - Production OU
        - Staging OU


### Resource Sharing across accounts

- Resource Access Manager : Share with any AWS account or organizations
       - App Mesh, Aurora, VPC subnets, EC2 images
- Transit Gateway : for network resources
        - Routes IP traffic(layer3)
        - Attach VPCs, VPN, Direct Gateway, Transit Gateway in another region
        - Route traffic through security products(proxies)







Note: In AWS no to accounts use same email Id.