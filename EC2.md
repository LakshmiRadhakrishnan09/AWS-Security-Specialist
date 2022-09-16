
How AMI are created and distributed across accounts in an enterprise? 

Instance metadata : http://169.254.169.254/latest/meta-data/

How private keys are managed when we use Bastion host to connect to a private instance? \
Answer: Using credential forwarding mechanisim in putty

## EC2 Storage ##

* Block Storage : Just like harddrives in ur computer. U can create a file system. Used for databases, OS.Fast access
    * Instance Store
    * Elastic Block Store(EBS)
* File Share: Network file share. Centalized repository. Accessible from anywhere. Grow and Shrink automatically.
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

Elastic Block Store
* Sits outside of host machine
* Persist storage
* Can detach and attach to another EC2 in same AZ.
* Snapshots stored in S3. Can u used to replicate in another AZ.( Disaster Recovery)
    * Incremental Snapshot
    * Point in time snapshot
    * In case of multiple EBS volumes, snap shot need to be consistent
          * Stop and then take snapshot
          * Use DB backup tool for DB products
          * Multi volume snapshot
* Relational Database, NoSql applications.
* 4 types of EBS volumes
   * SSD
      * General Purpose
         * balance price-performance
         * For every GiB, 3 IO per second
         * If u dont use these IOs, it will get aggregated and allows to use upto 3000 IOPS(Burst). Helps to handle spikes. But if credit runs out, it will throttle to baseline.
         * Max IOPS, 16000 ( 5.3 TB or more). If requirement is more than this, then GP is not suitable.
         * One way to increase IOPS is by RAID 0(doubles throughput, IOPS, Volume). Use **two** EBS volume. U need to manage additional software
         * RAID 1: Mirrors data. Since EBS volumes are already replicated, RAID 1 is not useful for EBS.
         * Max throughput: 250 MB/s
         * Ideal for development and test workloads
      * Provisioned IO
         * Latency Sensitive transactional workloads.
         * You need to specify required IOPS. EBS guarentee this IOPS.
         * Max 64000 IO per second. Constraint: Max IO cannot be more than 50 times the storage size in GiB.So if u allocate a 100 GB volume, max IO is 5000.
         * Max throughput: 1000 MB/s
   * HDD
      * Through put optimized- st1
         * Low cost
         * Max IOPS per GB is 500
         * Max throughput is 500
         * Hadoop, Data ware house, log processing
      * Cold - sc1
         * Max IOPS per GB is 250
         * Max throughput is 250
         * In frequently accessed sequential workloads
* Summary
    * st1 ans sc1 are cheap. Suitable for data ware house, analytical or log processing use cases
    * General purpose is cheap. But IOPS is limited. For 500 GB, Base line (500 * 3 = 1500 IOPS). Can burst upto 3000.
    * Provisioned. Price vary by IOPS provisioned. Costly compared to GP.


AWS File share Service
* Eg. private space for each employee in ur organization( documents and data)
* While lift and shift from on-premise to cloud
* Why not s3? S3 needs API. File share using file system commands. Easy for existing ( non cloud) solutions.
* EFS on linux
* FSx on Windows
* Fsx for Lustre

EFS
* NFS File System

Fsx
* NTFS on SMB protocol

FSX for lustre
* Standalone
* Link to S3
* High performance
