## Types of KMS Keys

|Type of KMS key.         | Can view KMS key metadata| Can manage KMS key|Used only for my AWS account| Automatic rotation        | Pricing
**Customer managed key**	        Yes	                  Yes	                Yes	          Optional. Every year (approximately 365 days) Monthly fee (pro-rated hourly) . Per-use fee

**AWS managed key**	          Yes	                  No	                   Yes	          Required. Every year (approximately 365 days). No monthly fee. Per-use fee (some AWS services pay this fee for you)

**AWS owned key**	             No	                  No	                    No	                 Varies	                     No fees


Customer Managed Keys: **Customer managed keys are KMS keys in your AWS account that you create, own, and manage. You have full control over these KMS keys**, including establishing and maintaining their key policies, IAM policies, and grants, enabling and disabling them, rotating their cryptographic material, adding tags, creating aliases that refer to the KMS keys, and scheduling the KMS keys for deletion.

For customer managed key, u can decide how key is generated. 3 options : KMS, External, Custom Key Store(CloudHSM). 
            - KMS - KMS will create key. Optional 1 year key rotation. Only master key is rotated.
            - External - Onpremise key generation. Need to rotate the keys manually.
            - CloudHSM- Cluster of server. Need to rotate the keys manually.

AWS managed keys are KMS keys in your account that are created, managed, and used on your behalf by an AWS service integrated with AWS KMS to protect your resources in the service.**You don't have to create or maintain the key or its key policy**.You have permission to view the AWS managed keys in your account, view their key policies, and audit their use in AWS CloudTrail logs. However, **you cannot change any properties of AWS managed keys, rotate them, change their key policies, or schedule them for deletion.** And, you cannot use AWS managed keys in cryptographic operations directly; the service that creates them uses them on your behalf.**All AWS managed keys are automatically rotated every year. You cannot change this rotation schedule.** In general, unless you are required to control the encryption key that protects your resources, an AWS managed key is a good choice. Starts with aws/<service>. Eg: aws/ebs, aws/dynamodb. One key per service. 
            
 AWS owned keys. You don't need to create or maintain the key or its key policy. AWS owned keys are not in your AWS account. The rotation of AWS owned keys varies across services. In general, unless you are required to audit or control the encryption key that protects your resources, an AWS owned key is a good choice.The rotation of AWS owned keys varies across services. Default DynamoDB key cannot be seen by customer. These keys are shared among multiple customers.

 KMS will not rotate data key.

 Key rotation change only key material. Key Id , arn, policies will not change.

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
            - If your object uses SSE-KMS, don't send encryption request headers for GET requests and HEAD requests, or you???ll get an HTTP 400 BadRequest error.
- Server-Side Encryption with Customer-Provided Keys (SSE-C)
            - you manage the encryption keys and Amazon S3 manages the encryption, as it writes to disks, and decryption, when you access your objects.
            - Amazon S3 does not store the encryption key you provide. 
            - Amazon S3 rejects any requests made over HTTP when using SSE-C. 
            - If you encrypt an object by using server-side encryption with customer-provided encryption keys (SSE-C) when you store the object in Amazon S3, then when you retrieve the metadata from the object, you must use the following headers:

                  x-amz-server-side-encryption-customer-algorithm
                  x-amz-server-side-encryption-customer-key
                  x-amz-server-side-encryption-customer-key-MD5
            
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
AWS KMS supports automatic key rotation only for symmetric encryption KMS keys with key material that AWS KMS creates. Automatic rotation is optional for customer managed KMS keys. AWS KMS always rotates the key material for AWS managed KMS keys every year. Rotation of AWS owned KMS keys varies.
            
Using alias - We can reuse the same code in different AWS Regions. Generate different key in different region. Associate same alias. Application code use alias of a key.
            
Automatic key rotation is not supported on the following types of KMS keys, but you can rotate these KMS keys manually.
  - Asymmetric KMS keys,
  - HMAC KMS keys,
  - KMS keys in custom key stores,
  - KMS keys with imported key material,

Can KMS imported key rotated automatically?What will happen when you import new key material to existing CMK? 
            
