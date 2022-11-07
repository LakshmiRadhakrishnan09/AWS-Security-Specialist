

### How Microsoft AD works?

Centallized Domain Controller with user/password details. Other Windows machines can now use this information.Logins synchronized across windows servers.

ADFS : Single Signon 

ADFS checks with Microsoft AD. Get SAML token passed to AWS.

<img width="755" alt="Screenshot 2022-10-31 at 4 30 12 PM" src="https://user-images.githubusercontent.com/33679023/200356456-7f01e72a-df88-43ee-ace2-bb96a6946f33.png">


### AWS Directory Services

- AWS Managed Microsoft AD: AD both in on-premise and cloud
- AD Connector: AD only in on-premise. Proxy requests to on-premise AD.
- Simple AD: Standalone. No integration

AWS Managed Microsoft AD: Create AD in cloud. Establish "trust" connection with on premise AD.

Onpremise AD --- Trust ---- AWS AD

Users are defined in both on-premise and cloud(can be prsent either in one AD). Users can be authenticated against AWS AD or On-premise.

It can be a Standalone AD or can be joined with onpremise AD.
Can be used for AWS SSO.
Can be used for RDS for SQL Servers

For on-premise AD Integration: \
- Should have a direct connect or VPN connection.
- 3 kinds of forest trust
      - One way, AWS to On-premise
      - One way, On-premise to AWS
      - Two way.

AD Connector

Proxy to redirect to On-premise AD. 

Onpremise AD --- Proxy ---- AD Connector.

Users only in on-premise AD.Users are authenticated against AWS AD Connector

Dont work with SQL Server.

Simple AD

Standalone. Cannot be joined to AD. 

How to set up replication of on-premise AD to Cloud? \
Install Microsoft AD on EC2 install. Set up relication.
Establish trust between EC2 and AWS Managed Microsoft AD.

A company is migrating to the cloud with employee credentials managed on-premises.  They need a simple setup to manage identities and grant required access in AWS.  There are two thousand employees in the organization.  Which option should they use?

Active Directory Connector is a proxy service that is easy to set up and eliminates the need for complex hosting infrastructure.  It forwards all AD requests to the on-premises Active Directory.  AD Connector Small supports up to 500 users, and Large supports up to 5000 users. Simple AD is a standalone directory and is not suitable for federation. AWS Managed Microsoft Active Directory is a full-featured Microsoft Active Directory, and this will also meet the needs. However, it is a more complex solution than the AD Connector.
