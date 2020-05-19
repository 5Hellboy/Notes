# Amazon S3

### Overview 

* Amazon S3 allows people to store objects in buckets \(defined at region level\) 
* Has naming convention
* and has a globally unique name
* Objects have a Key and the key is a full path:
  * s3://somethnig-bucket/filename.txt
  * s3://somethnig-bucket/some\_folder/filename.txt
* The kys is composed of a prefix + object name
  * prefix : some\_folder
  * object name: my\_file
* No concept of directories just keys with very long names that contain slashes



### S3 Versioning

* You can version your files in Amazon S3
* It is enabled at default level 
* Same key overwrite will increment the version: 1,2,3
* It is best practice to version your buckets 
  * Protect against unintended deletes \(ability to restore a version\)
  * Easy roll back to previous version
* Notes:

  * Any file that is not versioned prior to enabling versioning will have version "null"
  * Suspending versioning does not delete the previous versions \(adds a delete marker to hide the file\)
  * You have to delete the delete marker to restore the file back.
  * Delete the version ID to delete the that specific version.

### S3 Encryption for Objects

* There are 4 methods of encryption objects in S3
* SSE-S3: encrypts S3 objects using keys handled and managed by AWS
* SSE\_KMS: leverage AWS Key management service to manage encryption keys
* SSE-C: when you want to manage your own encryption keys
* Client side encryption



### S3 Security

* User based security:
  * IAM policies
* Resource Based:
  * Bucked policies
  * Object ACL
  * Bucket ACL
* IAM principal can access an S3 bucket if the IAM permission allows it OR the resource policy ALLOWS it and theres no explicit deny.
* These are json based policies that deal with bucket security.

### S3 Websites

* S3 can host static website and have them accessible on the www
* The website will be 
  * &lt;bucket-name&gt;,s3-website-&lt;AWS-region&gt;.amazonaws.com

### CORS - Cross origin resource sharing

* Get resources from different origin
* you can get resources from other origin only when other origin allows you to get resources.
  * Same origin: http://example.com/app1 & http://example.com/app2
  * Different origin: http://example.com/ & http://other.example.com
* The request wont be fullfilled unless origin allows for CORS header: Access control allow origin
* CORS headers have to defined on cross origin bucket and not the first origin bucket.
* 
### Amazon S3 consistency models:

* Read after write consistency for PUTS of new objects
  * PUT 200 =&gt; GET 200
* This is true if we did a GET before to see if the object exitsted
  * GET 404 -&gt; PUT 200 -&gt; GET 404 - eventually consistent
* Eventual consistency for deletes and puts of existing objects
  * If we read an object after updating, we might get the older version
  * If we delete an object, we might still be able to retrieve it for a short time

**MFA-Delete: We can set up MFA to stop deletions and version controls.** 

**S3 Default encryption and bucket policies:**

* **Bucket policy is the old way and Default encryption settings is the new way.**

**S3 Access Logs:**

* Never set your logging bucket to be the monitored bucket.
* It will create a loop and your bucket will grow in size exponentially.

**S3 Replication**

* Must enable versioning in source and destination
* Cross Region Replication \(CRR\)
* Same Region Replication \(SRR\)
* Buckets can be in different accounts
* Copying is asynchronous
* Must give proper IAM permissions to S3

CRR - Use case: Compliance, lower latency access, replication across accounts

SRR - Use cases: log aggregation, live replication between production and test accounts.

\*\*\*\*