You can delete imported key material from a KMS key, immediately rendering the KMS key unusable. Also, when you import key material into a KMS key, you can determine whether the key expires and **set its expiration date.** When the expiration date arrives, AWS KMS deletes the key material. Without key material, the KMS key cannot be used in any cryptographic operation. **To restore the key, you must reimport the same key material into the key. **           
            
When you import key material into a KMS key, the KMS key is permanently associated with that key material. You can reimport the same key material, but you cannot import different key material into that KMS key. Also, you cannot enable automatic key rotation for a KMS key with imported key material. However, you can manually rotate a KMS key with imported key material.
            
When you encrypt data under a KMS key, the ciphertext is permanently associated with the KMS key and its key material. **It cannot be decrypted with any other KMS key, including a different KMS key with the same key material.** This is a security feature of KMS keys.  

Keys generated by AWS KMS do not have an expiration time and cannot be deleted immediately; there is a mandatory 7 to 30 day wait period. All customer managed KMS keys, irrespective of whether the key material was imported, can be manually disabled or scheduled for deletion. In this case the KMS key itself is deleted, not just the underlying key material.

            
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

The KeyId parameter is not required when decrypting with symmetric encryption KMS keys. AWS KMS can get the KMS key that was used to encrypt the data from the metadata in the ciphertext blob. 

#### S3-Server side encryption
- KMS will have master key
- S3 asks KMS to generate data key
- KMS returns data key, encrypted data key
- Master key never leaves KMS
- S3 used data key for AES 256 encryption

Master key is identified by keyId, Key arn, alias, alias arn. **Alias** is used for rotating and it points to current master key. \
Encrypted data key has reference to master key ID( Check if this correct!!) So even in case master key is rotated, it refers to old master key. \
Application code use alias. So no need to update application even if key is rotated.

Each object has different data key.

#### EBS Encryption

Optional feature u can enable when u create a volume. An unencrypted volume cannot be enabled with encryption directly. U need to create snapshot first.U can enable encryption while **copying** snapshot or while **creating new volume from snapshot**. Inorder to change encryption also same. (Note: While taking snapshot u cannot specify encryption. Only after taking snapshot.)

- EBS calls KMS service to get data key.
- The encrypted data key is stored in the volume.
- Use single data key for the volume. Enables high performance.
- When we attach volume to EC2 instance, the identity of user who is attaching volume is used to get data key from KMS.
- The EC2 host keep the data key in memory till host is stopped
- All data moving betwwen ec2 and volume is encrypted
- data at rest is encrypted

Unencrypted Volume --- Snapshot is Unencrypted --- Restored Volume is Unencrypted \
Encrypted Volume by key A---- Snapshot is Encrypted by Key A--- Restored Volume is Encrypted by Key A\

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
 
GenerateDataKeyWithoutPlaintext is identical to the GenerateDataKey operation except that it does not return a plaintext copy of the data key. This operation is useful for systems that need to encrypt data at some point, but not immediately. When you need to encrypt the data, you call the Decrypt operation on the encrypted copy of the key.

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

How EBS encryption works?

EC2 needs the plain text data key to encrypt and decrypt data. 
Encrypted data key is stored in EBS volume.This happens when u create volume. using GenerateDataKeyWithoutPlaintext 
When u attach volume to EC2, Amazon EC2 sends a CreateGrant request to AWS KMS so that it can decrypt the data key.
AWS KMS decrypts the encrypted data key and sends the decrypted data key to Amazon EC2. Using  Decrypt.

