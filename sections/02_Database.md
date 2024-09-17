**Database**

- [Amazon DynamoDB](#amazon-dynamodb)
  - [Traditional Architecture](#traditional-architecture)
  - [NoSQL databases](#nosql-databases)
  - [Amazon DynamoDB](#amazon-dynamodb-1)
  - [DynamoDB - Basics](#dynamodb---basics)
  - [DynamoDB – Primary Keys](#dynamodb--primary-keys)
- [DynamoDB - Hands On](#dynamodb---hands-on)
- [DynamoDB in Big Data](#dynamodb-in-big-data)
  - [DynamoDB – Read/Write Capacity modes](#dynamodb--readwrite-capacity-modes)
  - [R/W Capacity Modes – Provisioned](#rw-capacity-modes--provisioned)
  - [DynamoDB – Write Capacity Units (WCU)](#dynamodb--write-capacity-units-wcu)
  - [Strongly Consistent Read vs. Eventually Consistent Read](#strongly-consistent-read-vs-eventually-consistent-read)
  - [DynamoDB – Read Capacity Units (RCU)](#dynamodb--read-capacity-units-rcu)
  - [DynamoDB – Partitions Internal](#dynamodb--partitions-internal)
  - [DynamoDB – Throttling](#dynamodb--throttling)
  - [R/W Capacity Modes – On-Demand](#rw-capacity-modes--on-demand)

`Managing and querying structured and semi-structured data`

Up next, we're going to dive into the details of the various database technologies Amazon offers for storing your structured or semi-structured data. We will cover DynamoDB, which has a lot of features and details these certification exams try to trip you up with. I'll cover Amazon RDS, along with a few specific points about RDS that the exam guide calls out and best practices for using it. We will touch on DocumentDB, MemoryDB for Redis, Amazon Keyspaces for Apache Cassandra, and Amazon Neptune. Then I'll resurface, going into depth on Amazon Redshift. Redshift is Amazon's data warehousing solution, and it's a very important component in data engineering and data analytics that you can expect to see a lot of on the exam. As with every section, we'll wrap up with a quiz featuring example questions to reinforce what you've learned and give some practice in thinking the way the exam expects you to. It's not enough to learn the trivia about each service. You need to think through how to assemble these services and features into larger systems, and these quizzes will give you some experience in doing that. This is one of the longer sections in the course, so let's get started. Let's dive into database systems on AWS.

### Amazon DynamoDB

`NoSQL Serverless Database`

#### Traditional Architecture

Now let's have a look at DynamoDB, which is a NoSQL serverless database. So if you consider the traditional architecture we've seen in and out in this course, we have clients and they connect to an application layer that could be made of an elastic load balancer and EC2 instances that are grouped and scaling with an autoscaling group. And then the data has to be sourced somewhere, so we have a database layer, and it could be using Amazon RDS, which is backed by MySQL or Postgres or these kind of technologies. Now these traditional applications will leverage RDBMS databases, and we do so because we have the SQL query language, it's really good, and then we can define strong requirements about how data should be modeled, because we have tables, we have schemas and so on, we can do joins, aggregations, complex computations, and that's all very good and working. What we get out of it, though, in case of scaling, is mostly vertical scaling, so in case you want a better database, and I'm talking just about database layer right now, if you want to scale it vertically, you need to replace database and get a more powerful CPU, more RAM, or better disk with I.O. And you can do some sort of horizontal scaling, but this is by only increasing the read capability, so either by adding EC2 instances at the application layer, or by adding RDS read replicas at the database layers. But if you add read replicas, you're going to be limited by the number of read replicas you can have, and therefore limited into your horizontal read scaling. And I'm not talking about horizontal write scaling, because you don't have that with RDS.

![](/img/03/01.png)

* Traditional applications leverage RDBMS databases
* These databases have the SQL query language
* Strong requirements about how the data should be modeled
* Ability to do query joins, aggregations, complex computations
* Vertical scaling (getting a more powerful CPU / RAM / IO)
* Horizontal scaling (increasing reading capability by adding EC2 / RDS Read Replicas)



#### NoSQL databases

So introducing to you NoSQL databases, which means not only SQL or non-SQL databases, depending on the definition. So the idea is that they're non-relational databases, and they're going to be distributed, which gives us some horizontal scalability. And some very famous technologies that have non-SQL databases are MongoDB and, of course, DynamoDB. Now, these databases do not support query joins, or have very limited support, and so for simplicity, I just assume that they don't have query joins. Now, all the data that is needed, therefore, must be present in one row in your database. And again, to simplify things, I know they're evolving, but let's just assume that NoSQL databases also do not perform aggregation computations, such as the sum, or the average, and so on. But the good thing out of it is that, thanks to the design, the NoSQL databases will scale horizontally. That means that if you need more write or read capacity, you can, behind the scenes, have more instances, and it will scale really well. So there's no right or wrong for NoSQL versus SQL. It just depends on how you think about your modeling of data, about your application, about your user queries, and about your scaling needs.

* NoSQL databases are non-relational databases and are distributed
* NoSQL databases include MongoDB, DynamoDB, …
* NoSQL databases do not support query joins (or just limited support)
* All the data that is needed for a query is present in one row
* NoSQL databases don’t perform aggregations such as “SUM”, “AVG”, …
* **NoSQL databases scale horizontally**
* There’s no “right or wrong” for NoSQL vs SQL, they just require to model the data differently and think about user queries differently

#### Amazon DynamoDB

So let's talk about DynamoDB now. So DynamoDB is a fully managed NoSQL database, and it's highly available, and has replications across multiple AZs out of the box. So it's a NoSQL database. It's not a relational database, so it's different than RDS. It scales to massive workload, and it's fully distributed. That means that you can scale to millions of requests per second, trillions of rows, and hundreds of terabytes of storage, regardless of your workload. So fast and consistent performance, so that means you get really low latency on retrieval. And it's an AWS service, so it's going to be fully integrated with IAM for security, authorization, and administration. You can enable event-driven programming with DynamoDB Streams, as we'll see in this section. It's low-cost, it has auto-scaling capability, and you have the standard and infrequent access IA table class for different storage tiers. 

* Fully managed, highly available with replication across multiple AZs
* NoSQL database - not a relational database
* Scales to massive workloads, distributed database
* Millions of requests per seconds, trillions of row, 100s of TB of storage
* Fast and consistent in performance (low latency on retrieval)
* Integrated with IAM for security, authorization and administration
* Enables event driven programming with DynamoDB Streams
* Low cost and auto-scaling capabilities

#### DynamoDB - Basics

So let's have a look at the basics of DynamoDB. So DynamoDB is made out of tables, and each table will have a primary key, and we'll see what the primary key can be in the next slides. You must decide what the primary key is before you create your table. Now, each table can have an infinite number of rows, also called items, so I will use rows and items interchangeably in this course, and each item will have attributes. Now, these attributes could be similar to the columns in your table, but these attributes can also be nested, so it's a bit more powerful than columns, and they can be added over time. You'll need to define them all at creation time of your table, and some of them can be null, so it's totally fine for an attribute to be missing in some data. Now, each item or each row will have up to 400 kilobytes of data, so it's a limitation, and data types supported are going to be scalar types, so string, number, binary, Boolean, and null. There's going to be document types, such as list and map, so they give you some kind of nesting capability, and set types, such as string set, number set, and binary set. 

* DynamoDB is made of Tables
* Each table has a Primary Key (must be decided at creation time)
* Each table can have an infinite number of items (= rows)
* Each item has attributes (can be added over time – can be null)
* Maximum size of an item is 400KB
* Data types supported are:
  * Scalar Types – String, Number, Binary, Boolean, Null
  * Document Types – List, Map
  * Set Types – String Set, Number Set, Binary Set

#### DynamoDB – Primary Keys

Now a very important point to understand is how to choose a primary key for DynamoDB, and the exam will definitely test you on the knowledge of this. So you have two options for primary keys, and the first one is called a partition key, which is also called a hashed strategy. So the partition key in this case would be unique for each item, which is very similar to a normal database, and therefore the partition key must be diverse enough so that your data is going to be distributed. For example, if you consider user ID for users table, then the partition key has been user ID, the attributes have been first name, last name, and age, and then you have your first user ID with some attributes being filled out. Your second user ID, as you can see, does not have a last name, but this is fine in DynamoDB. And the third partition key, yet again, has three attributes attached to it. So this is what DynamoDB looks like. It looks like a database for now. It's pretty easy. 

![](/img/03/02.png)

**Option 1: Partition Key (HASH)** 
* Partition key must be unique for each item
* Partition key must be “diverse” so that the data is distributed
* Example: “User_ID” for a users table

**Option 2: Partition Key + Sort Key (HASH + RANGE)**

But you can have a second option of partition key and sort key, also called hash plus range. And now the combination of these two items must be unique for each item. So the data is going to be grouped by partition key, and this is why it's very important to choose a good partition key. So if you consider a user's game table, then user ID for the partition key and game ID for the sort key. Let's see what that means. That means that users can attend multiple games. So we have these four columns attributes, but the first one is going to be our partition key. We want data to be grouped by user ID, and the second one is going to be the sort key. This is going to give us the uniqueness of the combination of partition key and sort key. So both of them are going to be making the primary key, and the rest are going to be attributes. So if you consider a user ID, then it has a sort key, which is the game ID, and then we attribute score 92, result win. Again, another different user ID and another different game ID, so this works as well. We have a lose game with a score of 14. And more interestingly, in this third row, what we have is that we have the same partition key, so row 2 and 3 have the same partition key, but a different sort key. Of course, it is fine for a user to attend multiple games, and so therefore you want the combination of user ID and game ID to be unique, obviously. But it's fine to also have the same partition key and different sort key. And this is why it's super important to choose a really good partition key, so the data is going to be distributed enough. 

![](/img/03/03.png)

* The combination must be unique for each item
* Data is grouped by partition key
* Example: users-games table, “User_ID” for Partition Key and “Game_ID” for Sort Key

**DynamoDB – Partition Keys (Exercise)**

So here's an exercise, and this is what the exam may test you on as well. So you're building a movie database, and you want to choose what is the best partition key that is going to maximize data distribution. Is it movie ID? Is it producer name? Is it leader actor name? Or is it movie language? Well, think about it for a second. What if you choose the first one? What if you choose the second one? And so on. Now the answer is, you choose movie ID, because movie ID is going to be unique for each row, and therefore it's a really good candidate to partition your table by. If you have a movie language as a partition key, then it won't have as many values as you want, and maybe most of your movies are going to be towards English, so it's not going to be a great choice because there's not enough diversity, and there's a skew of data towards one specific value. And so the exam will ask you to choose the best partition key for some tables based on what it means. So always choose the one with the highest cardinality, and the one that can take on the most amount of values. So that's it for a short overview of DynamoDB. We have a long section on it, but let's go over the hands-on to practice a little bit using DynamoDB.

* We’re building a movie database
* What is the best Partition Key to maximize data distribution?
  * movie_id
  * producer_name
  * leader_actor_name
  * movie_language

* “movie_id” has the highest cardinality so it’s a good candidate
* “movie_language” doesn’t take many values and may be skewed towards English so it’s not a great choice for the Partition Key


### DynamoDB - Hands On

And DynamoDB service is going to allow us to create tables. As you can see, we can create tables, not databases, because the database is already created for us, and this is a server that's operating, so we just go ahead and create tables.

![](/img/03/04.png)

![](/img/03/05.png)

 So the table name has to have a name, so we'll enter users, and then we have to define a partition key, and optionally a sort key. So the partition key, I'll use user underscore id, and then we can change the type, so it can be binary, number, or string, but I will leave it as a string. And then we can use a sort key, but I will show you this later on, so currently we'll leave it empty. Now the quick start allows you to start with the default settings, and it sets some settings for you, or you can go ahead and customize the settings, but because we are learning about DynamoDB, let's go ahead and learn about custom settings. So the first thing is around table class, so it's either DynamoDB standard, which is a general purpose table class, which is recommended for most use cases, but if your data is going to be sitting around for a long time, and you're not going to have a lot of reads or writes, then DynamoDB standard IA is going to say that the data is infrequently accessed, and give you some cost optimizations. But right now, we'll use a default, which is DynamoDB standard.  

![](/img/03/07.png)

Capacity calculator, we'll see this later on, because we have to understand how to compute capacity in DynamoDB, but for now, we don't do anything in here. And then two modes for read and write capacity, on-demand or provisioned, and provisioned is within the free tier. Again, we'll have a long discussion around these modes later on, but we'll use provisioned because it is within the free tier. Next, we have to set the read and the write capacity, and I'm going to disable autoscaling for both, so we just provision a fixed read and write capacity, and we have two capacities within the free tier, so I'm going to set up, we have 10, sorry, within the free tier, so I'm going to set up two provisioned capacity units for reads and two for writes, and we'll see how we make them evolve over time. We're now going to create a secondary index for now, because this is something that's going to be advanced later on in this course,

![](/img/03/08.png)

and here we get the estimated cost for this table, which is $1.32 per month. Now, because we are within the free tier, you can disregard this number, okay, but if you are not within the free tier, this would be how much this table would cost you per month. Then we'll encrypt the data at rest using the DynamoDB key. We'll leave it like this, but we have other options, and when you're ready, you click on **Create Table**.

![](/img/03/09.png)
![](/img/03/10.png)

Okay, so my table has now been created.

![](/img/03/11.png)

I can click on it, and I will remove the left panel, and as we can see, we get a lot of information around our table, so around the settings for our table, so which one is the partition key, the sort key, the capacity mode, and there's some replication that created, and so on, so a lot of information, and for us, the important thing is going to be around the items, so if I scroll down, there's like View Items, or if I go to the right-hand side, there's also **View Items**. 

![](/img/03/12.png)

Now, as we can see, I can scan or query my table, but what I'm going to do is just create an item,

![](/img/03/13.png)

so in this Create Item, I can choose a user ID, and we need to define the partition key, so for example, John123, and this is just a user ID, a random one, you can enter whatever you want, and then we can add attributes for John123, so we can have a string, for example, and the string is going to be first underscore name, and the value is going to be, well, John, and then if we have a string, and it's last underscore name, then we can have John Doe, and as you can see, we can have many attributes as we wanted of these types, 

![](/img/03/14.png)

so we're good to go, we're going to just create an item, and as you can see, this could be either through a form or through a JSON document, 

![](/img/03/15.png)

so we'll just keep it as a form and create this item. So now this item, John Doe, is appearing in my table, as you can see, we have user ID, first name, and last name, so what we can do is to start adding a second item,

![](/img/03/16.png)

So let's create a second item.

![](/img/03/17.png)

![](/img/03/18.png)

So as you can see, the request was successful.

So in a RDS SQL database, you would have had a problem saying the column was not defined, some values are null, whatever, but here we're able to get John to have a first name and a last name, and Alice to just have an age and a first name. So in DynamoDB, it is completely fine for you to add attributes over time, the only thing that has to be non-null is the user ID, as you can see, things get null by default, so John has a null age, and Alice has a null last name, but this is fine. So this is the power of DynamoDB, also a risk, but also the power, you can add attributes over time without impacting your previous data, and this is completely fine. You cannot define in DynamoDB a constraint of this column is not null, it just doesn't exist. So we have our first table being created, but it's super easy to keep on creating tables,

![](/img/03/19.png)

so if I go back to DynamoDB and click on Create Table, 

![](/img/03/04.png)

we can create a second table called Users Post, okay, or User Post. Okay, now the partition key is going to be, yet again, user ID, but now we're going to have a sort key, which is post TS, for timestamp. Again, abstract, string, binary, or number. And what we're going to create here is we create a table of data which will have the post of the users. And so, again, we're going to customize the settings

![](/img/03/20.png)

and we're going to choose provisions, but this column is off, and we're going to have a 2 and 2, and then scroll down and create this second table.

![](/img/03/21.png)
![](/img/03/22.png)

**`Create table`**

![](/img/03/23.png)

So as we can see now, I have two tables in my database, and again, no need to say what's happening to the table, what's the underlying database. It's really you can create as many tables as you want within your quote-unquote DynamoDB database, which is done at the region level. So now let's go into User Post, and now we can see that there's a partition key and a sort key being defined.

![](/img/03/24.png)

![](/img/03/25.png)

So if I go to View Items, and then I'm going to create an item, now we need to specify a user ID, for example, john123, and a post timestamp. So, for example, 2021, 10, 09. I'm not sure that's the correct timestamp, but let's hope it is. TS, and then a time, so 123409z. Okay, so next we need to add some content for our post, so I'll say content and say, hello world, this is my first blog. Okay, Now let's create this item

![](/img/03/26.png)

and as we can see now, we have user ID john and post timestamp this, and some contents. So now what happens if we create a second item? 

![](/img/03/27.png)

Well, we can keep john123 as my user ID, and for the post timestamp, I'm going to just have 2021, maybe something a bit later, so 1104t, and then, again, just a random timestamp. Okay, and we're going to add a new content and say second post, yay. Okay, and create this item. 

![](/img/03/28.png)

So as we can see, this has worked. So even though we have the same user ID, because the post timestamp was different, we were able to enter this data into my table. So the uniqueness has to be on the user ID and post timestamp as a combination, and so that means that the data is partitioned by user ID, so this is why john is clickable, because we can query and search for john123 as my user ID, and then we can sort the data by post timestamp. Okay, so it's called a sort key.

![](/img/03/29.png)

So this is very, very, very handy, and as we can see, if we wanted to create one last item, and this was for Alice456, again, you will need to enter a post timestamp and some content, and you'll be good to go, and so this is why it's super important to choose a really good partition key in these kind of moments, because if you choose a partition key that keeps on coming back, so if john123 is the only user that posts and you have 10,000 posts, then the data is going to be heavily skewed toward john123, so it's just something to be aware of, okay?

### DynamoDB in Big Data

Now, how does DynamoDB relate to the big data world? Well, there's some very common use cases that includes mobile ads, gaming, digital arts ad serving, live voting, audience interaction for live events, sensor network, log ingestion, access control for web-based content, metadata storage for Amazon S3 objects, e-commerce shopping cart, web session management. So, the use cases are quite diverse, but basically that means that any time you have some data that needs to be very hot, that needs to be ingested at scale within a database, DynamoDB is going to be great for that. In terms of the inside pattern, what don't you do with DynamoDB? Well, for example, an application that uses RDS, like a traditional database, like a RDBMS, then you may be able to use RDS instead because you don't want to rewrite it entirely. Or, if you need to have joins or complex transactions, then DynamoDB may not be the best friend for this. Maybe, again, an RDS database may be better for you. If you need to store binary large objects or pull-up data, so big data, you know, maybe it's better to store it in S3 and store the metadata in DynamoDB. This is a very common pattern. And, in general, if you have large data that has low IO writes, so very little writes or very little reads, S3 is going to be a much better storage option for you. So, think about it. DynamoDB is going to be more for when your data is hot and smaller, while S3 is going to be more when your data is a bit colder but much bigger.

**Common use cases include:**
* Mobile apps
* Gaming
* Digital ad serving
* Live voting
* Audience interaction for live events
* Sensor networks
* Log ingestion
* Access control for web-based content
* Metadata storage for Amazon S3 objects
* E-commerce shopping carts
* Web session management

**Anti Pattern**
* Prewritten application tied to a traditional relational database: use RDS instead
* Joins or complex transactions
* Binary Large Object (BLOB) data: store data in S3 & metadata in DynamoDB
* Large data with low I/O rate: use S3 instead


#### DynamoDB – Read/Write Capacity modes

Okay, so now let's talk about read and write capacity modes for DynamoDB. So this is how you control your table's capacity, so you have to specify an event, so read and write throughput, so you have two modes actually. The first one is called provision mode, which is the default mode, in which we specify the read and write per second, so also called read capacity units and write capacity units. You need to plan your capacity beforehand, you're going to pay for whatever is going to be provisioned, so if you say I want 10 read capacity units instead of 5 write capacity units, you're going to pay for that every hour, and you have the option to do autoscaling, as we'll see very soon. There's a second mode called the on-demand mode, and this one will automatically have the reads and the writes scaled up and down based on your workloads, and there's no capacity planning needed, so you don't need to provision capacity units, they will just be there, and you're going to pay exactly for what you use, but it's going to be a lot more expensive than provision mode. So the idea is that you have different use cases, and we'll see them in this lecture in details. So you can switch between the two modes, provision and on-demand, once every 24 hours. So we're going to do a deep dive on both of them, so do not worry right now, it's just an introduction

* Control how you manage your table’s capacity (read/write throughput)

* **Provisioned Mode (default)**
  * You specify the number of reads/writes per second
  * You need to plan capacity beforehand
  * Pay for provisioned read & write capacity units

* **On-Demand Mode**
  * Read/writes automatically scale up/down with your workloads
  * No capacity planning needed
  * Pay for what you use, more expensive ($$$)

* You can switch between different modes once every 24 hours


#### R/W Capacity Modes – Provisioned

But now let's do a long deep dive on provision read capacity mode. So you must provision read and write capacity units, also called RCU, which is the throughput for reads, and WCU, which is the throughput for writes. Now you have the option to set up autoscaling of throughput to meet demand, so don't you think too hard about your RCU and your WCU value, you just say what's your target capacity usage, and DynamoDB will scale those for you. Now in case you go over the RCU and over the WCU because you are consuming more or writing more than what you provisioned, it's completely fine because you can tap into, temporarily, what's called a burst capacity. But if you exhaust the burst capacity because it's been fully consumed, then you're going to get an exception called the provision throughput exceeded exception, which is very obvious about what it is. Then, in case you get these things, you need to obviously retry, and the strategy to retry your writes or read is called the exponential back-off retry strategy. 

* Table must have provisioned read and write capacity units
* **Read Capacity Units (RCU)** – throughput for reads
* **Write Capacity Units (WCU)** – throughput for writes
* Option to setup auto-scaling of throughput to meet demand
* Throughput can be exceeded temporarily using **“Burst Capacity”**
* If Burst Capacity has been consumed, you’ll get a **“ProvisionedThroughputExceededException”**
* It’s then advised to do an **exponential backoff** retry

#### DynamoDB – Write Capacity Units (WCU)

Now let's look at WCU in detail. So the example will ask you to perform some computations, and so therefore you need to understand the formulas to compute WCU and RCU. So one write capacity unit, WCU, will represent one write per second for an item of up to one kilobyte in size. And if your item is obviously larger than one kilobyte, then you're going to consume more WCUs. So let's go through examples to understand the computation, and you can pause this video if you want to do the computation on your own. So if you write 10 items per second, and the item size is, on average, two kilobytes, how many WCUs do you need? Well, we do the math. We need 10 items per second, so 10 times, and then the item size is two kilobytes, which we divide by one kilobyte, which is the number WCU you need for one kilobyte, and you get a result of 20 WCUs. So this was a very easy example. Now, in example two, we write six items per second, and this time the item size is 4.5 kilobytes. So the trick here on this one, so we need 6 times 5 divided by 1, which is 30 RCUs. Why? Well, because the 4.5 kilobytes is always going to be rounded to the upper kilobytes, but then we need to get an idea of how many WCUs you've consumed. So remember, you need to round to the upper kilobytes for WCUs. Now, if you write 120 items per minute, and the item size is two kilobytes, the trick here is that we have items per minute, we need to do a small computation, which is 120 divided by 60 to get items per second, and then this gives us four WCUs. Okay, so write-by-passing units are very basic and honestly, very easy to understand. The more complicated ones are going to be around reads.

![](/img/03/30.png)

#### Strongly Consistent Read vs. Eventually Consistent Read

So first, we need to define two kinds of read modes for DynamoDB, which is a strongly consistent read and the eventually consistent read. So if you consider DynamoDB, it's a serverless database, of course, but behind the scenes, there are servers. You just don't see them or manage them. So we have servers, and let's just consider three servers right now to make it very, very simple, but it's obviously a lot more, and your data is going to be distributed and replicated across all these servers. Now, if you consider your application, your application is going to do writes to one of these servers, and internally, DynamoDB is going to replicate these writes across different servers, such as server two and server three. Now, when your application reads from DynamoDB, there is a chance that you're going to read, not from server one, but from server two. Now, two things can happen, right? If we are in the eventually consistent read, which is a default mode, then if we do a read just after a write, it's possible that we'll get stale data because the replication has not happened yet. If we're very, very quick, but after, say, 100 milliseconds, you're good to go, obviously. But if you do a strongly consistent read, you're saying, hey, I want to read the data just after the write, and you're going to get, for sure, the correct data that was just written. For this, we need to set a parameter called consistent read in our API to true, and it would be applied to getItem, bash.item, care, query, and scan. And why wouldn't we do this all the time? Why wouldn't we want to have strongly consistent reads all the time? Well, it's going to consume twice the RCU, it's going to be a more expensive query, and also may have a slightly higher latency. So you need to ask yourself, do I need eventually consistent reads, or do I need strongly consistent reads?

![](/img/03/31.png)

#### DynamoDB – Read Capacity Units (RCU)

If I need eventually consistent reads or if I need strongly consistent reads. Now, let's talk about RCU regarding these two things. One read capacity unit, RCU, represents one strongly consistent read per second, or two eventually consistent reads per second, for an item of up to 4 kilobytes in size, which makes the computation a little bit trickier. And if your items are larger than 4 kilobytes, more RCUs are going to be consumed, and again, it's going to be rounded up to the nearest upper 4 kilobytes. So let's go through examples again, and feel free to pause the video. So if you have 10 strongly consistent reads per second, and the item size is 4 kilobytes, how many RCUs do you need? Only 10 times 4 kilobytes divided by 4, which is equal to 10 RCUs. Now, if you have 16 eventually consistent reads per second, and the item size is 12 kilobytes, we're going to get 16 divided by 2 times 12 divided by 4, which is 24 RCUs. So in this example, obviously, we need to divide 16 divided by 2, because we have two eventually consistent reads per second in one RCU, and then we divide 12 by 4, which gives us 24 RCUs. And last example, which is 10 strongly consistent reads per second with an item size of 6 kilobytes. So this one's a little bit trickier. So what is it? Well, it's 10 times 8 divided by 4, and why 8? Well, we round up 6 kilobytes to the nearest 4 kilobytes, so it's going to be 8 kilobytes, and you have to always go up. And in this case, we get 10 times 8 divided by 4, which is 2, so 20 RCUs. So now that we know about WQEs and RCUs, let's talk about how DynamoDB works in the backend with partitions. 

![](/img/03/32.png)

#### DynamoDB – Partitions Internal

So DynamoDB is made of tables, and each table has partitions. And partitions are just copies of data that live on specific servers. Now, when your application does write into DynamoDB, what's going to happen is that your application will send a partition key, a sort key maybe, and some attributes, and all this data is going to go through a hashing algorithm. So only the partition key, actually, is going to go through a hashing algorithm to understand which partition to go to. So if you take the partition key of ID 13, it's going to go through this internal hash function of DynamoDB, and it's going to say, hey, any time I see ID 13, this is going to go into partition 1. And if you have ID 45 in the second row, then ID 45 is going to go through the hash function, and the hash function will say, hey, this ID 45 should go to partition 2, and this is how the data gets distributed. So obviously, if you have what's called a hot key, the data is always going to be into the same partition. So to compute the number of partitions, there are some intense formulas that you do not need to know for the exam, so let's go over them very quickly. You compute the number of partitions by dividing the RCUs by 3,000 and WCUs by 1,000, and adding those up. You can also have a look at how much data you have, so total size of your dataset divided by 10 gigabytes, and then the number of partitions is going to be the max of these two things. Now, you don't need to know these formulas, so do not worry about it at all. The way to understand, though, is that if you have 10 partitions, and you provision 10 WCUs and 10 RCUs, then they're going to be spread evenly across partitions. That means that each partition is going to get one WCU and one RCU, and this is the point I want you to remember. WCUs and RCUs are going to be divided and spread evenly across partitions, which brings us to FrontLink.

![](/img/03/33.png)

#### DynamoDB – Throttling

So in case you exceed the RCUs or WCUs, and this is at the partition level, then you're going to get a provisioned throughput exceed exception. So maybe because you have a hotkey, so one partition key is being read too many times from a specific partition, which is because maybe you have a popular item, or hot partitions, or very large items, because obviously when WCU and RCUs are computed, it depends on the item size, and so if you read or write a very large item, you're going to consume a lot of RCUs or WCUs. Now, solutions for attacking this provisioned throughput exceed exception is, number one, to do exponential backup when the exception is encountered, which is, if you're using SDK, something that's already included, you have to distribute the partition keys as much as possible, which was the exercise we did in the first lecture to understand how we could choose a really good partition key. And if this is an RCU issue because you're reading one data point on one partition very, very heavily, then we'll have a look at the feature called DynamoDB Accelerator, or DAX.

* If we exceed provisioned RCUs or WCUs, we get “`ProvisionedThroughputExceededException`”
* Reasons:
  * Hot Keys – one partition key is being read too many times (e.g., popular item)
  * Hot Partitions
  * Very large items, remember RCU and WCU depends on size of items
* Solutions:
  * Exponential backoff when exception is encountered (already in SDK)
  * Distribute partition keys as much as possible
  * If RCU issue, we can use DynamoDB Accelerator (DAX)


#### R/W Capacity Modes – On-Demand

Now, the last mode we need to understand, and this is a much easier mode to understand, is the on-demand read-write capacity mode, which is going to automatically accept any reads and any writes, and it's going to scale up and down based on workload. So there's no capacity planning needed. You do not specify the RCU or the WCU. It's unlimited. There's no throttling, but obviously this is more expensive. And you're going to charge for the actual reads and writes. So it's called read-request-use, or RRUs, and write-request-use, which is WRUs. The computations are the same, but the idea is that because we have every request that succeeds, it's not a capacity we talk about, it's just on the request. Now, to give you some overview, on-demand is about 2.5x more expensive than provisioned capacity, so make sure that you have it only for specific kind of use cases, for example, unknown workloads or when it's unpredictable application traffic. So that's it for capacity modes in DynamoDB. I hope you liked it, and I will see you in the next lecture.

* Read/writes automatically scale up/down with your workloads
* No capacity planning needed (WCU / RCU)
* Unlimited WCU & RCU, no throttle, more expensive
* You’re charged for reads/writes that you use in terms of **RRU** and **WRU**
* **Read Request Units (RRU)** – throughput for reads (same as RCU)
* **Write Request Units (WRU)** – throughput for writes (same as WCU)
* 2.5x more expensive than provisioned capacity (use with care)
* Use cases: unknown workloads, unpredictable application traffic, …