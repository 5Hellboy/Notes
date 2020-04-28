# EC2

## EC2 aka Elastic Compute Cloud  <a id="firstHeading"></a>

EC2 is a service that allows the user to manage virtual machines on the server. It offers different abilities like: 

* renting virtual machines \(EC2\)
* storing data on virtual machines \(EBS\)
* load balancing across virtual machines\(ELB\)
* scaling the services \(ASG\) 



### Security Groups

Security groups are like a **firewall** that control the inbound and the outbound traffics of the AWS. It is very similar to iptables imo. They **control access** to the ports.

* inbound - allows traffic into the EC2. \(should be controlled properly\)
* outbound - traffic that goes out of the EC2 machine.



### Note

* Security groups are firewalls **outside** the EC2 instance. i.e they live outside the EC2.
* Can be attached to multiple instances.
* Maintain a **separate security group for SSH access.**
* Security groups are locked down to a region/VPC combination.
* If you application **times out** then its a **security group issue** as the traffic has not even reached the EC2 instance.
* If you get a **connection refused** error then its an **application issue** as the traffic has reached the instance but refused by the application.
* By default,
  * All **inbound** traffic is **blocked** 
  * All **outbound** traffic is **authorized**.
* Instances with same security groups for inbound connections can communicate with each other.





```text
To connect to EC2 using SSH, make sure you set the permissions of the SSH key for user ec2-user to 0400.
```







### EC2 Instance types

* **On demand instances:** short workload, predictable pricing
* **Reserved**: \(Minimum 1 year i.e known amount of time\)  - when you have to run databases,etc
  * Reserved instances: long workloads
  * Convertible Reserved Instances: long workloads with flexible instances. You can convert the instances as per requirements.
  * Scheduled Reserved instances: example - every thursday between 3 and 6 pm. When you have to run some task for specific time but you need it for **at least a year.**
* **Spot Instances:** Short workloads, very cheap, but risk is you can lose instances \(less reliable\)
* **Dedicated instances:** no other customers will share the hardware on AWS. 
* **Dedicated Hosts:** book an entire physical servers, control instance placement.

#### EC2 On demand instances:

* Pay exactly for what we use \(billing is per seconds after the first minute\) when we start and stop instances, we just pay for the amount of time we used it
* Has the highest cost but no upfront payment.
* Has no long term commitments.
* Recommended for short term needs and un-interrupted workloads, where you can't predict how the application will behave.
* **Great for elastic workloads.**

#### Reserved Instances:

* We know that we can use the instances for long period of time so good for IT.
* Up to 75% discount compared to On-demand.
* Pay upfront for what you use with long term commitments.
* Reservation period can be 1 or 3 year.
* Reserve a specific instance type. 
* Recommended for steady state usage application \(like databases\)

**Convertible Reserved Instances**

* can change the EC2 instance type.
* Up to 54% discount. 

**Scheduled Reserved Instances**

* launch within a time window when we reserve.
* When we require a fraction of day/ week/ month.

#### EC2 Spot instances

* Can get upto 90% discount compared to on-demand.
* We can lose the instance at any point of time based on the price we pay. **If the max price &lt; current spot price: we lose instance.**
  * Hourly price varies based on offer and capacity.
    * if \(current price &gt; max price\) : 2 minutes grace period to {

      Stop or termiate

      } 
* **Most** cost efficient in AWS.
* **Spot block:** 
  * **"block"** spot instance during a specified time frame \(1 to 6 hours without interruptions\) 
  * In rare situations, the instance may be reclaimed.
  * You can cancel spot requests that are **open,active or disabled.**
  * **Cancelling a spot request does not terminate instances.**
  * **Cancel the spot request and then terminate the associated spot instances.**
* Use for workloads that are resilient to failure
  * Batch jobs
  * Data analysis
  * Image processing
  * **stuff that we can retry**
