### Web Application Firewall (WAF)

Protects from web application vulnerabilities(OWASP 20, CVV). WAF can block requests with suspicious content( Injection attacks, Server Side Request Forgery). Application Layer Protection(**layer7**). **Protect web application from common exploits.** Monitors https, https. WAF can be integrated with **ALB, CloudFront, API Gateway**.**AWS WAF does not support NLB.** 

WAF + CloudFront -> block cache bursting attacks

Visibility with cloudwatch metrics. Can log header and other parameters.

- Configure Web Access Control List(rules and rules group).
- Associate ACL with resources(CloudFront, ALB..)

Managed Rule Groups - provided by AWS. for OWASP Top 10, CVE, IP Restriction list, Ip Anonymous list( proxies, VPN, ). Automatically updated.

By default ALB doesnt protect you from Injection attacks. U need to associate WACL to ALB and then it prevent Injection and other attacks.

AWS WAF does not support NLB. One option is to configure CloudFront distribution with NLB as the origin and associate AWS WAF to the CloudFront distribution.

### Firewall Manager

Administraction and maintaince task in multiple accounts( Manage WAF rules, Shield Advanced, Security Groups across accounts).

Centally maintain Firewall rules. 

Findings are send to Security Hub

### AWS System Manager(SSM) and Patch Manager

For parching systems at scale. Centrally manage all servers. Need to install a agent in each server. **OS and software patches**. Run commands on a fleat of instances without login into server. **Session Manager**. IAM based access to servers. No need to use SSH, Bastion Hosts, RDP. AWS Config Integration.

Parameter Store: To store passwords and configuration data. Secret Manager stores everything encrypted. Parameter store - u need to encrypt and store. Parameter Store supports three parameter types: String, StringList and SecureString

Maintain consistent configuration.Automation of administrative tasks.

App Config: Manage application configuration

Quick Setup -> Configure for all accounts or one account.

Need two roles.One role for SSM. One role for EC2 to communicate with SSM. \

AmazonSSMManagedInstanceCore, CloudWatchAgentServerPolicy

Collect Inventory. OS used. applications running. You can find out how many instances are using a particular version of software. \
Instances that are compliant

Chhose all instances, or specific instances by tags. 

SSM can manage on-premise servers. On-premise need to have a certificate(instaed of IAM role) + Activation ID

Session Manager : to connect

Distributor: Install additional agents

Patch Manager: Configure Custom Patching. Select Patch Baseline for OS. Configure Schedule.

### Amazon Macie

Amazon Macie is a data security and data privacy service that uses machine learning (ML) and pattern matching to discover and protect your sensitive data in **S3**.

Discover sensitive Data -  PII data , protected health data, IP(Intellectual Property)

* Sensitive Data Finding
* Policy Finding : When Bucket Policy is changed. Bucket Encryption disabled.

### Amazon GuardDuty

Threat Detection Service: detect malicious and abnormal activities. unusual activities

It is difficult to monitor log trials and events manually. Guarduty automatically monitors it for you. GuardDuty monitors CloudTrail(Event Logs, Management events, S3 Data Events), VPC flow logs, DNS Logs.

GuardDuty is regional. Need to be enabled in each region.

GuardDuty in N.Virgia for Global resources.

Report suspicious activity in reions u do not use.

Currently GuardDuty generate findings for **EC2, IAM and S3**.

SEVERITY \
HIGH -> Immediate Action \
MEDIUM -> Investigate \
LOW -> Indication

Can configure Trusted IPs. Known Malicious IPs.

Supression Findings -> Exfilteration reported due to on-premiese AWS connectivity for gateway. Bastion Host.

GuardDuty -> EventBridge Integration. GuarDuty just report findings. You need to take action.

GuarDuty and AWS Organization for multi account. \
Nominate one of the member as GuardDuty admin account. This Admin account can manage configuration for all accounts. \
Do not use organization master account or root account. \
Need to be delegated for each region. Repeat for every region. \

### Amazon IAM Access Analyzer

AM Access Analyzer helps you identify the resources in your organization and accounts, such as Amazon S3 buckets or IAM roles, **shared with an external entity.** This lets you identify unintended access to your resources and data, which is a security risk.