In its GenerateDataKeyWithoutPlaintext and Decrypt requests to AWS KMS, Amazon EBS uses an encryption context with a name-value pair that identifies the volume or snapshot in the request. 
```
"encryptionContext": {
  "aws:ebs:id": "vol-0cfb133e847d28be9"
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

You can encrypt communication between your application and the database using SSL/TLS. ???Amazon RDS creates an SSL certificate and installs the certificate on the DB instance when the instance is provisioned???
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

??  The max message size is 4KB for GenerateMAC, VerifyMAC
?? HMAC Keys do not support automatic key rotation or imported key material
?? HMAC keys are currently not supported in CloudHSM (custom key store)

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

- Awazon managed CMK : Key generated by AWS. Stored in AWS account of customer(read-only)

- Customer managed CMK: Key is genereted by customer. Stored in AWS account of customer.

SSE with Customer-Provided Keys (SSE-C): Key is generated by customer and stored by customer. A client has to send the encryption key along with the object to be uploaded in a request. S3 then encrypts the object using the provided key and the object is stored in S3. Note that the encryption key is deleted from the system.When the user wants to download or retrieve the object it has to supply the encryption key in the request. S3 first verifies that it is the correct encryption key, after the successful match it decrypts the object and returns it to the Client.

Note:
AWS KMS is replacing the term customer master key (CMK) with AWS KMS key and KMS key. The concept has not changed. To prevent breaking changes, AWS KMS is keeping some variations of this term.
The KMS keys that you create are customer managed keys. AWS services that use KMS keys to encrypt your service resources often create keys for you. KMS keys that AWS services create in your AWS account are AWS managed keys. KMS keys that AWS services create in a service account are AWS owned keys.
            
Manual rotation is needed for folowing scenarios
            - External Key(Imported keys)
            - CloudHSM Keys
            - KMS key different schedule than supported 1 year
            - Asymettric keys

### Encryption Context
EncryptionContext provides three benefits:

Additional authenticated data (AAD)
Audit trail
Authorization context
           
authenticated encryption with associated data (AEAD): You can think of this as extending the signature over the ciphertext to cover additional data as well. In general, AAD should not contain any secret information, but should be contextual information used to understand the secret information uniquely.
            
 EncryptionContext is KMS???s implementation of AAD.Data that is commonly used for AAD might include header information, unencrypted database fields in the same record, file names, or other metadata. It???s important to remember that EncryptionContext should contain only nonsensitive information because it is stored in plaintext JSON files in AWS CloudTrail and can be seen by anyone with access to the bucket containing the information.

 Problem that EC is solving: Imagine that I have a shared address book that users can use to save and retrieve their physical address. For privacy and security purposes, I will encrypt the addresses before storing them in an Amazon DynamoDB table. The key used for encrypting address is in KMS. Assume each physical addrees is mapped by emailaddress. If user Mallory can modify the DynamoDB table, she can replace Alice???s address with her own. Mallory can do this even without access to the encryption keys by simply swapping the encrypted addresses between the records, which doesn???t require her to encrypt or decrypt anything. 

 We can fix this by including the unencrypted email address associated with the encrypted physical address as EncryptionContext. Now, when the system attempts to decrypt the record that has been tampered with, an InvalidCiphertextException is thrown and the threat is mitigated. This is because the EncryptionContext parameter that was provided at encryption (in this case, Alice???s email address) does not match the EncryptionContext provided at decryption (in this case, Mallory???s email address).

### Key Policy

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
 Allows key administrators to administer the KMS key: Key administrators have permissions to manage the KMS key, but do not have permissions to use the KMS key in cryptographic operations. ( Added throuh Console)
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

Grants are attached to a KMS key, and each grant contains the principal who receives permission to use the KMS key and a list of operations that are allowed.    
            
Grants are commonly used by AWS services that integrate with AWS KMS to encrypt your data at rest. The service creates a grant on behalf of a user in the account, uses its permissions, and retires the grant as soon as its task is complete.   

(Sample scenario to explain use of Grant(Confirm if this correct!!) - A user creating DynamoDB. DynamoDB Create A Grant on behalf of user to generate KMS data key for encryption. So that DynamoD can use it for encrypting and decrypting data. While Creation of DynamoDb, Grant is created. Later Service use the Grant for encryption and decryption. Attach GrantIsForAWSResource so that only DynamoDb can use it)
            
 A key can have only one key policy. Can have multiple Grants.     \
 Grant supports limited operations [CreateGrant, GenerateDataKey, RetireGrant, Encrypt, ReEncryptTo, Decrypt, GenerateDataKeyWithoutPlaintext, DescribeKey, Verify, ReEncryptFrom, Sign]      

 To get a list of grants for a KMS key, use the AWS KMS ListGrants operation.  To find out who or what has access to use the KMS key, look for the "GranteePrincipal" element. 
  
 Grants do not automatically expire. Retire or revoke the grant as soon as the permission is no longer needed. 

Deleting KMS Key
- Once u delete the key, it is irreversible. So better disable the key
- **You can only schedule the deletion of a customer managed key. You cannot delete AWS managed keys or AWS owned keys.**  
- You cannot enable or disable AWS managed keys or AWS owned keys. 
- To determine who uses key - Examining AWS CloudTrail logs to determine actual usage            
- AWS KMS requires you to set a waiting period of 7 ??? 30 days. The default waiting period is 30 days.
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

#### Key policy with ASG
         
Amazon EC2 Auto Scaling uses service-linked roles for the permissions that it requires to call other AWS services on your behalf. A service-linked role is a unique type of IAM role that is linked directly to an AWS service.
            
The predefined permissions also include access to your AWS managed keys. However, they do not include access to your customer managed keys, allowing you to maintain full control over these keys.
            
Your KMS keys must have a key policy that allows Amazon EC2 Auto Scaling to launch instances with Amazon EBS volumes encrypted with a customer managed key.

Assumption(confirm this is correct): When a ASG is configured. It creates a Grant on key for grantee 'service-liked-role'. When instances are created, this Grant allows service-linked-role to encrypt and decrypt the key. ServiceLinkedRole should be allowed for key operations and should be allowed to createAGrant.
            
You must, at minimum, **add two policy statements to your key policy** for it to work with Amazon EC2 Auto Scaling.

1. The first statement **allows the IAM identity specified in the Principal element** to use the customer managed key directly. It includes permissions to perform the AWS KMS Encrypt, Decrypt, ReEncrypt*, GenerateDataKey*, and DescribeKey operations on the key.
2. The second statement **allows the IAM identity specified in the Principal element to use grants** to delegate a subset of its own permissions to AWS services that are integrated with AWS KMS or another principal. This allows them to use the key to create encrypted resources on your behalf.
            
Example 1: Key policy sections that allow access to the customer managed key
```
 {
   "Sid": "Allow service-linked role use of the customer managed key",
   "Effect": "Allow",
   "Principal": {
       "AWS": [
           "arn:aws:iam::123456789012:role/aws-service-role/autoscaling.amazonaws.com/AWSServiceRoleForAutoScaling"
       ]
   },
   "Action": [
       "kms:Encrypt",
       "kms:Decrypt",
       "kms:ReEncrypt*",
       "kms:GenerateDataKey*",
       "kms:DescribeKey"
   ],
   "Resource": "*"
}
            