* **Not great for critical jobs or databases \(because we can lose them\)**
* **Great combo: Reserved instances for baseline + On-demand & Spot for peaks.** Reserved can be used for web apps. Unpredictable stuff can get on-demand or spot istances
* * **Spot Fleets \(SAA CO2\)**[Introduction](https://app.gitbook.com/@shivaji/s/awsaa/~/drafts/-M5Yd8A1PJA1Hg8sfUF6/)

  * **set of spot instances + \(optional\) On-Demand Instances**
  * Will try its best to meet the target capacity with price constraints. 
    * Define possible launch pools: instance type, OS, Availability Zones.
    * Can have multiple launch pools, so that the fleet can choose the best and most appropriate launch pools.
    * It stops when reaching capacity or max cost. 
  * Strategies to allocate spot instances:
    * **lowestPrice**: _launch instances from the pool with the lowest price_ \(cost optimization, short workloads\) \*\*
    * **diversified**: distributed across all pools \(great for availability, long workloads\)
    * **capacityOptimized:** pool with the optimal capacity for number of instances. 
  * **Spot Fleets allow us to automatically request Spot Instances with the lowest price.**
  * **Can be reclaimed by AWS.**

#### EC2 Dedicated Hosts 

* Physical dedicated EC2 server for you use.
* Full control of EC2 Instance placement.
* Visibility into the underlying sockets/physical cores of hardware which is great for licencing stuff.
* Allocated for your account for a 3 year period reservations.
* More expensive
* Useful for software that have complicated licensing model like some licenses that bill you based on number of core/sockets.
* Strong regulatory or compliance needs then you have to make sure you have your own hardware.

#### EC2 Dedicated Instances.

* Instances running on hardware that's dedicated to you. 
* May share hardware with other instances in same account.
* No control over instance placement \(can move hardware after stop/start\)

### **AWS Instance types** 

* **R**: applications that need a lot of **RAM**  in memory caches/memory databases
* **C**: applications that needs good **CPU** - computations / databases
* **M**: applications that are balanced \(think "**medium**"\)  - general / web application 
* **I**: applications that need good **I/O** \(instance storage\) - databases
* **G**: applications that need a **GPU** - video rendering / machine learning.
* * **T2/T3**: **burstable** instances\( up to a capacity\) - over use a burst then you lose the burst. good performance for a short while.
  * Burst means that overall, the instance has OK CPU performance.
  * during burst, then CPU can be very good.
  * If **machine bursts, it utilizes "burst credits"**
  * if **all credits are gone, the CPU becomes bad**
  * If the **machine stops bursting, credits are accumulated over time.**
  *  ****
  * **Amazing to handle unexpected traffic and getting insurance that it will handled correctly.**
  * if you instance runs low on credit, you need to move to a different kind of non-burstable instance.
* **T2/T3: unlimited**: unlimited burst.
  * unlimited burst credit balance. 
  * you pay extra money if you go over your credit balance, but you don't lose in performance.
  * costs could go high if you're not monitoring the health of your instances.
* www.ec2instances.info



### AMI

* images can be customized using runtime using EC2 user data.
* AMI - an image to use to create our instances. 
* AMI's can be built for Linux or Window machines.

Why custom AMI?

* Pre installed packages.
* Faster boot time \(no need for ec2 user data at boot time\)
* Machine comes configured with monitoring/enterprise software
* Security concerns - control over the machines in the network
* Active Directory Integration out of the box
* Installing app ahead of time \(faster deploys when auto scaling\)
* Using some one else's AMI that is optimized for running and app,DB,etc
* * **AMI are built\(locked\) for a specific AWS region.**

 ****

* Public AMI;s are available that you can use but only use those AMI that you trust. Public AMI's are available on amazon marketplace.
* AMI are stored on Amazon S3
* By default AMI are locked but we can make them public
* Charged for the actual space in amazon S3

\*\*\*\*

**Exam Tip**

* You can share an AMI with another AWS account.
* Sharing an AMI does not affect the ownership of the AMI.
* If you copy an AMI that has been shared with your account, you are the owner of the target AMI in your account.
* To copy an AMI that was shared with uou from another account, the owner of the source AMI must grant you read permission for the storage that backs the AMI, either the associated EBS snapshot \(for an Amaazon EBS-backed AMI\)or an associated S3 bucked \(for instance store-backed AMI\).
* Limits:
* You can't copy an encrypted AMI that was shared with you from another account. Instead, if the underlying snapshot and encryption key were shared with you, you can copy the snapshot while re-encrypting it with a key of your own. You own the copied snapshot, can register it as new AMI.
* You can't copy an AMI with an associated **billingProduct** code that was shared with you from another account. This includes Windows AMIs and AMIs from AWS Marketplace. TO copy a shared AMI with **billingProduct** code, launch an EC2 instance in your account using the shared AMI and then create an AMI from the instance. 
* Add 'create volume' doesnt prevent users to fully copy it. they can still create EC2 instances and make the AMI.





### EC2 Placement Groups

Placement groups allows us to be able to control the placement of EC2 instances. Basically it allows us to place the EC2 instances in specific hardware in a specific way. There are strategies defined to control the EC2 instances. These strategies are as follows:

* **Cluster:** clusters instances into a low-latency group in a single Availability Zone. 
  * All EC2 are in same rack, in same AZ. 
  * Great network \(10 Gbps banwidth\)
  * Single point of failure. if rack fails, all fails.
  * Uses:

    * Big data job that needs to complete fast
    * Application that needs extremely low latency and high network throughput.
* **Spread:** spreads instances across the underlying hardware \(max 7 instances per group per AZ\) - used for critical applications.
  * EC2 instances are spread on different hardware which is a good thing. 
  * Reduced risk is simultaneous failure. 
  * The instances are on different hardware so no single point of failure.
  * **But, we only get 7 instances per AZ.**
  * Uses:
    * Applications that need to maximize high availability.
    * Critical applications where each instance is isolated from failure from each other.



* **Partition \(set of racks\):** spread across many different partitions \(which rely on different set of racks\) within an AZ. Scales to 100s of EC2 instances per group.
  * Each partition has different EC2 instances. no shared failure accross partitions. A partition failure can affect many EC2 but not in other partitions. 
  * **7 partitions per AZ.**
  * Up to 100s of EC2 instances.
  * The instances in the partition **do not** share racks with the instances in the other instances.
  * EC2 instances get access to the partition information as metadata.
  * Use cases: HDFS, HBase, Cassandra, Kafka, distributed stuff.

### Elastic network interfaces \(ENI\)

ENI are a logical component in a VPC that represents a virtual network card. It is **bound to a specific AZ.** 

The ENI can have following attributes:

* Primary private IPv4, one or more secondary IPv4
* One Elastic IP \(IPv4\) per private IPv4
* One              Public IPv4
* One or more security groups
* A MAC address

You can create ENI independently and attach them on the fly\(move them\) on EC2 instances for failover. 

eth1 can be detached and reattached but eth0 can't

**Different IPs are due to different EC2 instance attachments.**





