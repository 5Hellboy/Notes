# High Availability and Scalability: ELB & ASG

## High Availability 

* Goes hand in hand with horizontal scaling.
* Running your application/system in atleast 2 data center \(AZ\)
* Goal is to survive data loss.
* can be passive. \(for RDS multi AZ\)
* can be active \(for horizontal scaling\)

## Vertical Scalability 

* **Increase size of the instance**. i.e scale up the size/memory/power of the CPU
* Up-scaling an application
* non distributed systems such as database

## Horizontal Scalability

* Increasing the number of instances/ systems for your applications
* Implies distributed systems.
* Common for web applications.

## Load Balancing

A load balancer is a middle man between traffic and EC2 instances. It balances the incoming traffic between multiple instances.

* It spreads load across multiple downstream instances.
* Expose a single point of access \(DNS\) to your application.
* Handle failures of downstream.
* Does a regular health check on instances.
* Provides SSL termination \(HTTPS\) for your cookies.
* Enforce stickiness with cookies.
* High availability across zones.
* Separate public traffic from private traffic.

**Health check:** checking if the port returns 200 OK response. timing of health checks can be configured. 

3 types of load balancers:

* **Classic Load Balancer** \(V1 - old\)  2009
  * HTTP, HTTPS, TCP
* **Application Load Balancer** \(v2 - newer generation\) - 2017
  * HTTP, HTTPS, WebSocket
  * Layer 7 \(HTTP\)
    * Load balancing across multiple HTTP applications across machines \(target groups\)
    * Load balancing to multiple applications on the same machine \(ex. container\)
    * Support for HTTP/2 and WebSocket
    * Support redirects \(from HTTP to HTTPS for example\)
    * **Routing tables enabled for different Load Balancers**
* **Network Load Balancer** \(v2 - new generation\) - 2017
  * TCP, TLS & UDP
    * Forward TCP and UDP traffic to your instances
    * Handle millions of requests per seconds
    * Less latency
    * * NLB has one **static IP per AZ** and supports assigning Elastic IP

We can set up internal and external load balancers.

**Troubleshooting:**

* 4xx errors are client induced errors.
* 5xx errors are application induced errors.
* Load Balancer Errors 503 means at capacity or no registered target
* If the LB cant connect to your application, check your security group

**Monitoring:**

* ELB access logs will log all access requests \(so you can debug per request\)
* CloudWatch Metrics will give you aggregate statistics \(ex: connection counts\)

**LB have their own security groups through which the users can interact. The LB interacts with EC2 through an Application security Group.** 

\*\*\*\*

**Security is much better when the only entry point for EC2 instances are the load balancers and not the direct connection to EC2.**

### **Load Balancing Stickiness**

Stickiness is when you want the same instance to handle the request of the same client. This is done by a **cookie.** Enabling the stickiness can bring load imbalance between the instances.

### Cross-Zone Load Balancing

Load balancers would distribute the traffic between instances that that are in registered in different AZs. This evenly spreads out traffic. Compared to non-cross zone load balancing where the traffic is only in those specific AZs. **Disabled by default. No charge.**

**Enabled by default on Application Load Balancer. \(Cannot be disable\)**

**Disabled by default on Network Load Balancer. \(requires $\)**

\*\*\*\*

### **Elastic Load Balancers - SSL Certificates**

* Load balancer uses X.509 certificate \(SSL/TLS server certificate\)
* You can manage certificates using ACM \(AWS Certificate Manager\)
* You can create upload your own certificates alternatively.
* * HTTPS listener - you can specify certs or list your own 
* **SNI :** 
  * **Multiple sites can be served using multiple certs.** 
  * The load balancer knows what cert belongs to what instances based on SNI.
  * Works on ALB, NLB not CLB

**Connection Draining:** 

