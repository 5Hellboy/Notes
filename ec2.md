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
* **Most** cost efficient in AWS.
* Use for workloads that are resilient to failure
  * Batch jobs
  * Data analysis
  * Image processing
  * **stuff that we can retry**
* **Not great for critical jobs or databases \(because we can lose them\)**
* **Great combo: Reserved instances for baseline + On-demand & Spot for peaks.** Reserved can be used for web apps. Unpredictable stuff can get on-demand or spot istances.

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

