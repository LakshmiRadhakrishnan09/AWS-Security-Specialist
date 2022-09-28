

Incident Response

• Educate
• Prepare 
• Simulate
• Iterate

AWS Cloud Adoption Framework(CAF) Security Perspective

The Security Perspective includes four components:
• Directive controls : establish the governance,risk,andcompliancemodelswithinwhichthe environment operates.
• Preventive controls: protect your workloads and mitigate threats and vulnerabilities.
• Detectivecontrols: providefullvisibilityandtransparencyovertheoperationofyourdeploymentsin
AWS.
• Responsivecontrolsdriveremediationofpotentialdeviationsfromyoursecuritybaselines.

Incident Domain

Domains where security incidents may occur

- Service Domain : Affect AWS Account. Issue with configuration or resource permissions
- Infrastrcuture Domain: dataornetwork-related activity
- Application Domain

In these domains, you must consider the actors who might act against your account, resources, or data. 
Whether internal or external, use a risk framework to determine what the specific risks are to your organization and prepare accordingly.

In the service domain, you work to achieve your goals exclusively with AWS APIs. 
For example, handling a data disclosure incident from an Amazon S3 bucket involves API calls to retrieve the bucket's policy, analyzing the S3 access logs, and possibly looking at AWS CloudTrail logs. 
In this example, your investigation is unlikely to involve data forensic tools or network traffic analysis tools.

In the infrastructure domain, you can use a combination of AWS APIs and familiar digital forensics/ incident response (DFIR) software within the operating system of a workstation, such as an Amazon EC2 instance that you've prepared for IR work. 
Infrastructure domain incidents might involve analyzing network packet captures, disk blocks on an Amazon Elastic Block Store (Amazon EBS) volume, or volatile memory acquired from an instance.

To detect security-related events in your AWS Cloud environmen

- Logs and Monitors
- Billing Activity
- Threat Intelligence
- Partner Tools
- AWS outreach
- One Time contact

Prepare access to AWS Account during an incident

Identify and discuss the AWS account strategy and cloud identity strategy with your organization's cloud architects to understand what authentication and authorization methods are configured, for example:
• Federation–AuserassumesanIAMroleinanAWSaccountfromanIdentityProvider.
• Cross-accountaccess–AuserassumesanIAMrolebetweenmultipleAWSaccounts.
• Authentication–AuserauthenticatesasanAWSIAMusercreatedwithinasingleAWSaccount.

• IndirectAccess: appl team perform steps under guidance from security team
• DirectAccess: deploy an AWS IAM role into the AWS accounts that your security engineers or incident responders can assume during a security event.
The incident responder authenticates either through a normal federated process, or through a special emergency process
• AlternativeAccess: By using a new, purpose-built AWS account, your responders can collaborate and work from an alternate, secure infrastructure. 
Use Amazon workspace, Amazon WorkMail and then access inmpacted AWS account from new account.
• AutomationAccess: create IAM roles specifically for your automation resources to use (such as Amazon EC2 instances or AWS Lambda functions). 
These resources can then assume the IAM roles and inherit the permissions assigned to the role. 
consider using the AWS Systems Manager Run Command, which enables you to remotely and securely administrate instances using an agent that you install on your Amazon EC2 instance operating system.


Prepare Processes

• DecisionTrees(p.18)
• UseAlternativeAccounts(p.18)
• VieworCopyData: At a minimum, the IAM permission policy for the responders should provide read-only access so that they can investigate. 
To enable appropriate access, you might consider some pre-built AWS Managed Policies, such as SecurityAudit or ViewOnlyAccess.
• Sharing Amazon EBS Snapshots (p. 19)
• SharingAmazonCloudWatchLogs: Logs that are recorded in Amazon CloudWatch Logs, such as Amazon VPC flow logs, can be shared with another account (such as your centralized security account) through a CloudWatch Logs subscription.
• Use Immutable Storage : Using the native features of Amazon S3, you can configure an Amazon S3 bucket to protect the integrity of your data. For example, by using S3 Object Lock, you can prevent an object from being deleted or overwritten for a fixed amount of time or indefinitely. 
Managing access permissions with S3 bucket policies, configuring S3 versioning, and enabling MFA Delete are other ways to restrict how data can
be written or read. This type of configuration is useful for storing investigation logs and evidence, and is often referred to as write once, read many (WORM). You can also protect the data by using server- side encryption with AWS Key Management Service (AWS KMS) and verifying that only appropriate IAM principals are authorized to decrypt the data.
• LaunchResourcesNeartheEvent: The best practice is to perform investigations and forensics in the cloud, where the data is, rather than attempting to transfer the data to a data center before you investigate.
t is also a best practice to perform the investigation in the same AWS Region where the event occurred, rather than replicating the data to another Region.
• IsolateResources: 
• LaunchForensicWorkstations: Create a AMI with all preconfigured and required tools that are required for analysis.


AWS enhanced networking uses single root I/O virtualization (SR-IOV) to provide high-performance networking capabilities on supported instance types. SR-IOV is a method of device virtualization that provides higher I/O performance and lower CPU utilization when compared to traditional virtualized network interfaces. Enhanced networking provides higher bandwidth, higher packet per second (PPS) performance, and consistently lower inter-instance latencies. There is no additional charge for using enhanced networking. 