* in CLB its called connection draining
* Target Group: Deregistration Delay \(in ALB,NLB\)
* When an EC2 instance is being de-registered or deemed unhealthy, then the load balancer decides to continue to the existing connections with that specific EC2 instances for a given amount of time. After that time, the load balancer will connect to other EC2 instances. 
* No new connections are made with the deregistiring EC2 instance and the load balancer waits for the existing connections to complete.

### Auto Scaling Groups

* ASG is a feature in AWS which allows us to add EC2 instances or decrease the EC2 instances on the fly or as per load. We can also set the limit of maximum and minimum EC2 instances that we have to set. This feature automatically registers new features to the load balancer. 
* Sizes:
  * **Minimum size**: The number of EC2 instances that we know for sure are running.
  * **Actual Size/ Desired Capacity**: The number of EC2 that are running at the moment.
  * **Maximum size**: The total number of EC2 added when scaled to handle load.b
* **Auto scale alarms:**
  * These are the conditions that trigger the scale in or scale out features. 
  * The alarms are set on Cloudwatch metrics. The metrics can be anything we want like traffic, number of users,etc.
  * Use launch templates/configs 
  * **IAM roles that are attached to the ASG will get assigned to EC2 instances.** 
  * ASG is free but the resources that it loads are not like EC2,etc.
  * If unhealthy instances are terminated then ASG will replace it with new one.

#### Auto Scaling Groups - Scaling policies

* **Target Tracking Scaling:** 
  * In this you set up like an average  CPU usage of the instances and then make decisions.
* **Simple / Step Scaling:**
  * We can scale up \(add instance\) or scale down \(remove instance\) based on the cloudwatch logs.
* **Scheduled Actions:**
  * Anticipate scaling based on some patterns like timely pattern of load,etc.

#### Scaling Cool down:

* The cooldown period ensures that the auto scaling doesnt launch or terminate instances before the previous scaling activities have taken affect. 
* We can create cooldowns to apply to our specific scaling policies. Custom scaling policy period overrides default policy period.
* for scaling down policies, amazon ec2 auto scaling needs less time to determine whether to terminate additional instances.
* You can override the scaling policies to terminate instances faster \(default 300 seconds\) to save cost.

### ASG for solutions architect

* How are the instances terminated? Say in 2 different AZ's 
  * First one to be zapped is the AZ with most instances
  * If there are multiple instances in AZ to choose from then choose the one with oldest launch configuration. 
* * Life cycle hooks
  * These are like hooks that helps us configure/interact with the instance when they are about to be in service or when they are about to be terminated. you can pause their service and termination and install software or extract information.
* * Launch template and configurations?
  * Configs are legacy
  * templates are newer ones
  * templates have parameters subsets - can make reusable configs.
  * templates can allow spot or on demand.
  * templates can use t2 unlimited burst feature.
  * both id of the AMI, instance type, key pair, security groups and other parameters.



\*\*\*\*



## Questions & Answers

* **What is High Availability?**
  * High Availability means having your data/application highly available and in use. Best way to achieve that is to have the application/instances running in atleast two different AZs.
* **What is Scalability?**
  * Scalability is the idea of scaling the infrastructure based on the current capacity of traffic. You can scale-in i.e reduce the instances or you can scale-out i.e increase the instances.
* **What is Vertical and Horizontal Scaling?**
  * **Vertical:**
    * Scaling the power of the current instance. I.e going up or down in instance capacity of handling load. Short term solution.
  * **Horizontal:**
    * Scaling the systems i.e increasing or decreasing the number of systems.  Long term fix. Common for distributed applications.
* **What is Auto Scaling?**
  * Auto scaling is a feature that allows the users to automatically increase or decrease i.e set up correct number of instances to manage the traffic. 
* **What are Auto Scaling groups?**
  * Auto Scaling groups is feature that allows us to create a collection of EC2 instances and set scaling policies on them. i.e set maximum/minimum/ desired number of instances.