For each instance of a resource shared outside of your account, IAM Access Analyzer generates a finding. 

Resources shared to another account outside of AWS organizations generates a finding.

Only for services that support Resource based policy - S3 bucket, IAM roles, KMS keys, Lambda functions, SQS queus, Secret Manager.

You enable IAM access analyzer for your entire organization or your account. The organization or account you choose is known as the zone of trust for the analyzer. The analyzer monitors all of the supported resources within your zone of trust.f you add a new policy , or change an existing policy, IAM Access Analyzer analyzes the new or updated policy within about 30 minutes.

Each finding includes details about the resource, the external entity with access to it, and the permissions granted so that you can take appropriate action.

IAM Access Analyzer analyzes only policies applied to resources in the same AWS Region where it's enabled. To monitor all resources in your AWS environment, you must create an analyzer to **enable IAM Access Analyzer in each Region** where you're using supported AWS resources.

### AWS Inspector

Security vulnerabilities of EC2 instance

Host assessment: Compare installed OS and software against CVE list. Also evaluate against CIS benchmark. Evaluate against AWS best practices. Need an agent(If instance is managed by SSM, Inspector can run commands). 

Network assessment : Network reachability assessment. Identify Ports that are reachable. Processes listening of port. Agent not required.Optional agent.

Schedule for weekly once or Run once. Takes 1 hr to run assessment. 

U can trigger assessment as response to an event ( someone changes SG).

Rule packages: Security best practices, RunTime Bahaviour Analysis, Common vulnerabilities, CIS OS Security Configuration Bechnmarks.

For EC2 and containers in ECS.

* Package vulnerability of software(CVE)
* Network Reachability( only for EC2 but checks various network configuration of VPC)

### Network Firewalls and Route 53 Resolver DNS Firewall

Block suspicious packages

Block DNS queries

## AWS Shield

Standard: Infrastructure layer protection(layer 3 and 4). For all customers. Free

Advanced: 24/7 DDos response team. Visibility and repoting. DDos auto scaling cost protection.Support plan. Includes WAF


AWS offers customers AWS Shield, which provides a managed **DDoS protection service** that safeguards web applications running on AWS. AWS Shield provides always-on detection and automatic inline mitigations that minimize application downtime and latency, so there is no need to engage AWS Support to benefit from DDoS protection. There are two tiers of AWS Shield: Standard and Advanced.

All AWS customers benefit from the no cost, automatic protections of AWS Shield Standard. AWS Shield Standard defends against most common, frequently occurring **network and transport layer DDoS attacks** that target your website or applications. When you use AWS Shield Standard with Amazon CloudFront and Amazon Route 53, you receive comprehensive availability protection against all known infrastructure (**Layer 3 and 4**) attacks.

For higher levels of protection against attacks targeting your web applications running on Amazon Elastic Compute Cloud (Amazon EC2), Elastic Load Balancing (ELB), Amazon CloudFront, and Amazon Route 53 resources, you can subscribe to AWS Shield Advanced. Additionally, AWS Shield Advanced gives you 24/7 access to the AWS DDoS Response Team (DRT). 

Layer 3 attack - Eg: UDP reflection attack. Overwhelm the system with flood of large packets. \
Layer 4 - Eg: SYN Flood. Attacker sends large number of SYNs. Connection half open. Never send ACK. \



## AWS Config Rules

Continuously monitor all resources.

Eg: Access keys rotated periodically, Unused EBS volume, unused Elastic IP, Check if multi-az configured for RDS.

Can aggrgate data from multiple accounts. 

Alert when changes are detected.

Monitor resources in a region for an account.

Remediation : Using SSM document.

Timeline of changes.

**AWS Config Aggregator**: Multi-Account Multi-Region Data Aggregation.
"An aggregator is an AWS Config resource type that collects AWS Config configuration and compliance data from the following:

Multiple accounts and multiple regions. Single account and multiple regions. An organization in AWS Organizations and all the accounts in that organization.

Use an aggregator to view the resource configuration and compliance data recorded in AWS Config."


### AWS Security Hub

