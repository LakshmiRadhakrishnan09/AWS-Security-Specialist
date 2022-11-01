## Types of KMS Keys

|Type of KMS key.         | Can view KMS key metadata| Can manage KMS key|Used only for my AWS account| Automatic rotation        | Pricing
**Customer managed key**	        Yes	                  Yes	                Yes	          Optional. Every year (approximately 365 days) Monthly fee (pro-rated hourly) . Per-use fee

**AWS managed key**	          Yes	                  No	                   Yes	          Required. Every year (approximately 365 days). No monthly fee. Per-use fee (some AWS services pay this fee for you)

**AWS owned key**	             No	                  No	                    No	                 Varies	                     No fees


Customer Managed Keys: **Customer managed keys are KMS keys in your AWS account that you create, own, and manage. You have full control over these KMS keys**, including establishing and maintaining their key policies, IAM policies, and grants, enabling and disabling them, rotating their cryptographic material, adding tags, creating aliases that refer to the KMS keys, and scheduling the KMS keys for deletion.

For customer managed key, u can decide how key is generated. 3 options : KMS, External, Custome Key Store(CloudHSM). 
            - KMS - KMS will create key. Optional 1 year key rotation. Only master key is rotated.
            - External - Onpremise key generation. Need to rotate the keys manually.
            - CloudHSM- Cluster of server. Need to rotate the keys manually.

AWS managed keys are KMS keys in your account that are created, managed, and used on your behalf by an AWS service integrated with AWS KMS to protect your resources in the service.**You don't have to create or maintain the key or its key policy**.You have permission to view the AWS managed keys in your account, view their key policies, and audit their use in AWS CloudTrail logs. However, **you cannot change any properties of AWS managed keys, rotate them, change their key policies, or schedule them for deletion.** And, you cannot use AWS managed keys in cryptographic operations directly; the service that creates them uses them on your behalf.**All AWS managed keys are automatically rotated every year. You cannot change this rotation schedule.** In general, unless you are required to control the encryption key that protects your resources, an AWS managed key is a good choice. Starts with aws/<service>. Eg: aws/ebs, aws/dynamodb. One key per service. 
            
 AWS owned keys. You don't need to create or maintain the key or its key policy.AWS owned keys are not in your AWS account. The rotation of AWS owned keys varies across services. In general, unless you are required to audit or control the encryption key that protects your resources, an AWS owned key is a good choice.The rotation of AWS owned keys varies across services. Default DynamoDB key cannot be seen by customer. These keys are shared among multiple customers.

## S3 Encryption at Rest

- Server-Side Encryption with Amazon S3-Managed Keys (SSE-S3)
            - Is a AWS Owned Key.
            - No fee
            - Key rotation handled automatically. No control.
            - Any user having S3 read access can read. No key permission needed.
- Server-Side Encryption with KMS keys Stored in AWS Key Management Service (SSE-KMS)
            - To upload an object encrypted with an AWS KMS key to Amazon S3, you need **kms:GenerateDataKey** permissions on the key. To download an object encrypted with an AWS KMS key, you need **kms:Decrypt** permissions. 
            - Can use AWS Managed Key or Customer Managed Key
            - Amazon S3 supports only symmetric encryption KMS keys and not asymmetric keys.
            - All GET and PUT requests for AWS KMS encrypted objects must be made using (SSL) or (TLS). Requests must also be signed using valid credentials, such as AWS Signature Version 4 (or AWS Signature Version 2)
            - If your object uses SSE-KMS, don't send encryption request headers for GET requests and HEAD requests, or you’ll get an HTTP 400 BadRequest error.
- Server-Side Encryption with Customer-Provided Keys (SSE-C)
            - you manage the encryption keys and Amazon S3 manages the encryption, as it writes to disks, and decryption, when you access your objects.
            - Amazon S3 does not store the encryption key you provide. 
            
 SSE-S3: Rotation managed by AWS.  /
 SSE-KMS( AWS Managed): aws/s3 : Mandatory rotation every year. You cannot change it /
 SSE-KMS( Customer Managed) : Rotation is not enabled by default. But if you enable it, it will be automatically rotated every 1 year. /
 SSE-KMS(Import Key Material) : No auotomatic key rotation /


