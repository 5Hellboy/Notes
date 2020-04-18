# Identity & Access Management\(IAM\)

## Identity & Access Management

AWS security can be categorized into three parts:

* Users:
  * A user is basically a person who has an account.
* Groups:
  * Functions or teams that have a certain permissions. The users that belong to a certain team are in that group. Each function or team can have certain permissions. 
* Roles:
  * Is a set of activities that the user can and cannot do. What resources the users can use and what they can't use.

Root user of IAM should never be shared with anyone. Basically don't use root user for anything. To perform super user activities, you should create a user and give him administrative privileges. Users should be created with **proper permissions**.

IAM policies can be configured using JSON. 