Security Hub gives you a comprehensive view of your high-priority **security alerts** and **compliance status** acrossAWS accounts in one place, enabling better visibility to these indicators. 

Integrated with AWS Security Hub's Foundational Security Best Practices.

Consolidates threats and prioritize from GuradDuty, Macie, IAM access analyzer, Firewall Manager, Inspector, Patch Manager.

Problem with all these tools: \
All these tools are regional. No global-view. 

**Security Hub and Amazon Detective attempt to do samething -> Consolidate View**

Security Hub requires that AWS Config is enabled in all accounts that have Security Hub enabled. Security Hub controls use AWS Config rules to complete security checks.

You need to enable Security Hub . Security standards lists the security standards that Security Hub supports. You can select the Standads to enable. AWS Config must be configured to record at a minimum the resources that are required for the standards that you have enabled.

- AWS Config resources required for CIS controls
- AWS Config resources required for PCI DSS controls
- AWS Config resources required for AWS Foundational Security Best Practices controls

### Amazon Detective

Amazon Detective collects log data from your AWS resources and uses machine learning, statistical analysis, and graph theory to help you **identify the root cause of potential security issues or suspicious activities**

Visualize interation.

**Security Hub reports. Detective to find root cause.** 

**GuardDuty should be enabled atleast for last 48 hours**

Analyze CloudTrail, VPC flow Logs and GuardDuty Findings. Integrates with GuardDuty and SecurityHub.

Create behaviour Graph. Memeber accounts need to be invited.

Search by IP address, Role

## AWS Trusted advisor

Do Benchmarking. Global resource.

Scans and compare infrastructure against AWS best practices in 5 areas - Cost optimization, Performance, Security, Fault Tolerance, Service Limits.

All customers have access to 7 core checks.

S3 with public access, SG with ports access to SSH to everyone, No MFA for root account, EBS snapshots that are public, Compare Service Limit

Business and Enterprise Plan - Full check.

Can be configured to run weekly.

The AWS Trusted Advisor service provides four checks at no additional charge to all users, including three important security checks: specific ports unrestricted, IAM use, and MFA on root account. When you sign up for Business- or Enterprise-level AWS Support, you receive full access to all Trusted Advisor checks.


### AWS Managed Services

AWS Managed Services offers two operations plans to meet your needs: 1) AWS Managed Services Accelerate for your new and existing AWS accounts via detective controls, giving you full control and flexibility to use AWS as you always have, and 2) AWS Managed Services Advanced with preventative controls via a change management system within an AWS managed landing zone, which provides a full operational solution and trades some flexibility for increased operational rigor to protect your critical business applications. Customers can select either operations plan on an account by account basis.

AWS Managed Services (AMS) specializes in managing AWS infrastructure and services. AMS **automates common activities such as change requests, monitoring, patch management, security, and backup services, and provides full-lifecycle services to provision, run, and support your infrastructure.**

As an Infrastructure Operator, AMS takes responsibility for deploying a suite of security detective controls and provides a 24/7 first line of response to alerts, using a follow-the-sun model. When an alert is triggered, AMS follows a standard set of automated and manual runbooks to ensure a consistent response. These runbooks are shared with AMS customers during onboarding so they can develop and coordinate response with AMS. AMS encourages the joint execution of security response simulations with customers to develop operational muscle before a real incident occurs.


Logging:

- AMS aggregated service Logs: Generated by AWS services
- AMS shared service logs: 
- Amazon EC2 system level logs: Generated by OS

Monitoring:

- Performance degradation
- Application health down





### AWS Support

AWS Support offers a range of plans that provide access to tools and expertise that support the success and operational health of your AWS solutions. All support plans provide 24/7 access to customer service, AWS documentation, whitepapers, and support forums. If you need technical support and more resources to help plan, deploy, and optimize your AWS environment, you can select a support plan that best aligns with your AWS use case.

## Diff between AWS Support and AWS Managed Service

AWS Support provides a mix of tools and technology, people, and programs designed to proactively help customers optimize performance, lower costs, and innovate faster. AWS Support addresses requests that range from answering best practices questions, guidance on configuration, all the way to break-fix and problem resolution.