If you need server-side encryption for all of the objects that are stored in a bucket.
```
{
  "Version": "2012-10-17",
  "Id": "PutObjectPolicy",
  "Statement": [
    {
      "Sid": "DenyIncorrectEncryptionHeader",
      "Effect": "Deny",
      "Principal": "*",
      "Action": "s3:PutObject",
      "Resource": "arn:aws:s3:::awsexamplebucket1/*",
      "Condition": {
        "StringNotEquals": {
          "s3:x-amz-server-side-encryption": "AES256"
        }
      }
    }
  ]
}

```

```
"Condition":{
            "StringNotEquals":{
               "s3:x-amz-server-side-encryption":"aws:kms"
            }
         }
 ```
 
 ```
 "Condition": {
                "Null": {
                    "s3:x-amz-server-side-encryption-customer-algorithm": "true"
                }
            }
 ```

**Amazon S3 Bucket Keys**

When you configure server-side encryption using AWS KMS (SSE-KMS), you can configure your bucket to use **S3 Bucket Keys for SSE-KMS**. Using a bucket-level key for SSE-KMS can reduce your AWS KMS request costs by up to 99 percent by decreasing the request traffic from Amazon S3 to AWS KMS.
            
When you use SSE-KMS to protect your data without an S3 Bucket Key, Amazon S3 uses an individual AWS KMS data key for every object. It makes a call to AWS KMS every time a request is made against a KMS-encrypted object.When you configure your bucket to use an S3 Bucket Key for SSE-KMS, AWS KMS generates a bucket-level key that is used to create unique data keys for new objects that you add to the bucket. This S3 Bucket Key is used for a time-limited period within Amazon S3, reducing the need for Amazon S3 to make requests to AWS KMS to complete encryption operations. This reduces traffic from S3 to AWS KMS, allowing you to access AWS KMS-encrypted objects in S3 at a fraction of the previous cost.

* Symmetric Encryption: Same Key for encryption and decryption. AES 256.( 256 bits). Is fast.
* Asymmetric encryption or public-key encryption: Public key and private key. Encrypt with one key and decrypt with another key. RSA, ECC, DH. Computation expensive. 
    * Used primarly for securley exchanging symmetric key over internet: TLS/SSL over HTTPS. Used once in the handshake process.
    * Digital Signature, used in CloudTrail log integrity validation

### Envelop Encryption

Plain Text --- Encrypted using **Data Key**   --- Encrypted Text \
Data Key   --- Encrypted using **Master Key** --- Encrypted Data Key \
Envelop --- > Encrypted Text + Encrypted Data Key \
Master Key is stored in KMS Service.


Benefits: If you use data key only, then u can use same data key for all data. If you use master key, u can use different data key for each data. 
Compromise of one data key will not result in compromise of other data.


#### S3-Server side encryption
- KMS will have master key
- S3 asks KMS to generate data key
- KMS returns data key, encrypted data key
- Master key never leaves KMS
- S3 used data key for AES 256 encryption

Master key is identified by keyId, Key arun, alias, alias arn. **Alias** is used for rotating and it points to current master key. \
Encrypted data key has reference to master key ID. So even in case master key is rotated, it refers to old master key. \
Application code use alias. So no need to update application even if key is rotated.

Each object has different data key.

#### EBS Encryption

Optional feature u can enable when u create a volume. An unencrypted volume cannot be enabled with encryption directly. U need to create snapshot first.U can enable encryption while **copying** snapshot or while **creatig new volume from snapshot**. Inorder to change encryption also same. (Note: While taking snapshot u cannot specify encryption)

- EBS calls KMS service to get data key.
- The encrypted data key is stored in the volume.
- Use single data key for the volume. Enables high performance.
- When we attach volume to EC2 instance, the identity of user who is attaching volume is used to get data key from KMS.
- The EC2 host keep the data key in memory till host is stopped
- All data moving betwwen ec2 and volume is encrypted
- data at rest is encrypted

Unencrypted Volume --- Snapshot is Unencrypted --- Restored Volume is Unencrypted \
Encrypted Volume by key A---- Snapshot is Unencrypted by Key A--- Restored Volume is Unencrypted by Key A\

