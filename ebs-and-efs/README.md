# EBS & EFS

## EBS Volume

When you terminate a EC2 instance, the root volume looses all of its data. EBS \(Elastic block storage\) is a network drive that you can use to store data and connect it to different instances. You can use is for database storage.

The network connectivity to access the EBS drive means there will be a little latency. But a benefit is that it can be detached from one instance and can be attached to other instance.

**EBS are locked to a AZ.**  To move EBS across volumes, you need to snapshot it.  

You have to give a provisional capacity \(how much you will use\) while setting up EBS. **You get billed for provisional not how much used data.**

**Root EBS volumes of the instances get terminated by deafault if the EC2 instance gets terminated.** 

### Types of EBS Volumes:

* 4 types of EBS volumes:
  * **GP2 \(SSD\):** General purpose SSD. balances price and performance.
    * System boot volumes
    * Virtual desktops
    * Low latency interactive apps
    * 1 Gib -16 T-b
    * **Max IOPS is 16000,  MIN 100 IOPS**
    * **3 IOPS per GB**
    * 
  * **IOI \(SSD\):** Highest performance SSD volume for mission critical low latency or high throughput workloads.
    * If you need more that 16000 IOPS then use this
    * Use this for critical database stuff
    * Provisined IOPS **MIN 100 MAX 64000**
    * **4 Gib - 16 TiB Size** and IOPS are independent.
  * **STI \(HDD\):** Low cost HDD volume designed for frequently accessed throughput intensive workload.
    * Streaming workload
    * 500 Gib - 16 TiB throughput
  * **SCI \(HDD\):** Lowest cost HDD volume designed for less frequently accessed workload. 
    * Throughput oriented storage for large volumes of data that is infrequently used.
    * **250 Gib - 16 Tib**
* EBS volumes are characterized in Size \| Throughput \| IOPS \(Input/Output operations per second\)
* **Only GP2 and IOI can be used as boot volumes.**

\*\*\*\*

\*\*\*\*

### **Mounting EBS on Linux**

\*\*\*\*[**https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ebs-using-volumes.html**](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ebs-using-volumes.html)\*\*\*\*

```text
lsblk - shows all attached drives

```





### EBS Snapshot   :

* Snapshots are incremental i.e only changed blocks of memory are saved.
* The Backup requires IO power so don't use when you in need of IO.
* Snapshots are stored internally in S3.
* **Not required to detach** the volume but **recommended.**
* We can copy snapshots from AZ or Region.
* We can make AMIs from Snapshots.

We can have automated backup solutions using AWS Backups.



### EBS Migration:

* Snapshot the volume.
* Copy the volume to a different region.
* Create a volume from the snapshot in the AZ of your choice.

### EBS Encryption:

* When we create an encrypted EBS, we get
  * Data at rest is encrypted inside the volume.
  * All the data in flight moving between the instance and the volume is encrypted.
  * All snapshots are encrypted.
  * All volumes created from the snapshot are encrypted.
* Enc and Dec are handled transparently.
* Encryption has minimal impact on latency.
* EBS encryption leverages keys from KMS \(AES - 256\).
* Copying an encrypted snapshot allows encryption.
* Snapshots of encrypted volumes are encrypted.

* To encrypt an unencrypted EBS volume:
  * Create a snapshot of the EBS Volume
  * Encrypt the EBS snapshot using Copy snapshot
  * Create a new EBS volume from the snapshot \(the volume will also be encrypted\)
  * Now you can attach the encrypted volume to the original instance.

### EBS vs Instance Store

* Some instance comes with "Instance store"
* Instance store is something that is physically attached to your hardware. i.e physically attached disk
* Pros:
  * Better IO performance
  * Good buffer/cache/ scracth data/ temporary content
* Cons:
  * **Cant resize**
  * on termination, the instance is lost
  * Backups must be operated by the user.
* Is my data ephemeral/ Am i okay with losing my data?
  * if not the use EBS

### EBS RAID configurations:

* EBS is already redundant storage \(replicated within an AZ\)
* **RAID 0**
  * **Means performance**
  * Combine two or more EBS volumes into one logical space and getting total disk space and IO.
  * One disk fails, all the data is failed.
  * Use case:
    * Applications that need lots of IOPS and doesnt need fault tolerance
    * A database that has replications already built in.
  * Using this, we can have a very big disk with a lot of IOPS
  * For example: two 500 Gib Amazon EC2 IO 1 volumes with 4000 provisioned IOPS each will create a 1000 GiB RAID 0 array with an available bandwidth of 8000 IOPS and 1000 MB/s of throughput.
* **RAID 1:**
  * **Means faul tolerance**
  * Writing data on both volumes at same time
  * Mirror a volume to another
  * If one volume fails then our logical volume is still working.
  * 2x network throughput since data is going to 2 EBS.
  * Application that needs volume fault tolerance
  * Application where you need to service disks
  * for example:
    * two 500 GiB Amazon EBS IO 1 volumes with 4000 provisioned IOPS each will create a 
    * 500 GiB RAID 1 with exact same IOPS.

## EFS - Elastic File System

* Managed Network File System that can be mounted on many EC2
* **EFS works with EC2 instances in multi AZ**
* Highly available, salable, expensive \(3x gp2\), pay per use.
* * EC2 instances                EC2 instances                    EC2 instances
  *  **---------------------- Security group------------------------**
  * **===================EFS======================**
* \*\*\*\*
* **Uses NFSv4.1 protocol.**
* Use security group to control the EFS
* Compatible with only Linux not Windows AMI
* Encryption at rest using KMS.
* File system scales automatically, pay per use, no capacity planning.
* * EFS Scale:
  * 1000s of concurrent NFS clients, 10 GB+ /s throughputs
  * GRow to petabye
* Performance mode \(set at EFS Creating time\)
  * General purpose: latency-sensitive use cases \(web server,CMS\)
  * Max I/O - higher latency throughput, highly parallel \(big data, media processing\)
* Storage Tiers lifecycle management failure - move file after N days\)
  * Standard: for frequently accessed files
  * Infrequent access \(EFS - IA\): cost to retrieve files, lower price to store



### 



\*\*\*\*