AWS Managed Services (AMS) helps enterprises adopt **AWS at scale and operate more efficiently and securely**. We are able to execute operational best practices on behalf of the customer through specialized automations, skills, and experience that are contextual to their environment and applications. We provide proactive, preventative, and detective capabilities that raise the operational bar and help reduce risk without constraining agility, allowing customers to focus on innovation. We extend customers' teams and operational capabilities including monitoring, incident detection, security, patch, backup, and cost optimization.


## How to use these services

Alerts are generated by AWS Config, AWS Security Hub Fundational Security Best Practices, AWS Trusted Advisor

Amazon GuardDuty and Access Analyzer -> Application findings??

Amazon Inspector and Amazon Macie -> data and end point concerns

Findings by -> Amazon GuardDuty , Access Analyzer , Amazon Inspector and Amazon Macie

Security Hub -> unify those findings(from Amazon GuardDuty , Access Analyzer , Amazon Inspector and Amazon Macie) into one place and react to them in concert with low latency, which is why it is suggested as a central location for remediation

To act to these events - u can use eithet CloudWatch Events that trigger Lambda or Security Hub.(Event Driven Response)

However, the ability **to build custom insights and add your own findings from the application domain** adds to the weighty reasons to use Security Hub instead.

## APN Partners


## From Documentation

### Logging and Events

**AWS CloudTrail** – AWS CloudTrail is a service that enables governance, compliance, operational **auditing**, and risk auditing of your AWS account. With CloudTrail, you can log, continuously monitor, and retain account activity related to actions across your AWS infrastructure. CloudTrail provides event history of your AWS account activity, including actions taken through the AWS Management Console, AWS SDKs, command line tools, and other AWS services. This event history simplifies security analysis, resource change tracking, and troubleshooting.


Validated log files are invaluable in security and forensic investigations. To determine whether a log
file was modified, deleted, or unchanged after CloudTrail delivered it, you can use **CloudTrail log file integrity validation**. This feature is built-in using industry standard algorithms: SHA-256 for hashing and SHA-256 with RSA for digital signing. This makes it computationally infeasible to modify, delete or forge CloudTrail log files without detection.

By default, the log files delivered by CloudTrail to your bucket are encrypted by Amazon server-side encryption. You can optionally use the AWS Key Management Service (AWS KMS) managed keys (SSE- KMS) for your CloudTrail log files.

**Amazon CloudWatch Events** – Amazon CloudWatch Events delivers **a near real-time stream of system events** that describe changes in AWS resources, or when API calls are published by AWS CloudTrail. Using simple rules that you can quickly set up, you can match events and route them to one or more target functions or streams. CloudWatch Events becomes aware of operational changes as they occur. CloudWatch Events can respond to these operational changes and take corrective action as necessary, by sending messages to respond to the environment, activating functions, making changes, and capturing state information. Some security services, such as Amazon GuardDuty, produce their output in the form of CloudWatch Events.

**AWS Config** – AWS Config is a service that enables you to assess, audit, and evaluate the configurations of your AWS resources. Config continuously monitors and records your AWS resource configurations and enables you to automate the evaluation of recorded configurations against desired configurations. With Config, you can review changes in configurations and relationships between AWS resources, manually or automatically. You can review detailed resource configuration histories, and determine your overall compliance against the configurations specified in your internal guidelines. This enables you to simplify compliance auditing, security analysis, change management, and operational troubleshooting.
      
**Amazon S3 Access Logs** – If you store sensitive information in an Amazon S3 bucket, you can enable S3 access logs to record every upload, download, and modification to that data. This log is separate from, and in addition to, the CloudTrail logs that record changes to the bucket itself (such as changing access policies and lifecycle policies).

**Amazon CloudWatch Logs** – You can use Amazon CloudWatch Logs to monitor, store, and access your log files (such as your operating system, application, and custom log files) from your Amazon Elastic Compute Cloud (Amazon EC2) instances using the CloudWatch Logs agent. Additionally, Amazon CloudWatch Logs can capture logs from AWS CloudTrail, Amazon Route 53 DNS Queries, VPC Flow Logs, Lambda functions, and other sources. You can then retrieve the associated log data from CloudWatch Logs.

