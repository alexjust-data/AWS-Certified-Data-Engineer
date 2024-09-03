**Storage**

- [Set up an AWS Billing Alarm](#set-up-an-aws-billing-alarm)
- [Amazon S3](#amazon-s3)
  - [S3 - Hands On](#s3---hands-on)
  - [S3 - Security - Bucket Policy](#s3---security---bucket-policy)
  - [S3 - Versioning Overview](#s3---versioning-overview)
  - [S3 - Versioning Hands on](#s3---versioning-hands-on)
  - [S3 - Replication](#s3---replication)
  - [S3 - Replication Hands On](#s3---replication-hands-on)
  - [S3 - Storage Classes Overview](#s3---storage-classes-overview)
  - [S3 - Storage Classes Hands On](#s3---storage-classes-hands-on)
  - [S3 - Lifecycle Rules](#s3---lifecycle-rules)

## Set up an AWS Billing Alarm

`Billing and Cost Management` > `Budgets` > Overview

... and create a new budget.

* `Zero spend budget` : Create a budget that notifies you once your spending exceeds $0.01 which is above the AWS Free Tier limits.
* `Monthly cost budget` : Create a monthly budget that notifies you if you exceed, or are forecasted to exceed, the budget amount.

## Amazon S3

### S3 - Hands On

https://github.com/alexjust-data/AWS_Certified_Cloud_Practitioner?tab=readme-ov-file#s3-hands-on



### S3 - Security - Bucket Policy

https://github.com/alexjust-data/AWS_Certified_Cloud_Practitioner?tab=readme-ov-file#s3-security-bucket-policy-hands-on

### S3 - Versioning Overview

https://github.com/alexjust-data/AWS_Certified_Cloud_Practitioner?tab=readme-ov-file#s3---versioning-overview

### S3 - Versioning Hands on

https://github.com/alexjust-data/AWS_Certified_Cloud_Practitioner?tab=readme-ov-file#s3---versioning-hands-on


### S3 - Replication

https://github.com/alexjust-data/AWS_Certified_Cloud_Practitioner?tab=readme-ov-file#amazon-s3--replication

**Replication (Notes)**
* After you enable Replication, only new objects are replicated
* Optionally, you can replicate existing objects using S3 Batch Replication
  * Replicates existing objects and objects that failed replication
* For DELETE operations
  * Can replicate delete markers from source to target (optional setting)
  * Deletions with a version ID are not replicated (to avoid malicious deletes)
* There is no “chaining” of replication
  * If bucket 1 has replication into bucket 2, which has replication into bucket 3
  * Then objects created in bucket 1 are not replicated to bucket 3

### S3 - Replication Hands On

https://github.com/alexjust-data/AWS_Certified_Cloud_Practitioner?tab=readme-ov-file#amazon-s3--replication


### S3 - Storage Classes Overview

https://github.com/alexjust-data/AWS_Certified_Cloud_Practitioner?tab=readme-ov-file#s3-storage-classes-overview

### S3 - Storage Classes Hands On

https://github.com/alexjust-data/AWS_Certified_Cloud_Practitioner?tab=readme-ov-file#s3-storage-classes-hands-on

### S3 - Lifecycle Rules

![](../img/02/01.png)

So now let's talk about how we can move objects between different storage classes, so you can transition them, and this is the diagram of how it's possible. As you can see, you can go from the standard, for example, to standard IA, to intelligent tiering, to one-zone IA, and then from one-zone IA, as you can see, you can go to flexible retrieval or deep archive, and so all the types of permutations are shown in this graph. As a matter of fact, if you know that your objects are going to be infrequently accessed, then move them to standard IA, and if you know that you're going to archive objects, move them into the glacier of tiers or the deep archive tier.

Now, moving these objects can be done manually, of course, but we can automate this using lifecycle rules. These lifecycle rules are made up of multiple things. The first thing is a transition action to configure the object to transition to another storage class. For example, you could say move to standard IA class 60 days after creation, or move to Glacier for archiving after six months. You can also set up expiration actions to configure objects to be deleted or expired after some time. For example, your access log files may be set to delete after 365 days. Or you could, for example, use an expiration action to delete all versions of files if you have enabled versioning. Alternatively, you can use this to delete incomplete multi-part uploads if, for example, the multi-part upload is more than two weeks old because, well, the thing should have been fully uploaded by now.

* `Transition Actions` – configure objects to transition to another storage class
  * Move objects to Standard IA class 60 days after creation
  * Move to Glacier for archiving after 6 months
* `Expiration actions` – configure objects to expire (delete) after some
time
  * Access log files can be set to delete after a 365 days
  * Can be used to delete old versions of files (if versioning is enabled)
  * Can be used to delete incomplete Multi-Part uploads
* Rules can be created for a certain `prefix` (example:s3://mybucket/mp3/*)
* Rules can be created for certain `objects` Tags (example: Department:
Finance)

**Scenarios**

Here are some scenarios. For example, you have an application on EC2, and it creates images or thumbnails after profile photos are uploaded to Amazon S3. But these thumbnails can be easily recreated from the original photo, and they only need to be kept for 60 days. However, the source images should be immediately retrievable for these 60 days, and afterward, the user can wait up to six hours. How would you design this? This is what an exam question might ask you.

The S3 source images can be on the standard class with a lifecycle configuration to transition them to Glacier after 60 days. The thumbnail images—this is how you use a prefix to differentiate between source images and thumbnails, for example—can be on one zone IA, because while they are infrequently accessed, they can be easily recreated. You can have a lifecycle configuration to expire or delete them after 60 days.


Lifecycle Rules (Scenario1)

* Your application on EC2 creates images thumbnails after profile photos are uploaded to Amazon S3. These
thumbnails can be easily recreated, and only need to be kept for 60 days. The source images should be able to be immediately retrieved for these 60 days, and afterwards, the user can wait up to 6 hours. How would you design this?
* S3 source images can be on Standard, with a lifecycle configuration to transition them to Glacier after 60 days
* S3 thumbnails can be on One-Zone IA, with a lifecycle configuration to expire them (delete them) after 60 days

Lifecycle Rules (Scenario 2)

Another scenario: a rule in your company states that you should be able to recover deleted S3 objects immediately for 30 days, although this may happen rarely. After this time, for up to 365 days, deleted objects should be recoverable within 48 hours. For this, we can enable S3 versioning to keep and have object versions so that the deleted objects are, in fact, hidden by a delete marker and can then be recovered. Then you will create a rule to transition the non-current versions of the objects to standard IA, meaning the versions that are not the top-level versions, and then transition these non-current versions to Glacier Deep Archive for archival purposes.

* A rule in your company states that you should be able to recover your deleted S3 objects immediately for 30 days, although this may happen rarely. After this time, and for up to 365 days, deleted objects should be recoverable within 48 hours.
* Enable S3 Versioning in order to have object versions, so that “deleted objects” are in fact hidden by a “delete marker” and can be recovered
* Transition the “noncurrent versions” of the object to Standard IA
* Transition afterwards the “noncurrent versions” to Glacier Deep Archive

**Storage Class Analysis**

Lastly, how do we determine the optimal number of days to transition an object from one class to another? You can do this thanks to Amazon S3 Analytics, which provides recommendations for standard and for standard IA. It does not work with Amazon IA or Glacier. The S3 bucket will have S3 Analytics run on top of it, and this will create a CSV report that provides recommendations and statistics. The report is updated daily, and it can take between 24 to 48 hours to start seeing data analysis results. This CSV report is a good first step in creating lifecycle rules that make sense or in improving them.

![](../img/02/02.png)