To change encryption: 
   - Unencrypted Volume --- Unencrypted SnapShot --- Enable Encryption while Restoring --- encrypted volume
   - Unencrypted Volume --- Unencrypted SnapShot --- Enable Encryption while Snapshot copy --- encrypted volume
   
To change encryption key: 
   - Encrypted Volume by Key A--- encrypted SnapShot by Key A --- Encrypt with Key B --- encrypted volume by key B
   - Encrypted Volume by Key A--- encrypted SnapShot by Key A --- Encrypt with key B while Snapshot copy --- encrypted volume by key B
   - Above process automatically decrypt with key A and encrypt with key B

Across Region:
   - KMS key is a regional resource
   - When u copy snapshot to a new region, use new  key in destination region
   - Snapshot automatically decrypt and encrypt using new key

EBS Encryption by default: Regional level setting. Any new EBS volume is automatically encypted using configured master key.Snapshot copies are automatically encrypted.

Note: By default for ebs encryption u can use AWS managed key aws\ebs. Or u can create a customer managed key.
            
When you configure a KMS key as the default key for EBS encryption, the default KMS key policy allows any IAM user with access to the required KMS actions to use this KMS key to encrypt or decrypt EBS resources. You must grant IAM users permission to call the following actions in order to use EBS encryption:

kms:CreateGrant: To grant use of KMS key by a user or role. /
kms:Decrypt
kms:DescribeKey
kms:GenerateDataKeyWithoutPlainText
kms:ReEncrypt            
 
To follow the principle of least privilege, do not allow full access to kms:CreateGrant. Instead, allow the user to create grants on the KMS key only when the grant is created on the user's behalf by an AWS service, as shown in the following example.
            
```
            {
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": "kms:CreateGrant",
            "Resource": [
                "arn:aws:kms:us-east-2:123456789012:key/abcd1234-a123-456d-a12b-a123b4cd56ef"
            ],
            "Condition": {
                "Bool": {
                    "kms:GrantIsForAWSResource": true
                }
            }
        }
    ]
}
```

#### RDS Encryption

Optional feature

- Volumes, logs, snapshots, backups are encrypted
- Read replica is encrypted with same key in same region
- Read replica is encrypted with different key in case of cross region replication
- Encrypting by snapshot work similar to EBS

RDS also supports Transparent Data Encryption (TDE) for SQL Server (Enterprise Edition) and Oracle (with Advanced security option in Enterprise Edition).
. Transparent Data Encryption in Oracle is integrated with AWS CloudHSM.

You can encrypt communication between your application and the database using SSL/TLS. “Amazon RDS creates an SSL certificate and installs the certificate on the DB instance when the instance is provisioned”
You would need to download the public key and use it to encrypt connection to the database.

#### DynamoDB Encryption

- Encrypted at rest
- You can change encryption key of existing tables.

#### Key Rotation

AWS Managed Keys: automatically rotated every year. /
AWS Owned Keys: no visibility to these keys and rotation frequency varies. /
Customer Managed Keys (CMK): 
   * KMS - Key generated by KMS. Optionally enable automatic-rotation - rotated every year. If you need to rotate key material at any other frequency (like monthly, quarterly, and so forth), you can rotate them manually.
   * External Key Origin : Import customer owned key generation material. **cannot be rotated automatically, and you need to rotate them manually**
   * CloudHSM: CloudHSM **cannot rotate the key material automatically, and you need to rotate them manually**

#### KMS API

KMS Encrypt, Decrypt, ReEncrypt are for **small data** like data keys, passwords, personel identifiers and other sensitive data.  
            
### KMS Key types 
Symmetric encryption keys, symmetric HMAC keys, asymmetric encryption keys, and asymmetric signing keys.    
- Symmetric encryption KMS key: Ideal for most cases.
- HMAC KMS key (symmetric):to generate and verify hash-based message authentication codes.
- Asymmetric KMS key: use for encryption and decryption or signing and verification
            

#### HMAC (Hash-Based Message Authentication Code) Keys

Normal hash algorithms are susceptible to brute-force attacks. Normal hash algoritm dont use a secret key.  HMAC improves by incorporating a secret key. 
They work like digital signatures but use the same key for both signing and verification.

·  The max message size is 4KB for GenerateMAC, VerifyMAC