**Amazon VPC Flow Logs** – VPC flow logs enable you to capture information about the IP traffic going
to and from network interfaces in your VPC. After you've created a flow log, you can view and retrieve its data in Amazon CloudWatch Logs. VPC flow logs can help you with a number of tasks. For example, you can use flow logs to troubleshoot why specific traffic is not reaching an instance, which can help you diagnose overly restrictive security group rules. You can also use flow logs as a security tool to monitor the traffic to your instance.

**AWS WAF Logs** – AWS WAF now supports full logging of all web requests that are inspected by the service. You can store these logs in Amazon S3 for compliance and auditing needs, as well as to use them for debugging and additional forensics. These logs help you to understand why certain rules are triggered and why certain web requests are blocked. You can also integrate the logs with your SIEM and log analysis tools.

Other AWS Logs – With the pace of innovation, we continue to deploy new features and capabilities for customers practically every day, which means that there are dozens of AWS services that provide logging and monitoring capabilities. For information about the features available for each AWS service, see the AWS documentation for that service.

### Visibility and Alerting

**AWS Security Hub** – AWS Security Hub gives you a comprehensive view of your high-priority security alerts and compliance status across AWS accounts. With Security Hub, you have a single place that aggregates, organizes, and prioritizes your security alerts or findings from multiple AWS services, such as Amazon GuardDuty, Amazon Inspector, and Amazon Macie, as well as from AWS Partner solutions. Your findings are visually summarized on integrated dashboards with actionable graphs and tables. You can also continuously monitor your environment using automated compliance checks based on the AWS best practices and industry standards your organization follows.

**Amazon GuardDuty** – Amazon GuardDuty is a managed threat detection service that continuously monitors for malicious or unauthorized behavior to help you protect your AWS accounts and workloads. It monitors for activity such as unusual API calls or potentially unauthorized deployments that
indicate a possible account compromise. GuardDuty also detects potentially compromised instances or reconnaissance by attackers.
**GuardDuty identifies suspected attackers through integrated threat intelligence feeds and uses machine learning to detect anomalies in account and workload activity**. When a potential threat is detected, the service delivers a detailed security alert to the GuardDuty console and AWS CloudWatch Events. This makes alerts actionable and easy to integrate into existing event management and workflow systems.

**Amazon Macie** – Amazon Macie is an AI-powered security service that helps you prevent data loss
by automatically discovering, classifying, and protecting sensitive data stored in AWS. Amazon Macie uses machine learning to recognize sensitive data such as personally identifiable information (**PII**) or intellectual property, assign a business value, and provide visibility into where this data is stored and how it is being used in your organization. Amazon Macie continuously monitors data access activity for anomalies, and delivers alerts when it detects a risk of unauthorized access or inadvertent data leaks.
 
**AWS Config Rules** – An AWS Config Rule represents the preferred configurations for a resource and is evaluated against configuration changes on the relevant resources, as recorded by AWS Config. You can see the results of evaluating a rule against the configuration of a resource on a dashboard. Using Config Rules, you can assess your overall compliance and risk status from a configuration perspective, view compliance trends over time, and find which configuration change caused a resource to be out of compliance with a rule.

**AWS Trusted Advisor** – AWS Trusted Advisor is an online resource to help you reduce cost, increase performance, and improve security by optimizing your AWS environment. Trusted Advisor providesreal time guidance to help you provision your resources by following **AWS best practices**. The full set of Trusted Advisor checks, including CloudWatch Events integration, is available to Business and Enterprise support plan customers.

**Amazon CloudWatch** – Amazon CloudWatch is a monitoring service for AWS Cloud resources and the applications you run on AWS. You can use Amazon CloudWatch to collect and track metrics, collect and monitor log files, set alarms, and automatically react to changes in your AWS resources. Amazon CloudWatch can monitor AWS resources, such as Amazon EC2 instances, Amazon DynamoDB tables, and Amazon RDS DB instances, as well as custom metrics generated by your applications and services, and any log files your applications generate. You can use Amazon CloudWatch to get system-wide visibility into resource utilization, application performance, and operational health. You can use these insights to react accordingly and keep your application running smoothly.