{
   "Sid": "Allow attachment of persistent resources",
   "Effect": "Allow",
   "Principal": {
       "AWS": [
           "arn:aws:iam::123456789012:role/aws-service-role/autoscaling.amazonaws.com/AWSServiceRoleForAutoScaling"
       ]
   },
   "Action": [
       "kms:CreateGrant"
   ],
   "Resource": "*",
   "Condition": {
       "Bool": {
           "kms:GrantIsForAWSResource": true
       }
    }
}            
```    
Example 2: Key policy sections that allow cross-account access to the customer managed key        
            
Step1: key policy should allow IAM of another account role and give Grant permission(same as above, but Principal will be account root user.)
            
Step2: Then, from the account that you want to create the Auto Scaling group in, create a grant that delegates the relevant permissions to the appropriate service-linked role. The Grantee Principal element of the grant is the ARN of the appropriate service-linked role of asg in second account. The key-id is the ARN of the key in source account.
            
For this command to succeed, the user making the request must have permissions for the CreateGrant action.  
```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "AllowCreationOfGrantForTheKMSKeyinExternalAccount444455556666",
      "Effect": "Allow",
      "Action": "kms:CreateGrant",
      "Resource": "arn:aws:kms:us-west-2:444455556666:key/1a2b3c4d-5e6f-1a2b-3c4d-5e6f1a2b3c4d"
    }
  ]
}
```         
            
https://aws.amazon.com/blogs/security/managing-permissions-with-grants-in-aws-key-management-service/   

### Key Policy Cross account

User assumes role with permission to use a KMS key in a different AWS account

Key in Account2
            
- The key policy for the KMS key in account 2 allows account 2 to use IAM policies to control access to the KMS key.
- The key policy for the KMS key in account 2 allows account 1 to use the KMS key in cryptographic operations. However, account 1 must use IAM policies to give its principals access to the KMS key.
- An IAM policy in account 1 allows the Engineering role to use the KMS key in account 2 for cryptographic operations.
- Bob, a user in account 1, has permission to assume the Engineering role.
- Bob can trust this KMS key, because even though it is not in his account, an IAM policy in his account gives him explicit permission to use this KMS key. 
            
Key Policy in Account 2 should allow accoun2 and account1(cross account)
```
            {
    "Id": "key-policy-acct-2",
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "Permission to use IAM policies",
            "Effect": "Allow",
            "Principal": {
                "AWS": "arn:aws:iam::444455556666:root"
            },
            "Action": "kms:*",
            "Resource": "*"
        },
        {
            "Sid": "Allow account 1 to use this KMS key",
            "Effect": "Allow",
            "Principal": {
                "AWS": "arn:aws:iam::111122223333:root"
            },
            "Action": [
                "kms:Encrypt",
                "kms:Decrypt",
                "kms:ReEncryptFrom",
                "kms:ReEncryptTo",
                "kms:GenerateDataKey",
                "kms:GenerateDataKeyWithoutPlaintext",
                "kms:DescribeKey"
            ],
            "Resource": "*"
        }
    ]
}
```
IAM Policy in Account 1 which allows to perform cryptographic operation on key in account2
```
 {
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "kms:Encrypt",
                "kms:Decrypt",
                "kms:ReEncryptFrom",
                "kms:ReEncryptTo",
                "kms:GenerateDataKey",
                "kms:GenerateDataKeyWithoutPlaintext",
                "kms:DescribeKey"
            ],
            "Resource": [
                "arn:aws:kms:us-west-2:444455556666:key/1234abcd-12ab-34cd-56ef-1234567890ab"
            ]
        }
    ]
}
```     

Do not set the Principal to an asterisk (*) in any key policy statement that allows permissions unless you use conditions to limit the key policy. An asterisk gives every identity in every AWS account permission to use the KMS key, unless another policy statement explicitly denies it. 
            
KMS Encrypt max size is 4kb.           
            
            
Each time you import key material to a CMK, you need to download and use a new wrapping key and import token for the CMK. The wrapping procedure does not affect the content of the key material, so you can use different wrapping keys (and different import tokens) to import the same key material.

Cross Account KMS APi calls are recorded in the CloudTrail logs of both accounts.

To create an alarm that notifies you when someone in your account tries to use a KMS key that is pending deletion

CloudTrail -> cloudWatch logs -> metric -> Alarm

{ $.eventSource = kms* && $.errorMessage = "* is pending deletion."}

Determining past usage of KMS key

All AWS KMS API activity is logged by CloudTrail. By evaluating these log entries, you might be able to determine the past usage of a particular KMS key, and this might help you determine whether or not you want to delete it.

kms:Decrypt : key Id not required for symmetric keys.

The KeyId parameter is required when calling "Decrypt" or "Verify" with an asymmetric KMS key, or calling "VerifyMac" with an HMAC KMS key. 
            
USe wildCard "Resource": "*" element only needed. It is needed for CreateKey, ListAliases, ListKeys.       



Key Policy Evaluation

In same account
              : Either Key policy allows or Grant allows 
              : Or Key policy allows IAM and IAM policy allows

In Cross account :
                Above +
                callers account should give a grant  or Iam policy that permits to use this key

https://docs.aws.amazon.com/images/kms/latest/developerguide/images/kms-auth-flow-2020.png

           
