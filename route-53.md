---
description: 53 = DNS port?
---

# Route 53

**Route 53 is a managed DNS system provided by AWS**

In AWS, most common DNS records are:

* A: hostname to IPV4
* AAAA: hostname to IPV6
* CNAME: hostname to hostname
* Alias: hostname to AWS resource

DNS TTL - DNS cache time to live 



CNAME vs Alias?

* AWS resources expose an AWS hostname  \(ageagea.amazonaws.com\) and you want to expose your domain myname.mylastname.com
* CNAME:
  * Points a hostname to any other hostname. \(myname.mylastname.com -&gt; something.something.com\)
* WORKS ONLY FOR NON ROOT DOMAIN i.e myname.mylastname.com and not mylastname.com
* * Alias:
  * Points a hostname to an AWS resource \(myname.mylastname.com -&gt; ageagea.amazonaws.com\)
  * Works for ROOT DOMAIN and NON ROOT DOMAIN 
  * Free of charge
  * Native health checks enabled

**Routing Policy:**

* Use to redirect to a single resource in simple routing policy.
* In multiple routing policy, multiple resources are selected and the client is selected at random. \(you add multiple ip address in A record\)

**Weighted Routing Policy:**

* Route 53 sends the % of traffic to go to specific endpoint. 
* New app version that wants to test 10% of traffic.

**Latency Routing Policy:**

* Redirect to the server that has the least latency to us.
* Super helpful when latency of users is a priority.
* Latency is evaluated in terms of user to designated AWS region.
* Germany may be directed to the US \(if thats the lowest latency\)

**Health Checks:**

If an instance is unhealthy then route 53 will not send traffic to that instance. 

* If it fails X heath checks then its unhealthy, same with pass
* Default health check intervals: 30s
* about 15 health checkers will check the endpoint health.
* Health checks can be linked to the route53 DNS queries.

**Failover Routing Policy:**

* If health check fails then Route 53 will failover to a secondary endpoint.

**Geolocation routing policy:**

* Traffic that originates from an area affects the routing policy.
* Should create a default policy in case theres no match.

**MultiValue Routing Policy:**

* Use when routing traffic to multiple resources.
* Want to associate a Route 53 health checks with records.
* Up to 8 healthy records are returned for each multi value query.
* Multi value is not a substitute for having an ELB.

**3rd Party Domains in Route 53**

* Route 53 is also a registrar. 
*  If you buy your domain on a 3rd party website, you can still use Route53

  * By creating a Hosted Zone in Route 53
  * Update NS records on 3rd party website to use Route 53 name server.





