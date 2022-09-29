### Amazon Macie

Amazon Macie is a data security and data privacy service that uses machine learning (ML) and pattern matching to discover and protect your sensitive data
in **S3**.

Discover sensitive Data -  PII data , protected health data, IP(Intellectual Property)

### AWS Security Hub

Security Hub gives you a comprehensive view of your high-priority **security alerts** and **compliance status** acrossAWS accounts in one place, enabling better visibility to these indicators. 

AWS Security Hub's Foundational Security Best Practices,

### Amazon GuardDuty

### Amazon Access Analyzer

### Amazon Detective

Amazon Detective collects log data from your AWS resources and uses machine learning, statistical analysis, and graph theory to help you **identify the root cause of potential security issues or suspicious activities**

### AWS Managed Services

AWS Managed Services offers two operations plans to meet your needs: 1) AWS Managed Services Accelerate for your new and existing AWS accounts via detective controls, giving you full control and flexibility to use AWS as you always have, and 2) AWS Managed Services Advanced with preventative controls via a change management system within an AWS managed landing zone, which provides a full operational solution and trades some flexibility for increased operational rigor to protect your critical business applications. Customers can select either operations plan on an account by account basis.

AWS Managed Services (AMS) specializes in managing AWS infrastructure and services. AMS **automates common activities such as change requests, monitoring, patch management, security, and backup services, and provides full-lifecycle services to provision, run, and support your infrastructure.**

As an Infrastructure Operator, AMS takes responsibility for deploying a suite of security detective controls and provides a 24/7 first line of response to alerts, using a follow-the-sun model. When an alert is triggered, AMS follows a standard set of automated and manual runbooks to ensure a consistent response. These runbooks are shared with AMS customers during onboarding so they can develop and coordinate response with AMS. AMS encourages the joint execution of security response simulations with customers to develop operational muscle before a real incident occurs.

### AWS Support

AWS Support offers a range of plans that provide access to tools and expertise that support the success and operational health of your AWS solutions. All support plans provide 24/7 access to customer service, AWS documentation, whitepapers, and support forums. If you need technical support and more resources to help plan, deploy, and optimize your AWS environment, you can select a support plan that best aligns with your AWS use case.

## Diff between AWS Support and AWS Managed Service

AWS Support provides a mix of tools and technology, people, and programs designed to proactively help customers optimize performance, lower costs, and innovate faster. AWS Support addresses requests that range from answering best practices questions, guidance on configuration, all the way to break-fix and problem resolution.

AWS Managed Services (AMS) helps enterprises adopt **AWS at scale and operate more efficiently and securely**. We are able to execute operational best practices on behalf of the customer through specialized automations, skills, and experience that are contextual to their environment and applications. We provide proactive, preventative, and detective capabilities that raise the operational bar and help reduce risk without constraining agility, allowing customers to focus on innovation. We extend customers' teams and operational capabilities including monitoring, incident detection, security, patch, backup, and cost optimization.

## AWS Shield

AWS offers customers AWS Shield, which provides a managed **DDoS protection service** that safeguards web applications running on AWS. AWS Shield provides always-on detection and automatic inline mitigations that minimize application downtime and latency, so there is no need to engage AWS Support to benefit from DDoS protection. There are two tiers of AWS Shield: Standard and Advanced.

All AWS customers benefit from the no cost, automatic protections of AWS Shield Standard. AWS Shield Standard defends against most common, frequently occurring network and transport layer DDoS attacks that target your website or applications. When you use AWS Shield Standard with Amazon CloudFront and Amazon Route 53, you receive comprehensive availability protection against all known infrastructure (Layer 3 and 4) attacks.

For higher levels of protection against attacks targeting your web applications running on Amazon Elastic Compute Cloud (Amazon EC2), Elastic Load Balancing (ELB), Amazon CloudFront, and Amazon Route 53 resources, you can subscribe to AWS Shield Advanced. Additionally, AWS Shield Advanced gives you 24/7 access to the AWS DDoS Response Team (DRT). 

## AWS Trusted advisor

## AWS Config Rules

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
GuardDuty identifies suspected attackers through integrated threat intelligence feeds and uses machine learning to detect anomalies in account and workload activity. When a potential threat is detected, the service delivers a detailed security alert to the GuardDuty console and AWS CloudWatch Events. This makes alerts actionable and easy to integrate into existing event management and workflow systems.

