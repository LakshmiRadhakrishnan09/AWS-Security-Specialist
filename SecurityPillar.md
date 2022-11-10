
For federation to multiple accounts in your AWS Organization, you can configure your identity source in **AWS IAM Identity Center** (successor to AWS Single Sign-On) (IAM Identity Center), and specify where your users and groups are stored. Once configured, your identity provider is your source of truth, and information can be synchronized using the **System for Cross-domain Identity Management (SCIM) v2.0 protocol**. You can then look up users or groups and grant them single sign-on access to AWS accounts, cloud applications, or both.

For example, when using IAM Identity Center as the identity source, configure the “context-aware” or “always-on” setting for MFA, and allow users to enroll their own MFA devices to accelerate adoption. When using an external identity provider (IdP), configure your IdP for MFA.

For machine identities, you should rely on IAM roles to grant access to AWS. For EC2 instances, you can use roles for Amazon EC2. You can attach an IAM role to your EC2 instance to enable your applications running on Amazon EC2 to use temporary security credentials that AWS creates, distributes, and
rotates automatically through the **Instance Metadata Service (IMDS)**. The latest version of IMDS helps protect against vulnerabilities that expose the temporary credentials and should be implemented. For accessing EC2 instances using keys or passwords, **AWS Systems Manager** is a more secure way to access and manage your instances using a pre-installed agent without the stored secret. Additionally, other AWS services, such as AWS Lambda, enable you to configure an IAM service role to grant the service permissions to perform AWS actions using temporary credentials. In situations where you cannot use temporary credentials, use programmatic tools, such as AWS Secrets Manager, to automate credential rotation and management.


Best practices:

Do not use IAM user. Use federated users. Configure SSO using Identity Center.

For AWS resources use role.

Use Session Manager of AWS Systems Manager. Do not use Bastion Host.

Use Secret Manager.

We strongly recommend that you never put confidential or sensitive information, such as your customers' email addresses, into tags or free-form fields such as a Name field. Any data that you enter into tags or free-form fields used for names may be used for billing or diagnostic logs.

Resource Groups & Tag Editor: Helps to perform bulk actions.

Examples of bulk actions include:

Applying updates or security patches.
Upgrading applications.
Opening or closing ports to network traffic.
Collecting specific log and monitoring data from your fleet of instances.