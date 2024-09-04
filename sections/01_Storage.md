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
  - [S3 - Lifecycle Rules - Hands On](#s3---lifecycle-rules---hands-on)
  - [S3 - Event Notifications](#s3---event-notifications)
    - [S3 - Event Notifications – IAM Permissions](#s3---event-notifications--iam-permissions)
    - [S3 - Event Notifications with Amazon EventBridge](#s3---event-notifications-with-amazon-eventbridge)
  - [S3 - Event Notifications - Hands On](#s3---event-notifications---hands-on)
  - [Amazon S3 - Performance](#amazon-s3---performance)

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

Let's create a new bucket in S3 and call it s3-storage-classes-demo-2025-v1. I'll choose any region and go ahead and create this bucket.

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


### S3 - Lifecycle Rules - Hands On

So let's go ahead and create a lifecycle rule for our bucket. 

![](../img/02/03.png)

Let's go under Management and create a lifecycle rule. 

![](../img/02/04.png)

This one is going to be called DemoRule, and we apply it to all the objects in the bucket, and I acknowledge it.

![](../img/02/05.png)

you can see we have five different rule actions. We can move current versions of objects between storage classes, move non-current versions of objects between storage classes, expire current versions of objects, permanently delete non-current versions of objects, and finally, delete expired objects, delete markers, or incomplete multi-part uploads. So, five different use cases. Let's have a look at them one by one.

**move current version objects between storage classes**, 

it means that you have a versioned bucket, and the current version is the most recent version—the one displayed to the user. For example, we can transition to standard IA after 30 days. Then, we can move to each IA after 60 days. After that, we can go into Glacier for instant retrieval after 90 days. Then, after 180 days, we can move to flexible retrieval. Finally, we might archive to deep archive after 365 days. You can have as many transitions as you want, okay? And we need to check the box to acknowledge what we do.

![](../img/02/06.png)

**move non-current versions faster**. 

For this, we want to move an object that is non-current—meaning an object that has been "overridden" by a newer one. We could say, "Okay, we want to move it to Glacier flexible retrieval because we know that after 90 days, we won't need it for immediate retrieval, so this is perfect." And we're good to go, but we could add more transitions.

![](../img/02/07.png)

![](../img/02/08.png)

For example, we might want to **expire current versions of objects**, and at the bottom, you can set it up to happen after 700 days. 

![](../img/02/09.png)  
![](../img/02/10.png)  

Similarly, for non-current objects, we want to permanently delete them after 700 days as well, okay? So, this is something we can do.

![](../img/02/11.png)
![](../img/02/12.png)

Now we can have a look at all these transitions and expiration actions. This is nice because it shows you a timeline of what is going to happen to your current and non-current versions of your objects. 

![](../img/02/13.png)

If we're happy with all of this, we can just go ahead and create this rule. This rule will act in the background to do what it's supposed to be doing.

That's it. Now you know how to automate moving objects in our industry between different storage classes. I hope you liked it, and I will see you in the next lecture.

![](../img/02/14.png)

### S3 - Event Notifications

So now let's talk about S3 event notifications. The idea is that new events are going to happen in Amazon S3. What are events? Well, events are things such as when an object is created, an object is removed, an object is restored, or there is replication happening. You can filter these events. For example, you can specify that you only want to consider objects that end with ".jpeg".

The use case for event notifications would be, for example, to automatically react to certain events happening in Amazon S3. For instance, if you want to generate thumbnails of all the images uploaded to Amazon S3, you can create an event notification and send it to a couple of destinations. These destinations could be an SNS topic, an SQS queue, or a Lambda function. Don’t worry if you are not familiar with these services yet; we will cover them in the next lectures.

You can create as many S3 events as you desire, and you can send them to any target you want. The idea is that these events are typically delivered within seconds to their destinations, but sometimes it can take a minute or longer.


![](../img/02/15.png)


#### S3 - Event Notifications – IAM Permissions

For the event notifications to work, we need to have the correct IAM permissions. For example, if the S3 service is sending data to an SNS topic, we need to attach what’s called an SNS Resource Access Policy. This is an IAM policy that you attach to the SNS topic, allowing the S3 bucket to send messages directly to the SNS topic. Similarly, if we use SQS, we create an SQS Resource Access Policy that authorizes the S3 service to send data to our SQS queue.

Finally, for a Lambda function, we need a Lambda Resource Policy attached to our Lambda function to ensure that Amazon S3 has the right to invoke our function. Here, we don’t use IAM roles for Amazon S3; instead, we define Resource Access Policies on the SNS topic, the SQS queue, or the Lambda function. These policies function similarly to the S3 bucket policy we used before.

So, you should remember that SNS, SQS, and Lambda functions are event notification targets. 

![](../img/02/16.png)

#### S3 - Event Notifications with Amazon EventBridge

But now, there's a fourth integration that you will also learn about. All events from your Amazon S3 bucket end up in Amazon EventBridge, no matter what. From EventBridge, you can set up rules to send these events to over 18 different AWS services as destinations. This really enhances the capability of S3 event notifications.

We will explore EventBridge later on in this course, but with EventBridge, you get advanced filtering options—much more than before. You can filter by metadata, object size, and name, send to multiple destinations simultaneously, set up step functions, use data streams or Firehose, and access features directly from Amazon EventBridge, such as archiving events, replaying events, and achieving more reliable delivery.

Okay, there’s a lot we haven’t covered in this lecture about the new services, but let's just focus on Amazon S3 event notifications. The idea is that you can react to events happening in Amazon S3 by sending them to SQS, SNS, Lambda, or Amazon EventBridge.

![](../img/02/17.png)

* **Advanced filtering** options with JSON rules (metadata, object size, name...)
* **Multiple Destinations** – ex Step Functions, Kinesis Streams / Firehose…
* **EventBridge Capabilities** – Archive, Replay Events, Reliable delivery

### S3 - Event Notifications - Hands On

So let's go ahead and demonstrate S3 event notifications. For this, I'm going to create a bucket. 

![](../img/02/18.png)

I'll call it `demo-alexjust--event-notifications`. Once I'm ready, I will just go ahead and create my bucket.

![](../img/02/19.png)

click **`Create bucket`**

Okay, so my bucket is created. I'm going to go into it. Now, I'm going to make sure that event notifications are set up. So I go to **`Properties`**, scroll down, and then here we have `event notifications`.

As you can see, we have two options. Number one is to `create an event notification`, and I will show you this in a second. Number two is to enable the `Amazon EventBridge` integration to send all events from this S3 bucket to EventBridge. 

![](../img/02/21.png)

To do this, you just turn it `on`, and you're good to go.

![](../img/02/22.png)

If I wanted to, I could use Amazon EventBridge to capture the events happening in my S3 bucket.

However, I'll show you the simpler way first, because it's a bit more straightforward, which is to just create an `event notification` and send it, for example, to SQS.

![](../img/02/23.png)

I'll call this one demo-event-notification. Then, we can set up a prefix or a suffix, but I won't do that now. 

![](../img/02/24.png)

Next, we need to choose event types. We want to react to all object create events. This means that any time an object is created, an event is going to be triggered. If you want, you could get more granular and select specific types of events, but to keep it simple, I'll leave it at that. You can also include, for example, object removals or object restores, and on the right-hand side, it shows you all the events you can catch. I'll keep it simple and just scroll down. As you can see, there are lots of different events you can react to in Amazon S3.

![](../img/02/25.png)

Then, you need to publish to a destination. We have three options: Lambda functions, SNS topics, and SQS queues. I'm going to choose SQS queue, but first, we need to create a queue and authorize Amazon S3 to publish messages to that destination. 

![](../img/02/26.png)

So, what I'm going to do now is go into Amazon SQS and create a queue. 

![](../img/02/27.png)

I'll call this one `demo-S3-notification`. I'll go ahead and **`create queue`**.

![](../img/02/28.png)

and it's created. 

![](../img/02/29.png)

Now, I need to update the access policy to allow my S3 bucket to write to my SQS queue.

To demonstrate the issue, if I go back here and refresh the page to see my queue appear, 



I refresh it, select `demo-S3-event`, choose `all object create events`, scroll down, and then choose the SQS queue. I can select the queue from the dropdown, demo-S3-notification. 

![](../img/02/30.png)


However, if I try to save my changes, I get an unknown error saying that it can't validate the destination configuration 

![](../img/02/31.png)

because this SQS queue does not yet accept messages from my S3 bucket.

![](../img/02/33.png)

To resolve this, I need to change the access policy by clicking on "Edit." 

Scroll down to where the access policy is, and we need to generate a new policy. 

![](../img/02/34.png)

So, I go to the policy generator. It will be an SQS queue policy, 
* Princial : **`*`**
* Actions : **`send messages`**
* Amazon Resource Name (ARN) : ![](../img/02/35.png)

I add a statement and then generate this policy. 


![](../img/02/36.png)

![](../img/02/37.png)

Now, this is the policy I want to use, which allows anyone to write to my SQS queue. It's very permissive, but it will work. Click below to edit. To save the policy, copy the text below to a text editor. Changes made below will not be reflected in the policy generator tool.


![](../img/02/38.png)

Changes the policy generator tool.

![](../img/02/42.png)


Let's **`save`** this, and now my access policy has been updated.



So, if I go back and try to save my changes again, 

![](../img/02/31.png)


**`save changes`** as you can see, the operation was successfully completed. 


![](../img/02/43.png)

What happened is that if I go into my SQS queue and click on "Send and receive messages," 

![](../img/02/44.png)

then click on "Poll for messages," 

![](../img/02/45.png)

you can see a message was sent by Amazon S3 to test the connectivity. 

![](../img/02/46.png)

I can take this message and delete it.

![](../img/02/47.png)

Now, we want to test whether the S3 event notification is working with SQS. So, we're going to upload an object. 

![](../img/02/48.png)

Click on "Add files" and choose our coffee.jpeg. I will upload this file. Now, the file has been uploaded, and if I go into my bucket, I can see that my coffee.jpeg has been created.

![](../img/02/49.png)

`uploaded`

and if I go into my bucket, 

![](../img/02/50.png)

I can see that my coffee.jpeg has been created.

![](../img/02/51.png)



Imagine we want to automate this process and create a thumbnail from it. We would need to have a message in our SQS queue to process it and create a thumbnail.

Therefore, I'm going to poll for messages again, and as you can see, a message was created here.

![](../img/02/45.png)

you can see a message was sent by Amazon S3 to test the connectivity. 

So, the object was indeed created. If we look deeper, we'll see that the **`key`** of that message is **`coffee.jpeg`**. So, the coffee.jpeg was created, and it generated a whole event in my SQS queue. This demonstrates the power of S3 event notifications.

![](../img/02/53.png)

I can delete the message, and we're done.

![](../img/02/47.png)

Okay, that's it. We've seen S3 event notifications. Remember, you can send notifications to SQS, SNS, Lambda, and Amazon EventBridge for further processing and sending to more destinations.

`Amazon S3` > `Buckets` > `demo-alexjust-event-notifications`

![](../img/02/54.png)



### Amazon S3 - Performance