**Amazon Macie** – Amazon Macie is an AI-powered security service that helps you prevent data loss
by automatically discovering, classifying, and protecting sensitive data stored in AWS. Amazon Macie uses machine learning to recognize sensitive data such as personally identifiable information (PII) or intellectual property, assign a business value, and provide visibility into where this data is stored and how it is being used in your organization. Amazon Macie continuously monitors data access activity for anomalies, and delivers alerts when it detects a risk of unauthorized access or inadvertent data leaks.
 
**AWS Config Rules** – An AWS Config Rule represents the preferred configurations for a resource and is evaluated against configuration changes on the relevant resources, as recorded by AWS Config. You can see the results of evaluating a rule against the configuration of a resource on a dashboard. Using Config Rules, you can assess your overall compliance and risk status from a configuration perspective, view compliance trends over time, and find which configuration change caused a resource to be out of compliance with a rule.

**AWS Trusted Advisor** – AWS Trusted Advisor is an online resource to help you reduce cost, increase performance, and improve security by optimizing your AWS environment. Trusted Advisor provides
real time guidance to help you provision your resources by following AWS best practices. The full set of Trusted Advisor checks, including CloudWatch Events integration, is available to Business and Enterprise support plan customers.

**Amazon CloudWatch** – Amazon CloudWatch is a monitoring service for AWS Cloud resources and the applications you run on AWS. You can use Amazon CloudWatch to collect and track metrics, collect
and monitor log files, set alarms, and automatically react to changes in your AWS resources. Amazon CloudWatch can monitor AWS resources, such as Amazon EC2 instances, Amazon DynamoDB tables, and Amazon RDS DB instances, as well as custom metrics generated by your applications and services, and any log files your applications generate. You can use Amazon CloudWatch to get system-wide visibility into resource utilization, application performance, and operational health. You can use these insights to react accordingly and keep your application running smoothly.

**AWS Inspector**** – Amazon Inspector is an automated security assessment service that helps improve the security and compliance of applications deployed on AWS. Amazon Inspector automatically assesses applications for **vulnerabilities or deviations from best practices**. After performing an assessment, Amazon Inspector produces a detailed list of security findings prioritized by level of severity. These findings can be reviewed directly or as part of detailed assessment reports which are available through the Amazon Inspector console or API.

**Amazon Detective** – Amazon Detective is a security service that automatically collects log data from your AWS resources and **uses machine learning**, statistical analysis, and graph theory to build a linked set of data that enables you to easily conduct faster and more efficient security investigations. Amazon Detective **can analyze trillions of events from multiple data sources** such as Virtual Private Cloud (VPC) Flow Logs, AWS CloudTrail, and Amazon GuardDuty, and automatically creates a unified, interactive view of your resources, users, and the interactions between them over time. **With this unified view, you can visualize all the details and context in one place to identify the underlying reasons for the findings, drill down into relevant historical activities, and quickly determine the root cause.**

### Automation

**AWS Lambda** –  Lambda runs your code on high-availability compute infrastructure and performs all the administration of the compute resources for you. This includes server and operating system maintenance, capacity provisioning and automatic scaling, code and security patch deployment, and code monitoring and logging. All you have to do is supply the code.

**AWS Step Functions** – AWS Step Functions makes it easy to coordinate the components of distributed applications and microservices using visual workflows. Step Functions **provides a graphical console for you to arrange and visualize the components of your application as a series of steps.** 

**AWS Systems Manager** – AWS Systems Manager gives you visibility and control of your infrastructure on AWS. Systems Manager provides a unified user interface so you can view operational data from multiple AWS services, and enables you to automate operational tasks across your AWS resources. With Systems Manager, you can group resources by application, view operational data for monitoring and troubleshooting, and take action on your groups of resources. Systems Manager can keep your instances in their defined state, perform on-demand changes, such as updating applications or running shell scripts, and perform other automation and patching tasks.


### Secure Storage

**Amazon S3**

**Amazon S3 Glacier** 

### Custom

There are a number of partner tools available, such as those listed in our **APN Security Competency program**. You might also want to write your own queries to search your logs. With the extensive number of managed services that AWS offers, this has never been easier. There are many additional AWS services that can assist you with investigation that are outside the scope of this paper, such as Amazon Athena, Amazon OpenSearch Service, Amazon QuickSight, Amazon Machine Learning, and Amazon EMR.


