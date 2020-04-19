---
description: Amazon Elastic Compute Cloud
---

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











```text
To connect to EC2 using SSH, make sure you set the permissions of the SSH key for user ec2-user to 0400.
```

