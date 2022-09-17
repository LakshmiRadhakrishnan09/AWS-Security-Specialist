

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

Each object has different data key.

#### EBS Encryption

Optional feature u can enable when u create a volume

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