**AWS Inspector**** – Amazon Inspector is an automated security assessment service that helps improve the security and compliance of applications deployed on AWS. Amazon Inspector automatically assesses applications for **vulnerabilities or deviations from best practices**. After performing an assessment, Amazon Inspector produces a detailed list of security findings prioritized by level of severity. These findings can be reviewed directly or as part of detailed assessment reports which are available through the Amazon Inspector console or API.

**Amazon Detective** – Amazon Detective is a security service that automatically collects log data from your AWS resources and **uses machine learning**, statistical analysis, and graph theory to build a linked set of data that enables you to easily conduct faster and more efficient security investigations. Amazon Detective **can analyze trillions of events from multiple data sources** such as Virtual Private Cloud (VPC) Flow Logs, AWS CloudTrail, and Amazon GuardDuty, and automatically creates a unified, interactive view of your resources, users, and the interactions between them over time. **With this unified view, you can visualize all the details and context in one place to identify the underlying reasons for the findings, drill down into relevant historical activities, and quickly determine the root cause.**

How does Amazon Detective differ from Amazon GuardDuty and AWS Security Hub?

Amazon GuardDuty is a threat detection service that continuously monitors for malicious activity and unauthorized behavior to protect your AWS accounts and workloads. With AWS Security Hub, you have a single place that aggregates, organizes, and prioritizes your security alerts, or findings, from multiple AWS services, such as Amazon GuardDuty, Amazon Inspector, and Amazon Macie, as well as from AWS Partner solutions. Amazon Detective simplifies the process of investigating security findings and identifying the root cause. Amazon Detective analyzes trillions of events from multiple data sources such as Amazon VPC Flow Logs, AWS CloudTrail logs, Amazon EKS audit logs, and Amazon GuardDuty findings and automatically creates a graph model that provides you with a unified, interactive view of your resources, users, and the interactions between them over time.

Amazon GuardDuty: detects abnormal behaviour \
Amazon Detective : provides detailed summaries, analysis, and visualizations of the behaviors and interactions amongst your AWS aresources. Root Cause analysis \
Security Hub: Single place for all findings


### Automation

**AWS Lambda** –  Lambda runs your code on high-availability compute infrastructure and performs all the administration of the compute resources for you. This includes server and operating system maintenance, capacity provisioning and automatic scaling, code and security patch deployment, and code monitoring and logging. All you have to do is supply the code.

**AWS Step Functions** – AWS Step Functions makes it easy to coordinate the components of distributed applications and microservices using visual workflows. Step Functions **provides a graphical console for you to arrange and visualize the components of your application as a series of steps.** 

**AWS Systems Manager** – AWS Systems Manager gives you visibility and control of your infrastructure on AWS. Systems Manager provides a unified user interface so you can view operational data from multiple AWS services, and enables you to automate operational tasks across your AWS resources. With Systems Manager, you can group resources by application, view operational data for monitoring and troubleshooting, and take action on your groups of resources. Systems Manager can keep your instances in their defined state, perform on-demand changes, such as updating applications or running shell scripts, and perform other automation and patching tasks.


### Secure Storage

**Amazon S3**

**Amazon S3 Glacier** 

### Custom

There are a number of partner tools available, such as those listed in our **APN Security Competency program**. You might also want to write your own queries to search your logs. With the extensive number of managed services that AWS offers, this has never been easier. There are many additional AWS services that can assist you with investigation that are outside the scope of this paper, such as Amazon Athena, Amazon OpenSearch Service, Amazon QuickSight, Amazon Machine Learning, and Amazon EMR.

### AWS Artifact 

AWS Artifact is your go-to, central resource for compliance-related information that matters to you. It provides on-demand access to AWS’ security and **compliance reports** and select online agreements. Reports available in AWS Artifact include our Service Organization Control (SOC) reports, Payment Card Industry (PCI) reports, and certifications from accreditation bodies across geographies and compliance verticals that validate the implementation and operating effectiveness of AWS security controls. Agreements available in AWS Artifact include the Business Associate Addendum (BAA) and the Nondisclosure Agreement (NDA).

### IPTables, Windows Firewall, Host/Instance Based Firewall

