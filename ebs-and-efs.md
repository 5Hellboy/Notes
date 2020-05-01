# EBS & EFS

## EBS Volume

When you terminate a EC2 instance, the root volume looses all of its data. EBS \(Elastic block storage\) is a network drive that you can use to store data and connect it to different instances. You can use is for database storage.

The network connectivity to access the EBS drive means there will be a little latency. But a benefit is that it can be detached from one instance and can be attached to other instance.

**EBS are locked to a AZ.**  To move EBS across volumes, you need to snapshot it.  

You have to give a provisional capacity \(how much you will use\) while setting up EBS. **You get billed for provisional not how much used data.**

### Types of EBS Volumes:

* 4 types of EBS volumes:
  * **GP2 \(SSD\):** General purpose SSD. balances price and performance.
    * System boot volumes
    * Virtual desktops
    * Low latency interactive apps
    * 1 Gib -16 T-b
    * Max IOPS is 16000
    * 3 IOPS per GB
    * 
  * **IOI \(SSD\):** Highest performance SSD volume for mission critical low latency or high throughput workloads.
  * **STI \(HDD\):** Low cost HDD volume designed for frequently accessed throughput intensive workload.
  * **SCI \(HDD\):** Lowest cost HDD volume designed for less frequently accessed workload. 
* EBS volumes are characterized in Size \| Throughput \| IOPS \(Input/Output operations per second\)
* Only GP2 and IOI can be used as boot volumes.