· HMAC Keys do not support automatic key rotation or imported key material

· HMAC keys are currently not supported in CloudHSM (custom key store)

#### KMS Multi Key Region

KMS keys are regional.

Cross region replication different region: AWS will decrypt and re-encrypt with a different key for destination region. So, in case of server side encryption you were able to access the same data in different regions even though they were encrypted with different single-region keys. 

MultiRegion Key scenarios: 

* Some times application encrypt data before storing in database. Client Side Encryption. The keys used for this are stored in KMS. 
 - During Disaster Recovery, application may need to use data from different region. 
      Region1 --- Appl --- Key1(Region1)--- DataBase \
      Region2 --- Appl --- How to use key1? --- Backup DataBase \
Solution: Multi-Region Key

* Global applications that need to access data from multiple regions. 
   App --- Database(Region1) \
       --- Database(Region2) \
Solution:    Multi-Region Key

* Active-Active applications with client side encryption. For eg: DynamoDB Global Tables. \
  App --- Database(Region1) \
              |
              |
              |
           Auto Sync
              |
              |
              |
  App2 ---Database(Region2) 

* Distributed Signing :if you use certificate chaining with a single global trust store for a single root certification authority (CA), and regional CAs signed by the root CA, you don't need multi-region keys. However, if your system doesn't support intermediate CAs, you can use multi-region keys to bring consistency
      
you cannot convert your existing single region key to a multi-region key.
CloudHSM (custom key store) currently does not support multi-region keys; however, they do support backup and restore.


KMS Keys : If user has access to read bucket

- SSE-S3 : No protection as S3 automatically decrypt the message
- SSE-KMS : One layer of protection. User should have access to key
- SSE-C: Cannot read data without key
- Client Side: Can read data but cannot decrypt

SSE-S3: Key is generated and managed by AWS. Data is encrypted before saving and decrypted when u request it.

SSE-KMS: Customer Managed Keys(CMKs) stored in AWS KMS. There are two types of CMKs.

- Awazon managed CMK : Key generated by AWS. Stored in AWS account of customer

- Customer managed CMK: Key is genereted by customer. But stored in AWS account of customer.

SSE with Customer-Provided Keys (SSE-C): Key is generated by customer and stored by customer. A client has to send the encryption key along with the object to be uploaded in a request. S3 then encrypts the object using the provided key and the object is stored in S3. Note that the encryption key is deleted from the system.When the user wants to download or retrieve the object it has to supply the encryption key in the request. S3 first verifies that it is the correct encryption key, after the successful match it decrypts the object and returns it to the Client.

Note:
AWS KMS is replacing the term customer master key (CMK) with AWS KMS key and KMS key. The concept has not changed. To prevent breaking changes, AWS KMS is keeping some variations of this term.
The KMS keys that you create are customer managed keys. AWS services that use KMS keys to encrypt your service resources often create keys for you. KMS keys that AWS services create in your AWS account are AWS managed keys. KMS keys that AWS services create in a service account are AWS owned keys.
            
Manual rotation scenarios
            - External Key
            - CloudHSM Keys
            - KMS key different schedule than supported 1 year
            - Asymettric keys

### Encryption Context
EncryptionContext provides three benefits:

Additional authenticated data (AAD)
Audit trail
Authorization context
           
authenticated encryption with associated data (AEAD): You can think of this as extending the signature over the ciphertext to cover additional data as well. In general, AAD should not contain any secret information, but should be contextual information used to understand the secret information.
            
 EncryptionContext is KMS’s implementation of AAD.Data that is commonly used for AAD might include header information, unencrypted database fields in the same record, file names, or other metadata. It’s important to remember that EncryptionContext should contain only nonsensitive information because it is stored in plaintext JSON files in AWS CloudTrail and can be seen by anyone with access to the bucket containing the information.

To control access to your KMS keys, you can use the following policy mechanisms.
           - Key policy - Every KMS key has a key policy. Key policy can be configured for allowing key administrators, key users, services that can use key.(Same can be achieved by IAM policy or Grant)
           - IAM policies. IAM policies are particularly useful for controlling access to operations, such as CreateKey, that can't be controlled by a key policy because they don't involve any particular KMS key.
           - Grants . Grants are often used for temporary permissions because you can create one, use its permissions, and delete it without changing your key policies or IAM policies. Grant User and Grantee. We can revoke a Grant using GrandId.
                        - Grant User is the one who generate the grant. GrantToken is like a secret token with certain permission.
                        - Grantee. Who use the Grant. Dont have any KMS access.But can use GrantToken
