# Identity & Access Management\(IAM\)

## Identity & Access Management

AWS security can be categorized into three parts:

* Users:
  * A user is basically a person who has an account. 
  * **One user per physical person.**
* Groups:
  * Functions or teams that have a certain permissions. The users that belong to a certain team are in that group. Each function or team can have certain permissions. 
* Roles:
  * Is a set of activities that the user can and cannot do. What resources the users can use and what they can't use. 
  * **One IAM role per application.**

Root user of IAM should never be shared with anyone. Basically **never use root user for anything** except first setup. To perform super user activities, you should create a user and give him administrative privileges. Users should be created with **proper permissions**.

IAM permission policies can be configured using JSON. 

IAM has a global view i.e all the rules,users and groups created will be created globally.

> ### Its best to give users minimal privileges that they need to perform the task. Don't overpower any user.

IAM Federation: Big companies that have corporate credentials can integrate them with AWS using IAM federation. This integration is done using SAML standard. 







