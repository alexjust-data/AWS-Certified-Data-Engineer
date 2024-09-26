**Database**

- [Amazon DynamoDB](#amazon-dynamodb)
  - [Traditional Architecture](#traditional-architecture)
  - [NoSQL databases](#nosql-databases)
  - [Amazon DynamoDB](#amazon-dynamodb-1)
  - [DynamoDB - Basics](#dynamodb---basics)
  - [DynamoDB – Primary Keys](#dynamodb--primary-keys)
- [DynamoDB - Hands On](#dynamodb---hands-on)
- [DynamoDB in Big Data](#dynamodb-in-big-data)
- [DynamoDB - Throughput (RCU \& WCU)](#dynamodb---throughput-rcu--wcu)
  - [DynamoDB – Read/Write Capacity modes](#dynamodb--readwrite-capacity-modes)
  - [R/W Capacity Modes – Provisioned](#rw-capacity-modes--provisioned)
  - [DynamoDB – Write Capacity Units (WCU)](#dynamodb--write-capacity-units-wcu)
  - [Strongly Consistent Read vs. Eventually Consistent Read](#strongly-consistent-read-vs-eventually-consistent-read)
  - [DynamoDB – Read Capacity Units (RCU)](#dynamodb--read-capacity-units-rcu)
  - [DynamoDB – Partitions Internal](#dynamodb--partitions-internal)
  - [DynamoDB – Throttling](#dynamodb--throttling)
  - [R/W Capacity Modes – On-Demand](#rw-capacity-modes--on-demand)
- [DynamoDB - Throughput (RCU \& WCU) - Hands on](#dynamodb---throughput-rcu--wcu---hands-on)
- [DynamoDB - Basic APIs](#dynamodb---basic-apis)
  - [Writing Data](#writing-data)
  - [Reading Data](#reading-data)
  - [Reading Data (Query)](#reading-data-query)
  - [Reading Data (Scan)](#reading-data-scan)
  - [Deleting Data](#deleting-data)
  - [Batch Operations](#batch-operations)
  - [PartiQL](#partiql)
- [DynamoDB - Basic APIs - Hands On](#dynamodb---basic-apis---hands-on)
- [DynamoDB - Indexes (LSI \& GSI)](#dynamodb---indexes-lsi--gsi)
  - [Local Secondary Index LSI](#local-secondary-index-lsi)
  - [Global Secondary Index (GSI)](#global-secondary-index-gsi)
  - [Indexes and Throttling](#indexes-and-throttling)
- [DynamoDB - Indexes (LSI \& GSI) - Hands On](#dynamodb---indexes-lsi--gsi---hands-on)
  - [DynamoDB - PartiQL](#dynamodb---partiql)
  - [DynamoDB Accelerator (DAX)](#dynamodb-accelerator-dax)
  - [DynamoDB Accelerator (DAX) vs.ElastiCache](#dynamodb-accelerator-dax-vselasticache)
  - [DynamoDB Accelerator (DAX) Hand On](#dynamodb-accelerator-dax-hand-on)
  - [DynamoDB - Streams](#dynamodb---streams)
  - [DynamoDB Streams \& AWS Lambda](#dynamodb-streams--aws-lambda)
  - [DynamoDB - Streams - Hand On](#dynamodb---streams---hand-on)

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


### DynamoDB - Throughput (RCU & WCU)


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


### DynamoDB - Throughput (RCU & WCU) - Hands on

Let's have a look at how we can define the RCU and WCU of our tables. So if we take our users table, for example, and you go on the right-hand side and you do **`additional settings`**, 

![](/img/03/34.png)

you have the read-write capacity, and you can edit it. 

![](/img/03/35.png)

So this is what we have defined at table creation time, but the cool thing about NMDB is that we can switch between the capacity modes if we need to, and then we can also change them over time. So let's take the most simple capacity mode right now, which is on-demand. And this is simplified billing for paying for the actual read and write of your application. But this is twice or three times as expensive as provisioned. And so obviously this is when you have a very unusual, unpredictable type of workload, or maybe this is in your development environment, and let's consider that you're not using the table at all for 24 hours a day, but maybe one day for one hour you'll be using the table a lot. And so therefore, this is a great capacity mode because it actually bills you for what you've been using, which is quite amazing. Now there's provisioned capacity mode, and this is the one we spend most of the time on to understand and to compute the reads and writes. And so let's go into the capacity calculator. So as you can see here, you can specify the average item size. For example, six kilobytes, how many reads per second you want, so maybe three reads per second and two writes per second. The type of reads you want to have, so eventually consistent and strongly consistent. There's transactional, but I will define this a bit later on. I don't want to overwhelm you right now, so eventually or strongly consistent. And then for the write consistency, same standard or transactional, but again, transactional we'll see a little bit later. And as you can see, based on what I choose, it gives me the RCU and the WCU, as well as the estimated costs I will have for my table. So this is quite a handy calculator, and I would suggest that you practice a little bit with these settings and you try to guess correctly what's the RCU and WCU for this table because this is something that the exam will ask you.

![](/img/03/36.png)


Now let's take a look at the table capacity. So we can obviously have off autoscaling and off autoscaling for reads and writes, and so therefore each provisioned capacity unit, and they won't change over time unless you manually change them, but you can also set up autoscaling. And autoscaling is really cool because we're just saying, what's my min and my max I'm willing to consider, and what's my target utilization as a percentage? And then we will try, for example, if we have maximum capacity of 100, it will set to 100, the WCU, if you're actually consuming on average 70 WCUs and so on. But if you're consuming, say, 7 WCUs, then automatically the autoscaling will kick in and the desired capacity units will be 10 for that table. So this is going to be nice because you don't have to think too hard about setting the WCU and RCU. You just need to think about what's my min, what's my max, and what's my target utilization, and your application and DynamoDB will do the rest, which is nice to have a better pricing and a better scaling. So this is applying to both autoscaling for reads and for writes, so this is good. And then you would get the estimated cost and so on. So if I set it to autoscaling max 3 and min 1, and then again max 3 and min 1, and click on Save Changes,


![](/img/03/37.png)


now what I'm going to do is just wait a little bit of time for autoscaling to kick in and to see the autoscaling activities. And as you can see, the provision was 2 and 2 for WCUs, but we had 1 to 3 in terms of range for autoscaling. And I refreshed autoscaling activities, and as you can see for this table, the setting read capacity was set to 1 and write capacity was set to 1 because I'm not using the table, and so autoscaling kicked in and it worked. Therefore, if I refresh this page and remove this here, as you can see now, the RCU and WCUs are 1 and 1. So autoscaling is quite cool, it works just like EC2, but for DynamoDB.

![](/img/03/38.png)


### DynamoDB - Basic APIs


#### Writing Data

In the exam, you will see the API calls of the MODB referred by their name. So it's good for us to see them once. So if you want to write data, you have a few options. You have PutItem, and when you do a PutItem, it creates or fully replaces a new item that has the same primary key. It will consume write capacity units, and so the idea is that you want to do a full replace or writing a new item. The second one is UpdateItem, which is a bit different than PutItem. This one will edit the existing item's attributes, or will add a new item if it does not exist. But the idea is that with UpdateItem, we only edit a few attributes and not every other attribute, so this is the difference between PutItem and UpdateItem. And we can use it with atomic counters that we'll see in this section as well. And then you have ConditionalWrite, which is to accept a write update delete only if a condition is met, and this is helping with concurrent access to items, and we'll see this as well in this section.

* **PutItem**
  * Creates a new item or fully replace an old item (same Primary Key)
  * Consumes WCUs
* **UpdateItem**
  * Edits an existing item’s attributes or adds a new item if it doesn’t exist
  * Can be used to implement **Atomic Counters** – a numeric attribute that’s unconditionally incremented
* **Conditional Writes**
  * Accept a write/update/delete only if conditions are met, otherwise returns an error
  * Helps with concurrent access to items
  * No performance impact


#### Reading Data

To read data, we have GetItem, and GetItem is very simple and easy to understand. You read based on the primary key, and the primary key, again, can be a hash or a hash plus run, so you have the two options. And you get two modes to read from, so you have the EventuallyConsistentReadMode or to have StronglyConsistentReadMode, so you need to specify it. It will take more RCU and then maybe a little bit more latency. You can also specify a projected expression in your API, and this projection expression is going to help you receive only a few attributes out of the DynamoDB.

* **GetItem**
  * Read based on Primary key
  * Primary Key can be **HASH** or **HASH+RANGE**
  * Eventually Consistent Read (default)
  * Option to use Strongly Consistent Reads (more RCU - might take longer)
  * **ProjectionExpression** can be specified to retrieve only certain attributes

#### Reading Data (Query)

Next, we have the query, and the query is to return items based on a key condition expression, which is a partition key, so it must be the equal operator, so you're saying, hey, I want to query for job123, and also optionally a sort key, and because you can sort, you can have equal, less than, over than, begins, between, and so on. Then you can specify a filter expression, and this is to add additional filtering after the query operation has been done, but before the data is returned to you, and this is to use with non-key attributes, so you cannot use a filter expression with hash or range attributes. And when the query returns, it's going to be a list of items, obviously, and you have a limit of how many items you retrieve based on the limit query parameter, and either you're going to reach that limit of number of items or you're going to get up to one megabyte of data. But if you want to get, obviously, more data over time, you can do pagination on the results and ask for more and more and more. Now you can query a table or a local secondary index or global secondary indexes, and we'll see those in the next lectures as well. 


* Query returns items based on:
  * KeyConditionExpression
    * Partition Key value (must be = operator) – required
    * Sort Key value (=, <, <=, >, >=, Between, Begins with) – optional
  * FilterExpression
    * Additional filtering after the Query operation (before data returned to you)
    * Use only with non-key attributes (does not allow HASH or RANGE attributes)
* Returns:
  * The number of items specified in Limit
  * Or up to 1 MB of data
* Ability to do pagination on the results
* Can query table, a Local Secondary Index, or a Global Secondary Index


#### Reading Data (Scan)

And finally, you have a scan, so getItem was for one item, then the query was for a specific partition key and a sort key, and scanItem is to read an entire table, and then if you wanted to, you could filter the data, but this is only done on the client side, so this is very inefficient. So scan is to really export the entire table, and each scan will return up to one megabyte of data, and if you want to keep on reading, then you use pagination techniques, so that means page one, page two, page three, and so on. It will consume a lot of RCU because you are reading your entire table, and so therefore, if you want to not impact your normal operations, you need to impact the scan using a limit statement or to reduce the size of the result and then pause a little bit. And if you wanted to instead consume a lot of RCUs and do a scan as fast as possible, then for faster performance, you would use a parallel scan. In this case, multiple workers that you define will scan multiple data segments at the same time, which will increase the throughput and RCU consumed, and if you wanted to start a parallel scan and limit its impact, you could use limit conditions and so on. And then scans can be used with projection expression and filter expression, so projection expression to only retrieve certain attributes and filter expression to send stuff server-side


* Scan the entire table and then filter out data (inefficient)
* Returns up to 1 MB of data – use pagination to keep on reading
* Consumes a lot of RCU
* Limit impact using **Limit** or reduce the size of the result and pause
* For faster performance, use **Parallel Scan**
  * Multiple workers scan multiple data segments at the same time
  * Increases the throughput and RCU consumed
  * Limit the impact of parallel scans just like you would for Scans
* Can use **ProjectionExpression & FilterExpression** (no changes to RCU)


#### Deleting Data

Now, if you wanted to delete data out of DynamoDB, you have the delete item, which is used to delete an individual item, and then you can also do a conditional delete, so delete this item only if money equals zero. And if you wanted to delete everything in your table, you have delete table, so this is to delete the whole table and its item, and it's much quicker than doing a scan and then deleting each and every single item on the table. Delete table will just drop everything, and this is something that can come up in the exam. If you wanted to just delete everything, do not do a scan, just use the delete table API. 


* **DeleteItem**
  * Delete an individual item
  * Ability to perform a conditional delete
* **DeleteTable**
  * Delete a whole table and all its items
  * Much quicker deletion than calling **DeleteItem** on all items


#### Batch Operations

Now, for efficiency purposes, you can actually batch operations in DynamoDB. So you save in latency and you gain in efficiency by reducing the number of API calls you do to the database, and all the operations as part of a batch are going to be applied in parallel by DynamoDB for better efficiency. So because you have a batch of operations, though, part of the batch can fail. In that case, you will receive the failed items back and you can retry only these failed items. So in the write mechanism, you have batch write item, and this allows you to perform up to 25 put item and or delete item in one call. You have up to 16 megabytes of data written and still have the same limit of 400 kilobytes of data per item, and you cannot update item. You can only do put item or delete item. Now, if you have items that were not able to be written for whatever reason, usually because of a lack of write capacity, then you will receive back something called unprocessed items, and then you can retry the items within the unprocessed items. So two options to process them correctly. Either you use an exponential backup strategy to keep on retrying with longer and longer time until it succeeds, or if you consistently get these unprocessed items and scaling issues, then, of course, you need to add write capacity units to allow your batch operations to complete efficiently. For batch get item, you will return items from one or more tables, and you can receive up to 100 items and up to 16 megabytes of data, and all these items are going to be retrieved in parallel to minimize latency. Again, if you are missing some items, it's because you may have some unprocessed keys because you have failed read operations because you don't have enough capacity, in which case, same idea. You use exponential backup to retry or you add read capacity units to increase your read capacity.


* Allows you to save in latency by reducing the number of API calls
* Operations are done in parallel for better efficiency
* Part of a batch can fail; in which case we need to try again for the failed items

* **BatchWriteItem**
  * Up to 25 **PutItem** and/or **DeleteItem** in one call
  * Up to 16 MB of data written, up to 400 KB of data per item
  * Can’t update items (use **UpdateItem**)
  * **UnprocessedItems** for failed write operations (exponential backoff or add WCU)

* **BatchGetItem**
  * Return items from one or more tables
  * Up to 100 items, up to 16 MB of data
  * Items are retrieved in parallel to minimize latency
  * **UnprocessedKeys** for failed read operations (exponential backoff or add RCU)


#### PartiQL

Also, we have PartyQL. So we've seen that in DynamoDB, we have specific API calls to do specific things, but sometimes all that you know as a data engineer or whatever, as a developer, may be SQL. And so you can use SQL on DynamoDB by using PartyQL. So here you have a standard SQL query where you find the order ID and the total from your orders table, and you have a filtering condition and an ordering condition. And so using PartyQL, you can do the exact same operations we saw before, which is to select, insert, update, and delete data in DynamoDB. But this time, instead of doing the DynamoDB-specific APIs, you can just use SQL, and you can run your queries across multiple DynamoDB tables, but you cannot do joins, okay? You can just do the select, insert, update, and delete. Everything you can do with an API, but then you use SQL for writing these calls. So you run PartyQL queries from the Management Console or from the NoSQL Workbench for DynamoDB or from DynamoDB APIs or from the CLI or from the SDK. And the goal of it is really not to add new capabilities to DynamoDB because you have the same capabilities, but it's just to use SQL to write these API calls against DynamoDB. So hopefully that makes sense. I hope you liked it, and I will see you in the next lecture.


* SQL-compatible query language for DynamoDB
* Allows you to select, insert, update, and delete data in DynamoDB using SQL
* Run queries across multiple DynamoDB tables
* Run PartiQL queries from:
  * AWS Management Console
  * NoSQL Workbench for DynamoDB
  * DynamoDB APIs
  * AWS CLI
  * AWS SDK

```sql
SELECT OrderID, Total
FROM Orders
WHERE OrderID IN [1,2,3]
ORDER BY OrderId DESC
```


### DynamoDB - Basic APIs - Hands On

![](/img/03/41.png)

So let's have a look at these data API calls we can do. So if I go on my table, as we can see right now, we are in the scan, and we can select a table, and then we click on run. So scan is going to scan the entire table, and obviously going to return many items to us. If we wanted to create an item, 

![](/img/03/43.png)


what we could do is specify a user ID, so alice456, then we specify a timestamp, so let me just do this one very quickly, and then t0506 and 00, here we go. Okay, and then some content for it, alice blog. Okay, now let's create this item.

![](/img/03/44.png)

Okay, now let's create this item. So this was a put item. Why? Because we have sent a new item into DynamoDB, and we specified user ID and post timestamp, and this was new, so here we go, created. If you want to look at update item, what I can do is action, and then edit, and then I will just edit one specific attribute, so alice blog edited, and click on save change,

![](/img/03/45.png)


![](/img/03/46.png)


 and behind the scenes, this is going to do a update item API call. 
 
![](/img/03/47.png)
 
So we have seen update, now let's look at get


So if I click on this one, for example `John123`, and I go into the item editor, well, behind the scenes, by clicking on this row, on this item, I was able to retrieve the content of this item, and so this was a get item that was done, so this is good.  

Now we can do, obviously, batch actions, so we can do action and then delete items through a batch delete item, okay, so batch write and a batch delete. But if you want to delete everything in the table, you can do a scan and a batch delete, but this would not be very efficient.

![](/img/03/48.png)


But this would not be very efficient. The other way to do it would be to simply drop the table and be done. 


Now finally, let's have a look at scan and query. So scan really gives you all the tables you have, and then you can apply a filter if you wanted to, but this is done client-side for this filter, so this will be filtered in your web browser, not in DynamoDB directly. Or you can do a query, and a query is very helpful because we can specify a specific user ID, for example, John123, and then click on run, and this gives us all the items of John123, but also we could have a query and specify a condition on the post timestamp, which is a sort key.

![](/img/03/49.png)

So we can have equal to, less than equal to, greater than, between, and begins with. So if you wanted to know all the posts after, say, 2021-11, and click on run, we will only get one return item, but then if we do 2021-09, we get two items back. So you can see the queries here is only done on user ID and post timestamp. We cannot query on the content or search for the content. We could filter on the content if we wanted to afterwards, but this would be done client-side. So this really shows you the power of using partition keys, such as a hash key and a sort key. So that's it. We've seen all the APIs of DynamoDB.

![](/img/03/50.png)


### DynamoDB - Indexes (LSI & GSI)

#### Local Secondary Index LSI

So an LSI is going to give you an alternative sort key for your table. So you have the same partition key from your base table, but you're going to get an additional sort key. And this sort key consists of one scalar attribute, so it could be a string, a number, or a binary. And you can get up to five LSIs per table. Now, LSIs must be defined at table creation time, so you cannot create them after your table has been created. So you need to have some careful thinking into how you want your table to be designed. Next, on your LSI, you can have some or all attributes from your main table. So it's up to you to choose in your LSI if you want to only have maybe one specific attribute because this is what you're trying to query for. So if we take an example of here, this is our table, which has user ID, game ID, game timestamp, score, and results. And right now we can do queries on user ID and game ID very simply, but we cannot do a query on user ID and game timestamp. For this, we need to do a scan and then do some client-side filtering. So to do so, if you wanted to do a query based on user ID and game timestamp, we need to create an LSI and define the LSI on the attribute game timestamp. And if we do so, then we can do a query on, hey, give me all the games that have been done by this user between 2021 and 2020, and so on. So this is super important for you to understand. This is the same partition key as before, but we have a different sort key, thanks to an LSI.

![](/img/03/51.png)

* **Alternative Sort Key** for your table (same **Partition Key** as that of base table)
* The Sort Key consists of one scalar attribute (String, Number, or Binary)
* Up to 5 Local Secondary Indexes per table
* **Must be defined at table creation time**
* **Attribute Projections** – can contain some or all the attributes of the base table (**KEYS_ONLY, INCLUDE, ALL**)

#### Global Secondary Index (GSI)

Next, we have GSI, or Global Secondary Indexes. So this is going to give you an alternative primary key. So you can have a different hash key, a different partition key, or you can have a different hash key and sort key as well from the base table. So this is helpful if you wanted to speed up queries on things that are non-key attributes within your table. So the index can consist of scalar attributes, so string, number, and binary. And again, you can specify which attributes you want to project on that index. And for that index, though, it's very special because it's kind of like a different new table. And therefore, for this index, you must provision the RCU and WCUs. Now, the GSIs are powerful because they can be added or modified after the table has been created. So let's take a very simple table in which we have user ID, game ID, and game timestamp. With this table, we're able to query based on user ID. So give me all the games of this user. But we cannot query on game ID. As we can see right now, if you want to query on game ID, it would be extremely difficult to do a scan and then filter client-side. So instead, we're going to create a GSI, a global secondary index, which is going to give us the ability to query by game ID. So now the partition key for that GSI becomes the game ID. The sort key may be, for example, game timestamp, and this is what you want to query on. And the attributes become user ID because we've done a projection of the user ID attributes. So you can see in this one, we're creating some fully different new queries by defining a new partition key and a sort key. And this is why it's very important in DynamoDB to understand how you're going to query data, to understand how you're going to make your local secondary indexes and your global secondary indexes. So within LSI, within GSI, very, very different purposes. But let's talk about these indexes and throttling.

![](/img/03/52.png)

* **Alternative Primary Key (HASH or HASH+RANGE)** from the base table
* Speed up queries on non-key attributes
* The Index Key consists of scalar attributes (String, Number, or Binary)
* **Attribute Projections** – some or all the attributes of the base table (**KEYS_ONLY, INCLUDE, ALL**)
* Must provision RCUs & WCUs for the index
* **Can be added/modified after table creation**


#### Indexes and Throttling

But let's talk about these indexes and throttling. So when you have a GSI, if the rights are throttled on the GSI, then the main table will be throttled as well. So this is a very, very important caveat that comes up in the exam. So even if the WCUs are fine on the main table, if they are throttling on the GSI, then the main table, no matter what, will be throttling. And therefore, choose your GSI partition key carefully and assign your WCU capacity very, very carefully. Whereas for LSI, so local secondary indexes, they will use the WCU and the RCU of the main table, and there's no special throttling considerations to be had. OK, so that's it for this lecture. I hope you like it.

* Global Secondary Index (GSI):
  * If the writes are throttled on the GSI, then the main table will be throttled!
  * Even if the WCU on the main tables are fine
  * Choose your GSI partition key carefully!
  * Assign your WCU capacity carefully!

* Local Secondary Index (LSI):
  * Uses the WCUs and RCUs of the main table
  * No special throttling considerations

### DynamoDB - Indexes (LSI & GSI) - Hands On

So let's have a look at how we can create indexes. So back into my table, I'm going to create a new table, and I'll call this one Demo Indexes. And we need to choose a partition key, so let's choose UserID and sort key GameTimeStep. So this is allowing us to query on UserID and GameTimeStep. And for this, I'm going to customize settings,

![](/img/03/53.png)

I'm going to say I want provision capacity of 1 RCU and 1 WCU, and here we can define our secondary indexes. So you can see we can create either a local index or a global index. Now local indexes can only be created at table creation time, whereas global indexes can be created afterwards. So we can create a local index, 

![](/img/03/54.png)

So we can create a local index, and here we need to specify a different sort key. So as you can see, we do not have the option to specify a different partition key, so UserID will remain the `partition key` for this index, but we can specify a different sort key. So for example, let's say I want to create a local index, and the sort key is going to be called GameID. So now the index name is GameID underscore index, and then what attributes do we want to project onto this index? We want all, only the keys, or only include specific attribute names that we specify. So we'll keep it simple and have it as all. 

![](/img/03/55.png)

 So we just created this index, and we could go ahead and create a global secondary index in which we have the option to specify a different partition key, and optionally a different sort key, as well as project some attributes. So let's just for now not create this global secondary index, we're just going to create a local index, okay? And then click on create table. 

![](/img/03/56.png)


**`Create table`**

So we just created this index, and we could go ahead and create a global secondary index in which we have the option to specify a different partition key, and optionally a different sort key, as well as project some attributes. So let's just for now not create this global secondary index, we're just going to create a local index, okay? And then click on create table. Okay, so my table is now created,

![](/img/03/57.png)

and let's have a look at how we can query it right now.

![](/img/03/58.png)

I go into query, as you can see, I can query either a table or an index, and I have two options. I can query my demo indexes, which is my table, so I can specify a user ID and a game timestamp, or I could query my index and specify a user ID and a GameID. So as you can see, this local index allows me to query by a different partition key, a different sort key, excuse me, with the same partition key. 

![](/img/03/59.png)

 So let's go back into the table details, and let's go into the indexes tab. So as you can see, there is one local secondary index that has been defined, so this one, and we cannot create another one. So LSIs have to be defined at table creation time, and not afterwards, whereas GSIs, so global secondary indexes, can be created afterwards.

![](/img/03/60.png)


**`Create index`**

So we could create an index, and we could enter a completely different partition key, for example, GameID, and a sort key, for example, GameTimestamp, okay? And this gives me a GSI, and now we need to also create some capacity for my GSI, so the local secondary indexes type in the RCU and WCU of the main table, whereas for a global secondary index, you need to create your own read and write capacity. So either we copy from base table, or we customize settings, and we can, again, specify autoscaling on and off. 

![](/img/03/61.png)

So we'll just `copy from base table`, and have one and one for RCU and WCUs, and then what attributes we want to project

![](/img/03/62.png)

and then this global secondary index is going to be created. Now remember, if you query a lot, that GSI and writes get throttled, then the writes will be throttled as well on the main table, whereas the LSI just types into the RCU and WCU of the main table. Okay, so my global secondary index is now created,

![](/img/03/63.png)

and what I can do now is that if I go back into view items, I can look at my table, and now I can query by game ID, `game timestamp inde`x, and so as you can see now, the partition key I can query by is my game ID, and my sort key is my game timestamp. So this really shows you all the powers of indexes. 

![](/img/03/64.png)


#### DynamoDB - PartiQL

Let me talk about PartyQL for DynamoDB, which allows you to use a SQL-like syntax to manipulate DynamoDB tables. So this is what the statements would look like, and then from this you can insert, for example, or update, or select, or delete items from a DynamoDB table. So it's to allow people who are more confident with SQL to still be able to interact with DynamoDB. And it also supports batch operations if you need to. So let me show you how PartyQL works in the console. So I am in my tables, and on the left-hand side is the PartyQL editor.

![](/img/03/65.png)

So let's open some of these tables. For example, let's open the Users table.

![](/img/03/66.png)

And I emptied it, but I can, for example, add an item real quick. 

![](/img/03/67.png)

So I can add the user ID, 123, as well as a new attribute named Stéphane. And this is good.

![](/img/03/68.png)

![](/img/03/69.png)

And for my other table, the Users post, I can again add some items. So user ID 123, post ID 456, and create the item.

![](/img/03/70.png)

![](/img/03/71.png)

![](/img/03/72.png)

As well as for the demo indexes, I can create an item.

![](/img/03/73.png)

 User ID 123, game timestamp 2022, even if it's not really good. And then game ID 456. 

![](/img/03/74.png)

Okay, so I have created some items into all my tables. And now if you go to the PartyQL editor, we can look, for example, at the Users table. 

![](/img/03/75.png)

Then we can also get an adjacent view, if you wanted to, because this is what will be used in your code to deal with it. We can download the results for CSV.  

![](/img/03/76.png)

![](/img/03/77.png)

Next, for more complicated stuff, you can look at the demo indexes table. And we can scan this table again, so we have the look at all the items. But you can do more interesting stuff. For example, you can query the table. And when you query the table, it generates for you a statement. And you say select start from demo indexes, where user ID equals, for example, 123. And then you can also have an end game times 10 equals sort key value, but this is optional. And if you run this, obviously, then we get the correct items.  

![](/img/03/79.png)

And so we can start building some pretty complicated queries. But because we have an index right here, we can actually use this index and scan it. 

![](/img/03/80.png)

So we can do select start from demo indexes, and then the name of the index. And it's going to return the item based on my index. And you can do many things. You can run insert statements, although they're not easy to run directly from the UI, because they're not automatically generated. You can also set an item. So you can update a specific item and set the attribute value, the partition key value, and the sort key value, and so on. 

![](/img/03/81.png)

Or you can, for example, drop a specific item and then delete from statements.

![](/img/03/82.png)

So this editor is just for people who want to use SQL against DynamoDB. And I just wanted to show you the feature very briefly. 

#### DynamoDB Accelerator (DAX)

So now let's talk about DynamoDB Accelerator, or DAX. DAX is a fully managed, highly available, and seamless in-memory cache for DynamoDB. The idea is that you're going to cache the most popular data, and therefore you're going to get microsecond latency for cached reads and queries. It doesn't require you to change any of your application logic, it's compatible with the existing DynamoDB APIs, and you just create a DAX cluster and you're good to go. Now, DAX, what does it solve? It solves the hotkey problem. So if you're reading a very specific key, or a very specific item, too many times, then you may get throttling on your RCUs. But if it's cached by DAX, then you've just solved that problem. So let's have an example. We have a DynamoDB, it's made of tables, and our application is trying to access these tables. Now, in between, we're going to create a DAX cluster, which is made of cache nodes, and we're going to provision them in advance. Now, the application will directly interact with the DAX cluster, and the DAX cluster will fetch the data from the DynamoDB tables. So by default, that means that some data is going to be cached, and if you think cache, you need to think TTL, so the TTL is going to be 5 minutes, so by default, every cached data will live for 5 minutes in your DAX cluster. Now, the DAX cluster is made of nodes, and so therefore, you need to provision them, and you can have up to 10 nodes in the cluster, and it's a good idea to be in a multi-AZ kind of setup, to have at least three nodes recommended in production, so one in each AZ. And DAX is fully secure, so you have encryption at rest, you have IAM authentication, VPC security, cloud-share integration, and so on. So remember, DAX is here to help you cache the most popular items or queries from DynamoDB.

![](/img/03/83.png)

* Fully-managed, highly available, seamless in-memory cache for DynamoDB
* Microseconds latency for cached reads & queries
* Doesn’t require application logic modification (compatible with existing DynamoDB APIs)
* Solves the “Hot Key” problem (too many reads)
* 5 minutes TTL for cache (default)
* Up to 10 nodes in the cluster
* Multi-AZ (3 nodes minimum recommended for production)
* Secure (Encryption at



Now, the question we have is, what's the difference between DynamoDB, Accelerator, so DAX, and ElastiCache? Well, they can be used as a combination, and the exam may test you on figuring out if it's best to use DynamoDB, DAX, or if you want to use ElastiCache. So with DAX, what you're going to do is that you're going to have a cache for individual objects or for your queries or your scans. This is very, very handy, and this is what I call simple types of queries, so your objects, your queries, and your scans. But if you're doing some kind of logic application-wise, you're doing a scan, and then you're doing the sum, and then you're filtering out some data, and so on, and you don't want to do this every single time because this is competitionally expensive, what you can do is you can store the results of whatever your application just did in Amazon ElastiCache and retrieve the data from ElastiCache directly instead of re-querying DAX and re-performing the aggregation client-side. So this could be a good way to actually use them both together in architecture. Okay, so now let's go ahead and see how we can create a DAX cluster.

#### DynamoDB Accelerator (DAX) vs.ElastiCache

![](/img/03/84.png)

Now, the question we have is, what's the difference between DynamoDB, Accelerator, so DAX, and ElastiCache? Well, they can be used as a combination, and the exam may test you on figuring out if it's best to use DynamoDB, DAX, or if you want to use ElastiCache. So with DAX, what you're going to do is that you're going to have a cache for individual objects or for your queries or your scans. This is very, very handy, and this is what I call simple types of queries, so your objects, your queries, and your scans. But if you're doing some kind of logic application-wise, you're doing a scan, and then you're doing the sum, and then you're filtering out some data, and so on, and you don't want to do this every single time because this is competitionally expensive, what you can do is you can store the results of whatever your application just did in Amazon ElastiCache and retrieve the data from ElastiCache directly instead of re-querying DAX and re-performing the aggregation client-side. So this could be a good way to actually use them both together in architecture. Okay, so now let's go ahead and see how we can create a DAX cluster.

#### DynamoDB Accelerator (DAX) Hand On

![](/img/03/85.png)

So in the DynamoDB console, on the left-hand side, you have DAX. And DAX is not part of the free tier, so creating a DAX cluster is not going to be free. But you can just go ahead and see the process. So you create a cluster, for example, DemoDax. And then the node family. So which kind of nodes do you want to attribute into your DAX cluster? It could be T types or R types. So R is memory and T is for bursting. So this is recommended for use cases requiring a lower throughput. And this is for always-ready capacity. Or you can compare all families. And in here, you can select the node types you want. So you want an R5 large, an R4X large, and so on. Or do you want to have a T2 small, and so on. So in this example, I'll take a T2 small. And then the cluster size. So you want 1 up to actually 11 nodes. So they keep on increasing the capacity. 3 is going to give you a multi-AZ setup. But 1 is going to be good for, for example, just 1 AZ or for development. But if you have 1 or 2 nodes, you might experience reduced availability. So let's go ahead and do 1 node. I'm just going to go ahead and create this for you. 

![](/img/03/88.png)

![](/img/03/89.png)

But you don't have to go ahead because this will cost you some money. Now the subnet group is which subnet group you want to associate it with. So demo subnet group. So this has to live within a specific subnet group and in a VPC. So you choose your VPC ID. And then you choose the subnet you want to have. Obviously, 3 subnets means you can have 3 nodes and be in a highly available setup. 

![](/img/03/90.png)

Access control. So what is the security group to access your DAX cluster? And you need to open up port 8111. Or 9111 if you do intrinsic encryption. So we need to go ahead and create a security group from the EC2 console. But right now, I'll keep it simple and just use a default security group just to show you the process. And then AZ allocation. Do you want to have automatic or do you want to spread your nodes manually? So we'll keep it as automatic.

![](/img/03/91.png)

Next for IAM permissions. So we need to provide an IAM service role that will give the DAX cluster access to DynamoDB. And so therefore, we are just going to create this IAM role called DAX to DynamoDB. And the policy will be created. We'll provide rewrite rights. And we're going to give access to all tables. But we can obviously limit some tables if we needed to. And then we're going to encrypt data intrinsic and at rest on our DAX cluster. 

![](/img/03/92.png)

So this is good. And then for parameter groups, we're going to choose the existing one. So this one. And we can, on the left-hand side, define some more parameters thanks to parameter groups. 

![](/img/03/93.png)

So this is going to basically tell how long to cache the item thanks to the time to live. And what's the query time to live as well. So this is the setting you have as part of the parameter group. But by default, the one is giving you five minutes of TTL on both item time and query time. Okay. The maintenance window. 

![](/img/03/94.png)

And so you can say no preference or specify your time window, your tags. And then you're good to go. You can review it and create your cluster. 

![](/img/03/95.png)

And then you're good to go. You can review it and create your cluster. 

![](/img/03/96.png)

So my DAX cluster is now created. And let's have a look. So the important thing I want you to look at is here. There is a cluster endpoint. And this is the endpoint your application should leverage to just leverage the DAX feature. Now we can have a look at the nodes. So we can have a look at the node types, how many VCP per node, memory, and so on. So we can add nodes over time if we wanted to. 

![](/img/03/97.png)

But as you can see, we cannot change the node types. Okay.  And we need to create a new DAX cluster if you wanted to. 

![](/img/03/98.png)

We can have some monitoring. So we can do alarms and metrics around whether or not the cache is being used correctly. 


![](/img/03/99.png)

So cache hits, cache misses for items and queries, CPU utilization, and so on. So this is very, very handy to see if DAX is effective for you. Events to monitor the events of your database. As well as settings if you want to modify some settings such as a parameter group, network configuration, security configuration, maintenance window, and tags. 

![](/img/03/100.png)

Okay. So that's it for DAX. I hope you liked it. Now I'm just going to delete this cluster, including all the CloudWatch alarms. And I will see you in the next lecture. Bye.

![](/img/03/101.png)


#### DynamoDB - Streams

So let's have a look at DynamoDB Streams. So streams are an ordered list of item-level modifications, such as create, update, and delete, that are happening within a table. So whenever you will insert an item, or modify it, or delete it, then that modification would be visible in the stream, and the stream will represent the list of all the modifications over time in your table. So stream records can be sent to multiple places, such as Kinesis Data Streams, so you can send a DynamoDB Stream into Kinesis and then do whatever you want with it. You can also use a Lambda function to read directly from your DynamoDB Streams, or you can use the Kinesis Client Library application to read directly as well from a DynamoDB Stream. The data retention within a DynamoDB Stream is up to 24 hours, so you need to make sure to either persist it somewhere like Kinesis Data Stream, where you can have a longer retention, or use whatever Lambda or KCL application to persist it somewhere more durable. The use cases for using DynamoDB Streams is to react to changes in real time happening in your DynamoDB tables, for example, having a flow to send a welcome email to your users, to do analytics, to transform the stream and create derivative tables in DynamoDB, or to send data to OpenSearch for indexing and give search probabilities on top of DynamoDB, or if you want to implement global tables and cross-region replication, you will need to have streams in the first place. 

* Ordered stream of item-level modifications (create/update/delete) in a table
* Stream records can be:
  * Sent to Kinesis Data Streams
  * Read by AWS Lambda
  * Read by Kinesis Client Library applications
* Data Retention for up to 24 hours
* Use cases:
  * react to changes in real-time (welcome email to users)
  * Analytics
  * Insert into derivative tables
  * Insert into OpenSearch Service
  * Implement cross-region replication

![](/img/03/102.png)

So if you look at the architecture of DynamoDB Streams, so we have our application, which does create, update and delete operations on our table, and any of these changes is going to appear in a DynamoDB Stream. So from there, Kinesis Data Stream can be a receiver of the DynamoDB Stream, and because we're using Kinesis Data Streams, then we can have Kinesis Data Firehose as a result, and then maybe send it to Amazon Redshift to perform some analytics queries on top of your data in DynamoDB, or send it to Amazon S3 for archival of all these changes, in case we need to, or sending it to OpenSearch Service to index it and to create a search capability on top of your DynamoDB table. The cool thing about this architecture is that pretty much everything is managed by AWS. If you wanted to add your own custom logic, you could use a processing layer, in which you would create either a Kinesis client library app, maybe running on EC2, or a Lambda function that would be reading from DynamoDB Streams, and from this, you can implement any sort of logic you want. So for example, you can have messaging or send notifications in Amazon SNS, you can do some filtering and transformation and then reinsert the data into a DynamoDB table, or for example, you can also use Lambda to send data into OpenSearch if you wanted to. So this gives you a different kind of architecture and all the possibilities that open up by using DynamoDB Streams. 


**DynamoDB Streams**

So the cool thing about DynamoDB Streams, though, is that we don't have to provision any kind of shards. This is done automatically by AWS, so it is really a hands-off approach. Now, if you enable DynamoDB Streams, just so you know, the records are not going to be retroactively populated in the stream after enabling it, because this is an exam trick. So once you enable the stream, only then will you receive these updates based on the changes that are appearing in your DynamoDB table.

* Ability to choose the information that will be written to the stream:
  * **KEYS_ONLY** – only the key attributes of the modified item
  * **NEW_IMAGE** – the entire item, as it appears after it was modified
  * **OLD_IMAGE** – the entire item, as it appeared before it was modified
  * **NEW_AND_OLD_IMAGES** – both the new and the old images of the item
* DynamoDB Streams are made of shards, just like Kinesis Data Streams
* You don’t provision shards, this is automated by AWS
* **Records are not retroactively populated in a stream after enabling it**


#### DynamoDB Streams & AWS Lambda

Finally, let's have a look at how DynamoDB Streams and Lambda work. So for this, we need to define an event source mapping to read from a DynamoDB Stream, and then you need to ensure that the Lambda function has the appropriate permissions to pull from the DynamoDB Stream, and then the Lambda function will be invoked synchronously. So let's take an example. The table goes into a DynamoDB Stream. The Lambda function will have an event source mapping, which is an interval process that will be pulling the DynamoDB Stream and retrieving records in batches from the DynamoDB Stream, and once the records are passed on to the event source mapping, then the event source mapping will invoke the Lambda function synchronously with a batch of records from the DynamoDB Stream. So that's it for this lecture. I hope you liked it, and I will see you in the next lecture for some hands-on.

* You need to define an Event Source Mapping to read from a DynamoDB Streams
* You need to ensure the Lambda function has the appropriate permissions
* Your Lambda function is invoked synchronously

![](/img/03/103.png)

#### DynamoDB - Streams - Hand On

Let's get our user's post table, and on it we're going to enable DynamoDB Streams. So to do so, let's go to Exports and Streams, and here we're going to have DynamoDB Streams Details, and we're going to enable it. 

![](/img/03/104.png)

Now we have the option to choose which type of view we want to have within our DynamoDB Streams. 


And it should be Key Attributes, New Image, Old Image, or New and Old Images. So I'm going to keep the last option to get as much information as possible, and I will enable the stream. 

![](/img/03/105.png)

So now my stream is enabled, and within it, as you can see, if I scroll down, there is the trigger.

![](/img/03/106.png)

And the trigger is, what is the stream going to trigger? So we can create a trigger, and here we have a lambda function that can be invoked every time your DynamoDB Stream is updated. So to do so, let's go ahead and create a new function.

![](/img/03/107.png)

**`Create new`**

And we can use a blueprint, and in here I can type DynamoDB, and we have a DynamoDB Process Stream Python, which is going to log all the updates made to a table. So this is good. Let's configure it, and I'll call it Lambda Demo DynamoDB Stream. Next, we're going to create a new role with basic Lambda permissions, and we'll make sure to add this role to add the permissions to read from DynamoDB. Now for DynamoDB triggers, we need to create the trigger, so it's going to be the Users Post, which table was it? The Users Post table, yes, indeed. And the batch size is 100. I guess this is how many records were read at a time. The batch window, if you wanted to gather records before invoking the function, so to be more efficient. And the starting position, so if you want to read from the start or from the end of the stream, just in case the stream had been already created. So we're good, we're able to trigger, and as you can see, what this does is that it just prints the records that we get out of the stream into the logs, so we'll be able to view this in CloudWatch logs.

![](/img/03/108.png)


![](/img/03/109.png)

So now we have created a function, but we have an error, which is that the function cannot access our stream, because we have missing IAM permissions. So we're going to fix this, so we click on our function, and then under Configuration, we're going to go under Permissions, and this is the `execution role`. 

![](/img/03/110.png)

So let's attach this policy, and to be honest, one of these is going to work, I'm trying to make this as simple as possible, which is why I'm not spending a lot of time on this. 

![](/img/03/111.png)

Now we have to click on this execution role, and we're going to add the necessary permissions to read from DynamoDB. So we'll attach the policy, and I will look for DynamoDB, and we'll have DynamoDB read-only access, because what we're doing is that we're actually reading from DynamoDB. So this is good, and this one is also good to read from DynamoDB. So we'll attach these two just in case, but this is meant for Lambda, so probably a little bit better. 

![](/img/03/112.png)


![](/img/03/113.png)

So let's attach this policy, and to be honest, one of these is going to work, I'm trying to make this as simple as possible, which is why I'm not spending a lot of time on this. So let's refresh now this page, 

![](/img/03/115.png)

and it seems like we're good to go. So we have more actions on DynamoDB, so this is good. 


![](/img/03/114.png)

And now if we refresh this, we can choose this function and set the batch size in 100, and we create this trigger. 

![](/img/03/116.png)

So now that means that my DynamoDB stream is going to trigger my Lambda function, 

![](/img/03/117.png)

and if I go back into my Lambda function and refresh this page, we can see that DynamoDB is indeed triggering my Lambda function. So this is an enabled type of integration with these settings.

![](/img/03/118.png)

3:25

So next what I'm going to do is just test it out. So we're going to go to our table, and what I'm going to do is click on View Items, and we're going to do a few things. For example, let's take this item, the second post of John, we're going to do action, edit it, and then I'm going to modify it. So I'm going to say second post here, edit. And click on Save Change, so this is good. And then we'll take Alice, and we're going to do action, and then duplicate, so this is going to create a new thing and a way to just add a bit more data. I say new Alice blog, and then create item. And then finally, we actually don't like this blog, so I'm going to delete it. So I'm going to do action, and then delete items, and yes. Okay, so we've done three kinds of operations. We've had an update, a create, and a delete. And so we went into Manage the Function. Manage the Function executed this code, which was printing, so logging these events. And so what I need to do is just go into CloudWatch Logs and have a look at whether or not we saw these information in our logs. So let's click on View the Logs in CloudWatch Logs. And here we have some information around this log stream. And as we can see in this log stream, we get a lot of information. Okay, so we get one line, so this was a modify, and this was a DynamoDB record, so it gives you the key, the user ID, and as well as the new image, okay, and the content, so we can see, yes, a second post, yeah, edit. And we can also see the old image, so what it was before. So that was like for one request. Then we have an insert, and so again, we can see the DynamoDB record in it, and then the new image, and there's no old image because obviously it was an insert, so there was nothing before. And then we get a remove, and again, this remove operation was logged. We have the old image, and obviously there's no new image because the thing was removed, okay? So that's fairly easy. We just enable the stream, and it went to another function, and another function was logging it, but this is the basis to have these kind of integrations with DynamoDB Strips, okay? Lastly, please make sure to disable the trigger, so on the DynamoDB, you take this trigger, and you disable it or delete it, whatever you want, and you're good to go. So that's it for this lecture. I hope you liked it, and I will see you in the next lecture.