KMS keys belong to the AWS account in which they were created. However, no identity or principal, including the AWS account root user, has permission to use or manage a KMS key unless that permission is explicitly provided in a key policy, IAM policy or grant. 
            
Unless the key policy explicitly allows it, you cannot use IAM policies to allow access to a KMS key. Without permission from the key policy, IAM policies that allow permissions have no effect.
            
Allows access to the AWS account and enables IAM policies
```            
            {
  "Sid": "Enable IAM policies",
  "Effect": "Allow",
  "Principal": {
    "AWS": "arn:aws:iam::111122223333:root"
   },
  "Action": "kms:*",
  "Resource": "*"
}
```            
 Allows key administrators to administer the KMS key: Key administrators have permissions to manage the KMS key, but do not have permissions to use the KMS key in cryptographic operations. 
 ```          
           
            {
  "Sid": "Allow access for Key Administrators",
  "Effect": "Allow",
  "Principal": {"AWS": [
    "arn:aws:iam::111122223333:user/KMSAdminUser",
    "arn:aws:iam::111122223333:role/KMSAdminRole"
  ]},
  "Action": [
    "kms:Create*",
    "kms:Describe*",
    "kms:Enable*",
    "kms:List*",
    "kms:Put*",
    "kms:Update*",
    "kms:Revoke*",
    "kms:Disable*",
    "kms:Get*",
    "kms:Delete*",
    "kms:TagResource",
    "kms:UntagResource",
    "kms:ScheduleKeyDeletion",
    "kms:CancelKeyDeletion"
  ],
  "Resource": "*"
}
```
            
How AWS Services use KMS Key
            
AWS services that are integrated with AWS KMS use only symmetric encryption KMS keys to encrypt your data. These services do not support encryption with asymmetric KMS keys. 
            
Grants are commonly used by AWS services that integrate with AWS KMS to encrypt your data at rest. The service creates a grant on behalf of a user in the account, uses its permissions, and retires the grant as soon as its task is complete.            
            
 A key can have only one key policy. Can have multiple Grants.     \
 Grant supports limited operations [CreateGrant, GenerateDataKey, RetireGrant, Encrypt, ReEncryptTo, Decrypt, GenerateDataKeyWithoutPlaintext, DescribeKey, Verify, ReEncryptFrom, Sign]           
            
Deleting KMS Key
- Once u delete the key, it is irreversible. So better disable the key
- You can only schedule the deletion of a customer managed key. You cannot delete AWS managed keys or AWS owned keys.  
- You cannot enable or disable AWS managed keys or AWS owned keys. 
- To determine who uses key - Examining AWS CloudTrail logs to determine actual usage            
- AWS KMS requires you to set a waiting period of 7 – 30 days. The default waiting period is 30 days.
- You can configure an Amazon CloudWatch alarm to warn you if a person or application attempts to use the KMS key during the waiting period. 
- You can delete the imported key material from a KMS key at any time. AWS KMS deletes the key material immediately, the key state of the KMS key changes to pending import, and the KMS key can't be used in any cryptographic operations.  However, these actions do not delete the KMS key. To use the KMS key again, you must reimport the same key material into the KMS key. In contrast, deleting a KMS key is irreversible.           
            
To Limit Key by Service
```
            {
  "Effect": "Allow",
  "Principal": {
    "AWS": "arn:aws:iam::111122223333:user/ExampleUser"
  },
  "Action": [
    "kms:Encrypt",
    "kms:Decrypt",
    "kms:ReEncrypt*",
    "kms:GenerateDataKey*",
    "kms:CreateGrant",
    "kms:ListGrants",
    "kms:DescribeKey"
  ],
  "Resource": "*",
  "Condition": {
    "StringEquals": {
      "kms:ViaService": [
        "ec2.us-west-2.amazonaws.com",
        "rds.us-west-2.amazonaws.com"
      ]
    }
  }
}
```           
