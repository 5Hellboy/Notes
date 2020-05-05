# RDS/Aurora/ElastiCache

### RDS - Relational Data Base 

* Uses SQL as a query language
* It allows you to create databases in the cloud that are managed by AWS
  * Postgress
  * MySQL
  * MariaDB
  * MSSQL 
  * Aurora
* Why RDS instead of using DB on EC2?
  * Automated provisioning, OS patching
  * Continuous backups and restore to specific timestamps 
  * Monitoring Dashboards
  * Read replicas for improved read performance.
  * Multi AZ setup for DR \(Disaster Recovery\)
  * Maintenance windows for upgrades.
  * Scaling capability \(vertical and horizontal\)
  * Storage backend by EBS \(gp2 or io 1\)
* But you can SSH into your instances.
* * RDS Backups:
  * Automatically enabled in RDS.
  * Automated Backups:
    * Transaction logs are backed up by RDS every 5 minutes 
    * =&gt; ability to restore to any point in time \(from oldest backup to 5 minute ago\)
    * 7 days retention \(can be increase to 35 days\)
  * DB Snapshots:
    * Manually triggered by the user.
    * Retention of backup for as long as you want.



### Read Replicas vs Multi AZ:\*\*\*

#### RDS Read Replicas for read scalability

* To scale your reads.
* We can create upto 5 read replicas. 
* Can be withing same AZ, Cross AZ or Cross Region
* Replication is ASYNC so reads are eventually consistent. So if an application is reading the data before the replication happened then it will get **old data** and not the fresh one.
* Replicas can also be promoted to their own databases.
* Applications must update the connection string to leverage read replicas.

**RDS Read Replicas - Use Cases**

* You have a production database that is taking on normal load. 
* You want to run a reporting application to run some analytics.
* You dont want t a production dB to handle the reporting application requests.
* You create a Read replica to run the new workload there.
* The production application is unaffected.
* Read replicas are used for SELECT queries and not INSERT, UPDATE or DELETE queries.

**Read Replicas - Network Cost**

* There is a network cost when your data goes from one AZ to another AZ.
* ASYNC replication from one AZ to other AZ will cost us money.
* If Read replica is within the same AZ then reduce the cost \(its free\)

**RDS Multi AZ**

* **SYNC replication**
* Replicates every single read and write to the standby RDS DB.
* One DNS name for application - automatic failover to standby.
* Increase availability
* Failover in case of loss of AZ, loss of network, instance or storage failure.
* No manual intervention in apps
* Not used for scaling.

**The Read Replicas be setup as Multi AZ for Disaster Recovery \(DR\)**

\*\*\*\*

### **RDS Security - Encryption**

* **At rest encryption:**
  * You can encrypt the master and the read replicas with AWS KMS - AES 256 encryption.
  * Encryption has to be defined at launch time.
  * **If the master is not encrypted, the read replicas** _**cannot**_ **be replicated.**
  * Transparent Data Encryption \(TDE\) available for Oracle and SQL Server.
* **In-flight encryption**
  * SSL certificates to encrypt data to RDS in flight.
  * Provide SSL options with trust certificates when connecting to database.
  * To enforce SSL:
    * There is a parameter in PostgreSQL
    * And there is a command in MySQL

**RDS Encryption Operation**

* **Encryption RDS backups:**
  * Snapshots of un-encrypted RDS databases are un-encrypted
  * Snapshots of encrypted RDS databases are encrypted
  * Can copy a snapshot into an encrypted one
* **To encrypt an un-encrypted RDS database:**
  * Create a snapshot of the un-encrypted database
  * Copy the snapshot and enable encryption for the snapshot
  * Restore the database from the encrypted snapshot
  * Migrate applications to the new database, and delete the old database.

**RDS Security - Network & IAM**

* Network Security
  * RDS databases are usually deployed within a private subnet, not in a public one
  * RDS security works by leveraging security groups - it controls which IP / security group can communicate with RDS
* Access Management
  * IAM Policies help control who can manage AWS RDS \(through RDS API\)
  * Traditional Username and Password can be used to login into the database.
* IAM-based authentication can be used to login into RDS MySQL & PostgresSQL

**IAM Authentication:**

* To connect to MySQL and PostgreSQL, we can use IAM database authentication. This authentication is done using authentication token obtained through IAM & RDS API calls.
* Auth token has lifetime of 15 minutes.
* * Network in/out traffic must be encrypted using SSL
* IAM to centrally manage users isntead of DB
* Can leverage IAM Roles and EC2 instance profiles for easy integrations.

