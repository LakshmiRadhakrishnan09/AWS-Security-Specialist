

Incident Response

• Educate
• Prepare 
• Simulate
• Iterate

AWS Cloud Adoption Framework(CAF) Security Perspective

The Security Perspective includes four components:

• Directive controls : establish the governance,risk,and compliance models within which the environment operates.
• Preventive controls: protect your workloads and mitigate threats and vulnerabilities.
• Detective controls: provide full visibility and transparency over the operation of your deployments in AWS.
• Responsive controls: drive remediation of potential deviations from your security baselines.

Incident Domain

Domains where security incidents may occur

- Service Domain : Affect AWS Account. Issue with configuration or resource permissions
- Infrastrcuture Domain: data or network-related activity
- Application Domain: Ur application

In these domains, you must consider the actors who might act against your account, resources, or data. 
Whether internal or external, use a risk framework to determine what the specific risks are to your organization and prepare accordingly.

In the service domain, you work to achieve your goals exclusively with AWS APIs. 
For example, handling a data disclosure incident from an Amazon S3 bucket involves API calls to retrieve the bucket's policy, analyzing the S3 access logs, and possibly looking at AWS CloudTrail logs. 
In this example, your investigation is unlikely to involve data forensic tools or network traffic analysis tools.

In the infrastructure domain, you can use a combination of AWS APIs and familiar digital forensics/ incident response (DFIR) software within the operating system of a workstation, such as an Amazon EC2 instance that you've prepared for IR work. 
Infrastructure domain incidents might involve analyzing network packet captures, disk blocks on an Amazon Elastic Block Store (Amazon EBS) volume, or volatile memory acquired from an instance.

To detect security-related events in your AWS Cloud environment

- Logs and Monitors
- Billing Activity
- Threat Intelligence
- Partner Tools
- AWS outreach
- One Time contact

Prepare access to AWS Account during an incident

Identify and discuss the AWS account strategy and cloud identity strategy with your organization's cloud architects to understand what authentication and authorization methods are configured, for example:

- Federation– A user assumes an IAM role in an AWS account from an IdentityProvider.
- Cross-account access– A user assumes an IAM role between multiple AWS accounts.
- Authentication– A user authenticates as an AWS IAMuser created with in a single AWSaccount.

- IndirectAccess: appl team perform steps under guidance from security team
- DirectAccess: deploy an AWS IAM role into the AWS accounts that your security engineers or incident responders can assume during a security event.
The incident responder authenticates either through a normal federated process, or through a special emergency process
- AlternativeAccess: By using a new, purpose-built AWS account, your responders can collaborate and work from an alternate, secure infrastructure. 
Use Amazon workspace, Amazon WorkMail and then access inmpacted AWS account from new account.
- AutomationAccess: create IAM roles specifically for your automation resources to use (such as Amazon EC2 instances or AWS Lambda functions). 
These resources can then assume the IAM roles and inherit the permissions assigned to the role. 

consider using the AWS Systems Manager Run Command, which enables you to remotely and securely administrate instances using an agent that you install on your Amazon EC2 instance operating system.


Prepare Processes

- DecisionTrees(p.18)
- UseAlternativeAccounts(p.18)
- VieworCopyData: At a minimum, the IAM permission policy for the responders should provide read-only access so that they can investigate. 
To enable appropriate access, you might consider some pre-built AWS Managed Policies, such as SecurityAudit or ViewOnlyAccess.
- Sharing Amazon EBS Snapshots (p. 19)
- SharingAmazonCloudWatchLogs: Logs that are recorded in Amazon CloudWatch Logs, such as Amazon VPC flow logs, can be shared with another account (such as your centralized security account) through a CloudWatch Logs subscription.
- Use Immutable Storage : Using the native features of Amazon S3, you can configure an Amazon S3 bucket to protect the integrity of your data. For example, by using S3 Object Lock, you can prevent an object from being deleted or overwritten for a fixed amount of time or indefinitely. 
Managing access permissions with S3 bucket policies, configuring S3 versioning, and enabling MFA Delete are other ways to restrict how data can be written or read. This type of configuration is useful for storing investigation logs and evidence, and is often referred to as write once, read many (WORM). You can also protect the data by using server- side encryption with AWS Key Management Service (AWS KMS) and verifying that only appropriate IAM principals are authorized to decrypt the data.
- Launch Resources Near the Event: The best practice is to perform investigations and forensics in the cloud, where the data is, rather than attempting to transfer the data to a data center before you investigate.
It is also a best practice to perform the investigation in the same AWS Region where the event occurred, rather than replicating the data to another Region.
- Isolate Resources: 
- Launch Forensic Workstations: Create a AMI with all preconfigured and required tools that are required for analysis.


