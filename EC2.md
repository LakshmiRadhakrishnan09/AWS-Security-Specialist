
How AMI are created and distributed across accounts in an enterprise? 

Instance metadata : http://169.254.169.254/latest/meta-data/

How private keys are managed when we use Bastion host to connect to a private instance? \
Answer: Using credential forwarding mechanisim in putty

## EC2 Storage ##

* Block Storage : Just like harddrives in ur computer. U can create a file system. Used for databases, OS.
    * Instance Store
    * Elastic Block Store(EBS)
* File Share: Network file share. Centalized repository. Accessible from anywhere
    * EFS
    * FSx
* Object Store: Accessible from anywhere
    * S3

Instance Store
* Storage of host computer on which EC2 VM is running is assigned
* Highest performnace
* Temporary storage. If instance stop or terminate data is lost. If reboot , data is not lost.
* Price is included by Instance price
* Hadoop File System. Mogo db uses Instance Store. For availability, data is replicated between EC2 clusters.
* U cannot attach at Instance Volume after instance is created
* U can attach multiple volumes to one intsance
* Two types
   * SSD
        * Random I/O
        * Peformance is measured in how many read/writes(IOs) per second. IOPS
        * Ideal for databases( Relational and NoSql), transactional online data, caching
        * Is costly
        * Can support millions of IOPS
   * HDD
        * Sequential I/O
        * Performance is measured on how many bytes are transferred(throughput). MiB/s
        * Ideal for log processing, mapreduce