### AMAZON AURORA:

* Aurora is a proprietary technology by AWS.
* PostgreSQL and MySQl are both supported as Aurora DB \(that means your drivers will work as if Aurorra was postgres or MySQL database\)
* Aurora is "AWS cloud optimized' and claims **5x performance improvement over MySQL** on RDS, over 3**x the performance of Postgress** on RDS
* Can automatically grow in increments of 10GB, up to 64 TB
* Aurora can have 15 replicas and replication process is faster
  * MySQL can have 5 replicas
* Failover is instantaneous. It is highly available.
* Aurora costs more than RDS \(20% more\) - but it is more efficient

**High Availability and Read Scaling**

* 6 copies of your data acrross 3 AZ:
  * 4 copies out of 6 needed for writes
  * 3 copies out of 6 need for reads
  * Self healing with peer to peer replication
  * Storage is striped across 100s of volumes
* One Aurora instance takes writes\(master\)
* Automated failover for master in less than 30 seconds.
* Master + up to 15 Aurora Read Replicas server reads
* Support for Cross Region Replication

**Aurora DB Cluster**

* Read replicas can be auto scaled.
* Writer endpoint: 
  * DNS name that always points to the master
  * Pointing to the master
* Reader Endpoint: connection load balancing. 
  * It connects automatically to all read replicas

**Aurora Security**

* Similar to RDS because it uses the same engine.
* Encryption at rest using KMS
* Automated backups, snapshots and replicas are also encrypted.
* Encryption in flight using SSL \(same process as MySQL and Postgres\)
* Possibility to authenticated using IAM token \(same method as RDS\)
* You are responsible for protecting the instance with security groups.
* You can't SSH

**Aurora Serverless**

* Automated database instantiation and auto-scaling based on actual usage.
* Good for infrequent intermittent or unpredictable workloads.
* No capacity planning needed
* Pay per second can be more cost effective.

**Global Aurora**

* **Aurora Cross Region Read Replicas:**
  * Used for disaster recover
  * Simple to put in place
* Aurora Global Database \(recommended\)
  * 1 Primary Region \(read/write\)
  * Up to 5 secondary \(read - only\) regions, replication lag is less than 1 second.
  * Up to 16 Read Replicas per secondary region \(ASNYC\)
  * Helps for decreasing latency
  * Promoting another region \(for disaster recovery\) has an Recovery time objective \(RTO\) of &lt; 1 minute.

### AWS ElasticCache 

* RDS for caches
* ElastiCache is to get manged  Redis or Memcached
* Caches are in memory databases with really high performance, low latency
* Helps reduce load off the databases for read intensive workloads.
* Helps make your application stateless
* Write scaling using sharding
* Read Scaling using Read Replicas
* Multi AZ with Failover Capability
* AWS takes care of OS maintenance/patching, optimizations, setup, configs, monitoring, failure recovery and backups

**ElastiCache SA - DB Cache**

* It acts as a middle man between application and Amazon RDS. 
* If Application queries are in cache then it will be a cache hit and the application will not talk to RDS but it will talk to cache.
* This helps relieve load off the RDS.
* Cache must have an invalidation strategy to make sure only the most current data is used in there.

**ElastiCache SA - User Session Store**

* The user logs into any of the application.
* The application writes the session data into ElastiCache
* The user hits another instance of our application
* The instance retrieves the data and the user is already logged in.

**ElastiCache - Redis vs Memcached**

* **Redis**
  * **Multi AZ** with Auto-Failover
  * **Read Replicas** to scale reads and have **high availability**.
  * Data Durability using AOF persistence.
  * **Backup and restore features**
* **Memcached**

  * Multi-node for portioning of data \(sharding\)
  * **Non persistent**
  * **No backup and restore**
  * Multi threaded architecture

### ElastiCache - Redis vs Memcached

* All caches in ElastiCache:
  * Support SSL in flight encryption
  * Do not support IAM authentication
  * IAM policies on ElastiCache are only used for AWS API-level security
* Redis AUTH
  * You can set a "password/token" when you create a redis cluster
  * This is an extra level of security for your cache on top of security.
* Memcached
  * Supports SASL-based authentication \(advanced\)

**Patterns for ElastiCache:**

* **Lazy Loading:**
  * All the read data is cached, data can become stale in cache
* **Write Through:**
  * Adds or update data in the cache when written to DB \(no stale data\)
* **Session Store:**
  * store temporary session data in a cache \(using TTL features\)

\*\*\*\*

\*\*\*\*