AWS enhanced networking uses single root I/O virtualization (SR-IOV) to provide high-performance networking capabilities on supported instance types. SR-IOV is a method of device virtualization that provides higher I/O performance and lower CPU utilization when compared to traditional virtualized network interfaces. Enhanced networking provides higher bandwidth, higher packet per second (PPS) performance, and consistently lower inter-instance latencies. There is no additional charge for using enhanced networking. 

Options for automated response


* AWS Lambda : System using AWS Lambda only, using your organizations enterprise language. Speed Flexibility Maintenance Skillset
* AWS Step Functions : System using AWS Step Speed Functions, Lambda, and SSM Agent.Speed Flexibility Maintenance Skillset 
* Auto Remediation with AWS Config Rules: Maintenance & Skillset Speed & Flexibility
* SSM Agent : Maintenance & Skillset Speed Flexibility
* AWS Fargate: Flexibility Speed Maintenance & Skillset
* Amazon EC2: Flexibility Speed Maintenance Skillset

For example, AWS Lambda offers more speed and requires less technical skillset. AWS Fargate offers more flexibility, and requires less maintenance and technical skillset.

Centralization refers to a central account that drives all of the detection and remediation for an organization. This approach may seem like the best choice out-of-the-box, and it is the current best practice.We encourage you to get started by leveraging the approach of the Security Tooling account in the Multi-Account Framework in AWS Organizations or AWS Control Tower.

Cost for automation is determined by
- Events per second (EPS) is the metric that you use to best estimate cost
- Scanning mode for anomaly detection- event based and time based

For example, Amazon EC2 and AWS Fargate have the highest costs for running 0-10 EPS, whereas AWS Lambda and AWS Step Functions have the highest costs for running 76+ EPS.

In many cases, a combination of both scanning approaches is most likely the best choice in a fully mature organization. The AWS Security Hub and AWS Foundational Security Best Practices standard provide a combination of both scanning methods.

IR for EC2 instance 

1. Capture the metadata from the Amazon EC2 instance, before you make any changes to your environment.
2. Protect the Amazon EC2 instance from accidental termination by **enabling termination protection** for the instance.
3. Isolate the Amazon EC2 instance by switching the VPC Security Group. However, be aware of VPC connection tracking and other containment techniques.
4. Detach the Amazon EC2 instance from any AWS Auto Scaling groups.
5. Deregister the Amazon EC2 instance from any related Elastic Load Balancing service.
6. Snapshot the Amazon EBS data volumes that are attached to the EC2 instance for preservation and follow-up investigations.
8. Tag the Amazon EC2 instance as quarantined for investigation, and add any pertinent metadata, such as the trouble ticket associated with the investigation.

To capture volatile data and do investigation

Although you could authenticate directly to the machine using a standard method (such as Linux secure shell (SSH) or Microsoft Windows remote desktop (RDP)), manual interaction with the operating system is not a best practice. We recommend that you programmatically use an automation tool to execute tasks on a host.

**The AWS Systems Manager Run Command helps you to remotely and securely perform on-demand changes running Linux shell scripts and Windows PowerShell commands on a targeted instance.**

Automating the Capture

One method to invoke the SSM Agent is to target the Run Command through Amazon CloudWatch Events when the instance is tagged with a specific tag. For example, if you apply the Response=Isolate+MemoryCapture tag to an affected instance, you can configure Amazon CloudWatch Events to trigger two actions:
* A Lambda function that performs the isolation activities
* A Run Command that executes shell command to export the Linux memory through the SSMAgent
**This tag-driven response is another method of event-driven response.**


Cloud Services for various activities


* LoggingandEvents
    -  CloudTrail
    -  CloudWatch Logs
    - CloudWatch Events
* VisibilityandAlerting
    - Security Hub
    - GuardDuty
    - Macie
    - Trusted Advisor
    - AWS Config Rules
    - Inspector
    - Detective
* Automation
    - System Manager
    - Lambda
* SecureStorage
    - S3
* Custom(p.44)