* **What is maximum/minimum/desired number of instances in Auto Scaling?**
  * Minimum : Minimum number of EC2 instances that you want to handle the traffic. Auto scaling group ensures that the instance number does not go below this size.
  * Maximum: Maximum number of EC2 instances that you want to handle the traffic. When traffic increases, the number of instances increases to manage the traffic. This number does not go above the set maximum number.
  * Desired: The desired number of EC2 instances that you want to handle the traffic at any given point. 
* **What is Auto Scaling alarms?**
  * Auto scale alarms are raised when a specific criteria for instance load is met. For example, CPU usage, users logged in, network traffic,etc. When the criteria is met, the auto scale alarm is set the the scaling policy decides whether to scale in or scale out. 
* **What are the Scaling policies?**
  * Scaling policies define how to scale the capacity of the instances in response to the changing demand.
* **What are different types of Scaling policies?**
  * Simple scaling policy:
    * Scaling policy that is set upon a single scaling adjustment. i.e Increase or Decrease the capacity based on a single policy.
  * Step scaling policy:
    * Scaling policies that are based on a set of scaling adjustments. 
  * Target scaling policy:
    * Scaling policy based on a target metric.
  * Scheduled Actions:
    * Scaling policies based on an anticipated even.
* **What is Elastic Load Balancer \(ELB\)? What does it do?**
  * Elastic load balancer balances the incoming traffic between EC2 instances. 
  * It allows stickiness policy using cookies i.e a user is sticked to a specific instance to maintain sessions,etc.
  * Separates out the internal and external networks. 
  * Provides a DNS address instead of EC2 IP addresses.
  * Conduct health checks by sending requests to instances and expecting a 200 OK response.
  * Handles downstream failure by terminating instances using Application Security Groups. 
* **What is Load Balancing Stickiness?**
  * Stickiness is when you want the same instance to handle the request of the same client. This is done by a **cookie.** 
* **Types of Elastic Load Balancers?**
  * **Classic Load Balancer:**
    * Provides Load Balancing for HTTP, HTTPS and TCP protocols.
    * Not generally promoted by Amazon now.
  * **Application Load Balancer:**
    * Provides Load Balancing between multiple applications of the same machine.
    * Generally supports HTTP and HTTPS protocols.
  * **Network Load Balancer:**
    * Useful to handle low latency situations. 
    * Supports TCP, UDP and TLS 
    * One static IP per AZ
    * Handles millions of requests per second
* **What is Cross Zone Load Balancing?**
  * Load Balancing done between the instances that are registered with different AZ. 
  * Enabled by default in ALB
  * Not in NLB. \(requires $\)
* **What is SNI? Server Name Indication**
  * Multiple sites can be present on an instance with multiple certificates. the ALB has to make a decision based on SNI which is in HTTP Header, which certificate to server the client and which instance the traffic to forward to.
* **What is Connection Draining**?
  * When a connection is being terminated with an EC2 by deeming unhealthy, the LB decides to complete the remaining connections of the instance before it deregisters.
* **How are instances terminated between AZ?**
  * AZ with most number of instances will be chosen then the instance with oldest configurations is terminated.
* **What is Life Cycle hooks?**
  * Allows us to connect with the instances just before they are being terminated or being started. 

\*\*\*\*

## Questions

* [ ] What is High Availability?
* [ ] What is Scalability?
* [ ] What is Vertical and Horizontal Scaling?
* [ ] What is Auto Scaling?
* [ ] What are Auto Scaling groups?
* [ ] What is maximum/minimum/desired number of instances in Auto Scaling?
* [ ] What is Auto Scaling alarms?
* [ ] What are the Scaling policies?
* [ ] What are different types of Scaling policies?
* [ ] What is Elastic Load Balancer \(ELB\)? What does it do?
* [ ] What is Load Balancing Stickiness?
* [ ] Types of Elastic Load Balancers?
* [ ] What is Cross Zone Load Balancing?
* [ ] What is SNI? Server Name Indication
* [ ] What is Connection Draining?
* [ ] How are instances terminated between AZ?
* [ ] What is Life Cycle hooks?

