# High Availability and Scalability: ELB & ASG

## High Availability 

* GOes hand in hand with horizontal scaling.
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

* 


\*\*\*\*





\*\*\*\*