Can be used for for unique requirements that are not easily met by AWS provided firewalls. Eg: Allow traffic only to www.abc.com. Using SG and NACL, we can limit only be CIDR block.

### Secrets Manager

Rotate credentials. Integrates with RDS and Redshift. Automatically rotate credentials without impacting application. In other cases, use lambda for credential rotation.  



Note:

With CloudFront whitlelist query paramater to prevent cache bursting attacks.

To manage private instances using SSM : Create systems manager endpoints for three services: ssm, ssmmessages and ec2messages.  The endpoint security group must allow 443 https traffic from your instance to the endpoint.

AWS Inspector can automatically scan Container Images stored in Elastic Container Registry (ECR). This is better than Basic Scanning provided by ECR.

Data Classification
* Tier1 : Information for Internal use only
* Tier2: Restrcited Data
* Tier3: Highly Strategic ( IP, Tradesecrets)


Amazon EventBridge

- One Default event bus in created when u create AWS account. You can create other event bus if required
- Serverless event bus
- to build event driven applications at scale
- U can set up rules. These rules are evaluated against events. Remain in event bus and evaluated by other rules.
- Decouple applications ( Difference from SQS : Event remains event after successful consumption)
- to Automate response and remediation
- respond automatically to system events such as application availability issues or resource changes.
- Security Hub automatically sends all new findings and all updates to existing findings to EventBridge as EventBridge events. 
- Events from AWS services are delivered to EventBridge in near-real time and on a guaranteed basis. You can write simple rules to indicate which events you are interested in and what automated actions to take when an event matches a rule. We can trigger lambda, EC2 run command, SNS.
- When an event occurs in EventBridge, CloudTrail records the event in Event history. ( Similar to other AWS services)
- Configuring an EventBridge rule for automatically sent findings. You can create a rule in EventBridge, that defines an action to take when a Security Hub Findings - Imported event is received. Security Hub Findings - Imported events are triggered by updates from both BatchImportFindings and BatchUpdateFindings.
- Findings fron Security Hub automatically send to EventBridge. U write rules in EventBridge
- U can create custom actions in EventBridge.

AWS Config

An AWS Config rule represents your desired configuration settings for specific AWS resources or for an entire AWS account. If a resource does not pass a rule check, AWS Config flags the resource and the rule as noncompliant, and AWS Config notifies you through Amazon SNS.

Confiig evaluation is triggered When: \
Configuration changes – AWS Config triggers the evaluation when any resource that matches the rule's scope changes in configuration. The evaluation runs after AWS Config sends a configuration item change notification.
Periodic – AWS Config runs evaluations for the rule at a frequency that you choose (for example, every 24 hours).


An aggregator is a new resource type in AWS Config that collects AWS Config configuration and compliance data from **multiple source accounts and regions.** Create an aggregator in the region where you want to see the aggregated AWS Config configuration and compliance data.

Aggregators provide a read-only view into the source accounts and regions that the aggregator is authorized to view. 


AWS GuardDuty

GuardDuty can be configured to use the following types of lists.

Trusted IP list:Trusted IP lists consist of IP addresses that you have trusted for secure communication with your AWS infrastructure and applications. GuardDuty does not generate VPC flow log or CloudTrail findings for IP addresses on trusted IP lists. You can include a maximum of 2000 IP addresses and CIDR ranges in a single trusted IP list. At any given time, you can have **only one uploaded trusted IP list per AWS account per Region.**


Threat IP list: Threat lists consist of known malicious IP addresses. This list can be supplied by third-party threat intelligence or created specifically for your organization. In addition to generating findings because of a potentially suspicious activity, GuardDuty also generates findings based on these threat lists. You can include a maximum of 250,000 IP addresses and CIDR ranges in a single threat list. GuardDuty only generates findings based on activity that involves IP addresses and CIDR ranges in your threat lists, findings will not be generated based of domain names. At any given time, you can have up to **six uploaded threat lists per AWS account per Region.**

Note
If you include the same IP on both a trusted IP list and threat list it will be **processed by the trusted IP list first**, and will not generate a finding.

Security Group supports a maximum of 1000 IP entries, whereas NACL allows only 40 entries.  WAF supports 10,000 entries per IP set, and you can use multiple IP sets.
