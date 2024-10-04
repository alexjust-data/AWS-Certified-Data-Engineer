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
    - [DynamoDB - Time To Live (TTL)](#dynamodb---time-to-live-ttl)
    - [DynamoDB - Patterns with S3](#dynamodb---patterns-with-s3)
    - [DynamoDB – Security \& Other Features](#dynamodb--security--other-features)
- [Amazon RDS (Relational Database Service)](#amazon-rds-relational-database-service)
  - [Amazon Aurora](#amazon-aurora)
  - [Using the LOCK command](#using-the-lock-command)
  - [Amazon RDS best practices](#amazon-rds-best-practices)
  - [Amazon RDS operational guidelines](#amazon-rds-operational-guidelines)
  - [Query optimization in RDS](#query-optimization-in-rds)
  - [DB-specific tweaks](#db-specific-tweaks)
  - [DocumentDB (NoSQLdatabase)](#documentdb-nosqldatabase)
  - [MemoryDB for Redis](#memorydb-for-redis)
  - [Keyspaces (for Apache Cassandra)](#keyspaces-for-apache-cassandra)
  - [Amazon Neptune](#amazon-neptune)
  - [Amazon Timestream](#amazon-timestream)
- [Amazon Redshift](#amazon-redshift)
  - [What is Redshift?](#what-is-redshift)
  - [Redshift Use-Cases](#redshift-use-cases)
  - [Redshift architecture](#redshift-architecture)
  - [Redshift Spectrum \& Performance](#redshift-spectrum--performance)
  - [Redshift Durability and Scaling](#redshift-durability-and-scaling)
  - [Redshift Distribution Styles](#redshift-distribution-styles)
    - [Importing / Exporting data](#importing--exporting-data)
    - [COPY command: More depth](#copy-command-more-depth)
    - [Redshift copy grants for cross-region snapshot copies](#redshift-copy-grants-for-cross-region-snapshot-copies)
    - [DBLINK](#dblink)
    - [Integration with other services](#integration-with-other-services)
    - [Concurrency Scaling](#concurrency-scaling)
    - [Automatic Workload Management](#automatic-workload-management)
    - [Manual Workload Management](#manual-workload-management)
    - [Short Query Acceleration (SQA)](#short-query-acceleration-sqa)
    - [Resizing Redshift Clusters](#resizing-redshift-clusters)
    - [VACUUM command](#vacuum-command)
    - [Redshift anti-patterns](#redshift-anti-patterns)
  - [Resizing Redshift Clusters](#resizing-redshift-clusters-1)
  - [RA3 Nodes, Cross-Region Data Sharing, Redshift ML](#ra3-nodes-cross-region-data-sharing-redshift-ml)
    - [Newer Redshift features](#newer-redshift-features)
    - [Amazon Redshift ML](#amazon-redshift-ml)
  - [Redshift security](#redshift-security)
  - [Redshift Serverless](#redshift-serverless)
    - [Redshift Serverless: Getting Started](#redshift-serverless-getting-started)
    - [Resource Scaling in Redshift Serverless](#resource-scaling-in-redshift-serverless)
  - [Redshift Materialized Views](#redshift-materialized-views)
    - [Using Materialized Views](#using-materialized-views)
  - [Redshift Data Sharing / Data Shares](#redshift-data-sharing--data-shares)
  - [Redshift Lambda UDF](#redshift-lambda-udf)
  - [Redshift Federated Queries](#redshift-federated-queries)
  - [Redshift System Tables and System Views](#redshift-system-tables-and-system-views)
  - [Redshift Data API](#redshift-data-api)
    - [Redshift Data API: Other use cases](#redshift-data-api-other-use-cases)
    - [Redshift Data API: Finer Points](#redshift-data-api-finer-points)
  - [Redshift - Hands On](#redshift---hands-on)

`Managing and querying structured and semi-structured data`

Up next, we're going to dive into the details of the various database technologies Amazon offers for storing your structured or semi-structured data. We will cover DynamoDB, which has a lot of features and details these certification exams try to trip you up with. I'll cover Amazon RDS, along with a few specific points about RDS that the exam guide calls out and best practices for using it. We will touch on DocumentDB, MemoryDB for Redis, Amazon Keyspaces for Apache Cassandra, and Amazon Neptune. Then I'll resurface, going into depth on Amazon Redshift. Redshift is Amazon's data warehousing solution, and it's a very important component in data engineering and data analytics that you can expect to see a lot of on the exam. As with every section, we'll wrap up with a quiz featuring example questions to reinforce what you've learned and give some practice in thinking the way the exam expects you to. It's not enough to learn the trivia about each service. You need to think through how to assemble these services and features into larger systems, and these quizzes will give you some experience in doing that. This is one of the longer sections in the course, so let's get started. Let's dive into database systems on AWS.

## Amazon DynamoDB

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


So next what I'm going to do is just test it out. So we're going to go to our table, and what I'm going to do is click on View Items, 

![](/img/03/119.png)

and we're going to do a few things. For example, let's take this item, the second post of John, we're going to do action, edit it, 

![](/img/03/120.png)

and then I'm going to modify it. So I'm going to say second post here, edit. 

![](/img/03/121.png)

And click on Save Change, so this is good. And then we'll take Alice, and we're going to do action, and then duplicate, so this is going to create a new thing and a way to just add a bit more data. 

![](/img/03/122.png)

I say new Alice blog, and then create item. 


![](/img/03/123.png)

And then finally, we actually don't like this blog, so I'm going to delete it. So I'm going to do action, and then delete items, and yes. 

![](/img/03/124.png)

Okay, so we've done three kinds of operations. We've had an update, a create, and a delete. And so we went into Manage the Function. Manage the Function executed this code, which was printing, so logging these events. 

![](/img/03/125.png)

And so what I need to do is just go into CloudWatch Logs and have a look at whether or not we saw these information in our logs. So let's click on View the Logs in CloudWatch Logs. 

![](/img/03/127.png)

And here we have some information around this log stream. 

![](/img/03/128.png)

And as we can see in this log stream, we get a lot of information. Okay, so we get one line, so this was a modify, and this was a DynamoDB record, so it gives you the key, the user ID, and as well as the new image, okay, and the content, so we can see, yes, a second post, yeah, edit. And we can also see the old image, so what it was before. So that was like for one request. Then we have an insert, and so again, we can see the DynamoDB record in it, and then the new image, and there's no old image because obviously it was an insert, so there was nothing before. And then we get a remove, and again, this remove operation was logged. We have the old image, and obviously there's no new image because the thing was removed, okay? So that's fairly easy. 

![](/img/03/129.png)

We just enable the stream, and it went to another function, and another function was logging it, but this is the basis to have these kind of integrations with DynamoDB Strips, okay? Lastly, please make sure to disable the trigger, so on the DynamoDB, you take this trigger, and you disable it or delete it, whatever you want, and you're good to go. 


![](/img/03/130.png)


#### DynamoDB - Time To Live (TTL)

So now let's talk about TimeToLive. TimeToLive allows you to automatically delete items after an expiry timestamp. So the idea is that you have a column you'll define, and then when you say the time of right now goes over the value of this column, then please remove the item. So having an item being deleted by a TimeToLive constraint does not consume any WCU, so there's no extra cost, and this timestamp must be a number that represents the Unix epoch timestamp value, as we'll see in the hands-on. Non-expired items will not be expired right away. There is a guarantee that they will be expired within 48 hours of the expiration, but in real life, it's actually pretty good. So if you look at a table, for example, a SessionData table, that has two columns, UserId and SessionId, we want to add an expiry time, which is going to be a TTL of our table, and we're going to define when each session will expire. Now what happens is that when you go and there is an expiration process in DynamoDB, it will look at the current time and say the current time is this, and it will mark, scan the table, and expire items that are going to obviously have a TTL epoch time less than the time right now. And then the second process will scan and delete these items from the table, and this is how TTL works. Now, these expired items that have been deleted will still appear in the read, query, and scan, so if you don't want them, you need to do some client-side filtering, so there could be some expired items already in your queries. You need to wait up to 48 hours to see it being deleted. And when the items are deleted, they're also deleted from your indexes, so your local secondary indexes and your global secondary indexes, and a delete operation for each expired item enters the DynamoDB stream. That means that any item that gets deleted thanks to the TTL will be in that stream, and you can recover it if you wanted to. Now the use cases for TTL would be to reduce stored data by keeping only current items, to adhere to regulatory obligations, or, for example, for the session data, it is a perfect use case of TTL. Okay, so let's have a look at how we can define a TTL in DynamoDB. So let's go ahead and create a table.

![](/img/03/131.png)

* Automatically delete items after an expir
* Doesn’t consume any WCUs (i.e., no extra cost)
* The TTL attribute must be a “Number” data type with “Unix Epoch timestamp” value
* Expired items deleted within 48 hours of expiration
* Expired items, that haven’t been deleted, appears in reads/queries/scans (if you don’t want them, filter them out)
* Expired items are deleted from both LSIs and GSIs
* A delete operation for each expired item enters the DynamoDB Streams (can help recover expired items)
* Use cases: reduce stored data by keeping only 


![](/img/03/132.png)

![](/img/03/133.png)

We'll customize the settings, we'll set provisions, and we'll have one RCU and one WCU, and then we'll go ahead and create this table. 

**`create table`**

So my table is now created, and what I'm going to do is just add a little bit of data. 

![](/img/03/134.png)

![](/img/03/135.png)

![](/img/03/136.png)


So I'm going to insert some items, so I'm going to create an item,

![](/img/03/138.png)

user ID is John123, and then I'm going to add an attribute, for example, name is going to be John, and then I want to have an expireOn attribute. So expireOn, essentially it's not a string, it's going to be a number. So we do expireOn, and then we need to give an expiration date to John. So I call it epochConverterOnline, and let's just get the first one.

https://www.epochconverter.com/

![](/img/03/137.png)

 And so this is how we can translate a timestamp of soon into an epoch timestamp, which I can enter into DynamoDB. So, for example, if I take, say, five minutes from now, so here we go, and I do human to timestamp, here is the epoch value of five minutes from now. So I'm going to paste that in and click on Create Item.

![](/img/03/138.png)

So that's one item that has been created, and I will create the second item, and this one is Alice456, it's a user ID, well, Alice's name is going to be Alice, and we need to, again, have an expireOn. And the expireOn, we can say, for example, one hour from now, so we'll have 10 here, and click on human data timestamp, take this in, and paste it, and create item. So now we have two rows in our DynamoDB table, and both of them have a different expireOn attribute.

![](/img/03/140.png)

![](/img/03/139.png)

Now we need to go ahead and define the TTL on our table. So if we look at our table, 

![](/img/03/142.png)

we can see the TimeToLive is currently disabled, so what I can do is go to Additional Settings, 

![](/img/03/143.png)

scroll down, and then look for the TimeToLive, and click on Enable.

![](/img/03/144.png)

**`Turn on TTL`**

Now we need to give the TTL attribute name, so what is the data that I want to expire, the column I want to expire on, so expireOn is the name that I give to this TTL attribute name. And then we can run a preview, 

![](/img/03/145.png)

Now we need to give the TTL attribute name, so what is the data that I want to expire, the column I want to expire on, so expireOn is the name that I give to this TTL attribute name. And then we can run a preview, okay? So if we run a preview right now, it says that these two items will be deleted in one hour, so if we take, for example, the time right now, which is right here, and I do Run Preview, it says, hey, there's no items that I want to delete, right? But if we do one hour from now, so if we do, for example, 10.10.10, and I get this epoch timestamp right here, and paste it, Run Preview, only John will be expired, okay? And then if I go a bit after, so if I go to 10.50, and take a human timestamp, data timestamp, and paste this in, Run a Preview, now two items will be deleted. And you could either specify an epoch value, or a custom time, or in the next 60 minutes, 24 hours, or seven days, which is quite cool for running some simulations. So we can enable TTL, and now TTL is enabled, and automatically, if I wait for one hour, my items are going to be completely expired, okay? So this is pretty cool, and we can do a graph of all the items deleted in the last 24 hours, thanks to this feature right here, okay? So this is a Kawash metric. So that's it for this lecture. I hope you liked it.

![](/img/03/146.png)


#### DynamoDB - Patterns with S3

So now let's look at two ways we can use DynamoDB with Amazon S3. And the first one is how to store large objects in DynamoDB. Well, it turns out that, as you know, in your tables in DynamoDB, you can only store up to 400 kilobytes of data. So obviously, if you just want to start storing some images, some videos, all that kind of stuff, DynamoDB is not the best place for it. So instead, what we're going to do is that we're going to have an Amazon S3 bucket that will contain our large objects. So what is the process to upload a large object? Well, say we upload an image into Amazon S3. We're going to get back an objects key. And what we're going to do is that we're going to store this metadata from the application into DynamoDB. So we'll have a product ID, a product name, and then an image URL, which is a pointer directly into Amazon S3. Now, what we've done is that we've effectively stored a very small amount of data in our products table in DynamoDB, and we've stored the large item in Amazon S3. Now, from the reading perspective, a client who wants to read this data first gets the metadata from DynamoDB, and then we'll get the image back from Amazon S3 to reconstruct these large objects. So we can go on and on and have many different products and use a strategy at scale. And the cool thing about this strategy is that we're using each service for what it's good at. So Amazon S3 is great for storing large objects, and DynamoDB is great for storing small objects that are going to be indexed with specific attributes. So in this example, we have the perfect combination of Amazon S3 and DynamoDB.

![](/img/03/147.png)


**DynamoDB – Indexing S3 Objects Metadata**

Another combination or synergy we can have is to use DynamoDB as a way to index S3 objects metadata. So the application is going to upload objects into Amazon S3, and Amazon S3 will have notifications set up, for example, to invoke a Lambda function. That Lambda function will store the objects metadata into DynamoDB table, for example, object size, date, who created it, whatever you want to think of about these objects. And why do we do this? Well, because it's much easier for us to build some queries on top of the DynamoDB table than on top of an S3 bucket. Again, an S3 bucket is not meant to be really scanned. It's meant to store large objects, and you're supposed to have some sort of database that knows what these objects are, their attributes, and so on. So by creating an application on top of DynamoDB, we can answer some questions such as, hey, we want to find objects by a specific timestamp on our S3 bucket, or we want to find the total storage used by a customer, or list all the objects with certain attributes, or find all the S3 objects uploaded within a date range. All of this by querying DynamoDB table, and then we read back the results from the DynamoDB, and we retrieve the necessary objects from your S3 buckets, okay? So hopefully these two strategies make sense. They're quite common, and they can come up at the exam. I hope you liked it. And I will see you in the next lecture.

ChatGPT said:
ChatGPT


![](/img/03/148.png)

#### DynamoDB – Security & Other Features

So one last talk about DynamoDB security and some other features. So for security, we have VPC endpoints that are going to be available to access DynamoDB without using the public internet and only keeping all the traffic within your VPC. Access to DynamoDB is fully controlled by IAM, which makes it a great database choice in AWS. And you have encryption at rest using AWS KMS or in transit using SSL and TLS. There are backup and restore features available. You have two of them, so you have point-in-time recovery, so PITR, just like RDS, and there's no performance impact, or you can just do a normal backup and restore. Now there's the concept of global tables in DynamoDB. So the idea is that you have a multi-region, multi-active, fully replicated high-performance table in DynamoDB. And how to enable it? Well, you need to first enable DynamoDB Streams. So while DynamoDB is a cloud service, it's possible for you to get a simulation of DynamoDB on your local computer called DynamoDB Local. And the idea is that you have a local DynamoDB database that you can use to develop and test your applications locally without using the DynamoDB web service, which is really, really handy. And if you wanted to migrate data to and from DynamoDB, the AWS Database Migration Service is a great choice. So for example, from MongoDB to DynamoDB, or DynamoDB to Oracle, MySQL, S3, and so on.

* Security
  * VPC Endpoints available to access DynamoDB without using the Internet
  * Access fully controlled by IAM
  * Encryption at rest using AWS KMS and in-transit using SSL/TLS
* Backup and Restore feature available
  * Point-in-time Recovery (PITR) like RDS
  * No performance impact
* Global Tables
  * Multi-region, multi-active, fully replicated, high performance
* DynamoDB Local
  * Develop and test apps locally without accessing the DynamoDB web service (without Internet)
* AWS Database Migration Service (AWS DMS) can be used to migrate to DynamoDB (from MongoDB, Oracle, MySQL, S3, …)


**DynamoDB – Users Interact with DynamoDB Directly**

Now another feature you need to understand around DynamoDB is going to be around fine-grained access. So for example, if you have clients and applications, web or mobile, and they need to access directly our DynamoDB table, then we don't want to give them IAM permissions and IAM roles of users directly from AWS. That would be truly inefficient and a security hole. Instead, we're going to use an identity provider. It could be Amazon Cognito User Pools, Google Login, Facebook Login, OpenID Connect, or SAML, or whatever. And the users will, in the simplified flow, log in with these identity providers, and they will have the feature to exchange the credentials they just got for temporary AWS credentials. And the idea is that because they're temporary, they're more secure. And with them, they can be associated with an IAM role. But this IAM role must be restricted because now that our clients and applications can access a DynamoDB table, you want them to be able to do operations only on the data that they own.

![](/img/03/149.png)


**DynamoDB – Fine-Grained Access Control**

And so how do we do this, this fine-grained access control? Well, we have a federated login to get temporary credentials. Then we create an IAM role, and this IAM role will have a condition. And this condition will lead onto what the user can do. So here is a sample policy. So in this policy, the user can do get item, batch get item, query, put item, update item, delete item, and batch write item on a specific table. But there's a condition here, and the condition is saying, hey, only if the leading key is corresponding to DynamoDB, and then the connect identity pseudo variables, which will be replaced at runtime by the specific user. And so effectively, what we're saying is that with leading key, we only limit row-level access for users based on what the primary key value is. And so therefore, we make sure that the users can only modify and access their own data. And you can also specify conditions on attributes. And this would be to limit the specific attributes a user can see in your DynamoDB table. So to summarize, you have fine-grained access control by using a federated login, and by specifying a condition on leading keys if you want to limit at the row level, or attributes if you want to limit at the column level, the attribute level. Okay, that's it for this lecture. I hope you liked it, and I will see you in the next lecture. Let's talk briefly about Amazon RDS, the release.

![](/img/03/150.png)

* Using Web Identity Federation or Cognito Identity Pools, each user gets AWS credentials
* You can assign an IAM Role to these users with a Condition to limit their API access to DynamoDB
* LeadingKeys – limit row-level access for users on the Primary Key
* Attributes – limit specific attributes the user can see


## Amazon RDS (Relational Database Service)

Let's talk briefly about Amazon RDS, the relational database service. It's not really relevant to big data, it's really more for small data, but it will come up on the exam and you need to know what it is. Usually sort of an example of what not to do or maybe an example of how to import your RDS data into a larger big data system. 

**What is RDS?**

What is RDS? Well, it is a hosted relational database and that can be any of the following database types. Amazon Aurora, which we'll talk about shortly. You can also host a MySQL database, a PostgreSQL database, MariaDB, which is basically an open source version of MySQL, Oracle, or a Microsoft SQL server. So it's a way to just have AWS host your own little database host so you don't have to worry about maintaining that database host yourself, which as we all know, being a DBA is not always a fun thing, kind of a handy thing. But it's not for big data. Like I said, it might appear on the exam as an example of what not to use for a big data problem or in the context of migrating from RDS into Redshift or something like that. It's made for smaller needs where you can get away with a single database host and store all of your data there. For example, I use RDS and Amazon Aurora for backing up my websites. So for WordPress, it's using RDS as its backing store for that. 


* Hosted relational database
  * Amazon Aurora
  * MySQL
  * PostgreSQL
  * MariaDB
  * Oracle
  * SQL Server
* Not for “big data”
  * Might appear on exam as an example of what not to use
  * Or in the context of migrating from RDS to Redshift etc.

**ACID**

A good time today to talk about ACID compliance. This comes up frequently in the world of relational databases in general and all the RDS databases offer full ACID compliance. So if you do need a database that meets these requirements, RDS can be a solution provided that you don't need massive datasets that can't fit on a single host. Now, under the hood, it might actually be using more than one host. That's all kind of a black box to us, but it still makes sense to think of RDS as more of a small data solution. ACID, anyway, stands for Atomic, Consistent, Isolated, and Durable. And just a little refresher here for you guys. So atomicity ensures that either the transaction as a whole is successfully executed or if part of a transaction fails, then the entire transaction is invalidated. So if you're sending a transaction that does more than one thing, if any bit of that fails, the entire transaction gets discarded. Consistency ensures that the data written to the database as part of the transaction must adhere to all defined rules and restrictions, including constraints, cascades, and triggers. Isolation ensures that each transaction is independent unto itself, and this is critical in achieving concurrency control. Finally, durability ensures that all the changes made to the database be permanent once a transaction is successfully completed.


* RDS databases offer full ACID compliance
  * Atomicity
  * Consistency
  * Isolation
  * Durability

### Amazon Aurora

 Let's talk a bit about Amazon Aurora, which is another choice for RDS. It provides support for MySQL and PostgreSQL databases. So for example, if you wanted to back your own WordPress site or something with Amazon Aurora, you could do that and it would just look like MySQL to WordPress as far as it's concerned. So it's a hosted alternative for MySQL and PostgreSQL. And according to the marketing speak from Amazon Web Services, it's up to five times faster than MySQL and three times faster than PostgreSQL. Of course, your mileage may vary. Actually, realizing those exact gains may not be possible. That's kind of marketing speak, but it is faster. It's also cheap. It can be up to one-tenth of the cost of commercial databases. Sort of an unfair comparison there, though, because MySQL is not a commercial database, right? So they're basically saying compared to getting Oracle or something like that, it's going to be a lot cheaper. But using just open source MySQL, it would be a lot cheaper as well. The current limit for storage is up to 128 terabytes per database volume. So note that the storage is decoupled from the actual instance for Aurora. And it also offers up to 15 read replicates. So if you need higher performance, you can use read replicas to gain faster, more distributed, scalable access to your underlying data. That's a good feature. And that also decreases the replication time down to milliseconds. So that's a handy thing, too. It also offers continuous backup to S3, so you never have to worry about losing your data. And it also offers replication across regions and across availability zones. So you can replicate your data all over the world, however you want to. It also offers a newer, I should say, service called Aurora Serverless. 


* MySQL and PostgreSQL – compatible
* Up to 5X faster than MySQL, 3X faster than PostgreSQL
* 1/10 the cost of commercial databases
* Up to 128TB per database volume
* Up to 15 read replicas
* Continuous backup to S3
* Replication across regions and availability zones
* Automatic scaling with Aurora Serverless


**Aurora Security**

So if you don't want to dedicate hardware to it and pay for that hardware all month long, if you have more dynamic, inconsistent traffic to your database, Aurora Serverless might be a better choice for you. That offers automatic scaling with your traffic, as opposed to just paying for a server, a database host sitting there all the time waiting for you that may or may not be getting used. But if you have a use case where you're hitting that database pretty consistently 24-7, you're probably better off with a dedicated database instance. But you have the choice with Aurora. You can have your own database. But you do have the choice with Aurora. You can either use your own dedicated database host, or you can use Aurora Serverless and have that capacity scale up automatically. As far as security goes with Aurora, it offers VPC network isolation, obviously. It can also do at-rest encryption using KMS. So data, backup, snapshots, and replicas can all be encrypted. And it can do in-transit security as well using SSL. So that's Aurora in a nutshell and RDS. Again, usually not in the context of big data, but Aurora is going in interesting directions. So we'll keep an eye on that going forward.


* VPC network isolation
* At-rest with KMS
* Data, backup, snapshots, and replicas can be encrypted
* In-transit with SSL


### Using the LOCK command

One thing I want to call out explicitly is the idea of using locks in relational databases. That is something that shows up on the exam guides, so we better talk about it. One way of doing that is using the lock command. So the idea of locks is preventing two people from doing the same thing at the same time to something, right, and getting conflicting results as a result. So implicitly, relational databases will lock your tables to prevent that from happening. So you don't want two people writing at the same time to the same piece of data, right? And you also don't want people reading data while that write is in process to that bit of data. So generally, to ensure ACID compliance, databases will automatically and implicitly lock things to prevent that sort of thing from happening. You don't really need to do anything special about that. However, it is possible to explicitly lock tables or explicitly lock rows to ensure your own data integrity and concurrency control. If you don't want to just put your faith in the database to do the right thing, there could be some special circumstances where you want more explicit control over those locks. So there's two kinds of locks. There's what's called a shared lock and an exclusive lock. So the syntax for a shared lock is for share. We'll see an example of that on the next slide. But a shared lock allows reads but prevents writes, and it can be held by multiple transactions. So if you want to allow people to read the data that's being accessed by your transaction but prevent people from writing to it during your transaction, you can say for share to enforce a shared lock. You can also have an exclusive lock that prevents all reads and writes to a resource, and only one transaction can hold an exclusive lock at a time. That's why it's called exclusive. The syntax for that is for update, and that says, while I'm accessing this data, nobody can read it, nobody can write to it, nobody can look at it or touch it but me. 


* Relational databases implicity `lock` tables to prevent two things writing to it at the same time, or reading while a write is in process.
* Tables or rows can also be explicitly locked to ensure data integrity and concurrency control.
* Types of locks:
  * Shared Locks: Allow reads, prevent writes. Can be held by multiple transactions. (FOR SHARE)
  * Exclusive Locks: Prevent all reads and writes to a resource. Only one transaction can hold an exclusive lock. (FOR UPDATE)

**Examples (MySQL)**

Some examples, and these are in MySQL, the syntax, again, varies from database to database, so don't get too hung up on the syntax. But if you wanted to lock an entire table, you could say lock tables employees, where employees is the name of the table I want to lock, write. That would lock the entire employees table against being written to. And when I'm done, I would have to say unlock tables to release that lock. Now Redshift also has a lock command that works very much the same way for the same purpose. So if I want to make sure nobody can write to my table while I'm doing something, lock tables is the way to do it. And thinking in terms of what the exam might be asking about, that's probably what they're going to ask about. I don't know, I haven't actually taken the exam yet when I'm recording this because it's before the beta exam right now. So I'm not giving you any forbidden knowledge there, I'm just an educated guess. Also an example of doing a shared lock. So let's say I want to allow reads but prevent writes during this transaction. So I'm not explicitly locking and unlocking here. You share it in exclusive locks or just applying to the transaction I'm in. So while the select statement is being executed, because I'm saying for share, that means that during this execution, other processes can do reads from the table, but nobody can write to it while that's being executed. And just for completeness, for update here is an example of an exclusive lock. During the lifetime of this transaction, nobody can read or write until that transaction is complete. So it is important to make sure that any transactions that do have a lock do complete. So if for some reason that transaction got interrupted or did not complete, you could end up with what's called a deadlock, where that lock is never released. So that is a potential failure mode, and again, maybe something the exam might expect you to know about. This exam focuses on operations and data engineering, so let's talk about some RDS best practices and operations to optimize the performance of your database and their operational stability. Tip number one is to use CloudWatch to keep an eye on your memory, CPU, storage, and replica lag on your RDS. Thank you.

* Lock an entire table:
  * `LOCK TABLES employees WRITE;` -- Locks the entire 'employees' table for write operations
  * Use `UNLOCK TABLES;` to release the lock.
  * Note: Redshift also has a LOCK command for the same purpose.
* Shared lock (allow reads, prevent other writes during this transaction.)
  * `SELECT * FROM employees WHERE department = 'Finance' FOR SHARE;`
* Exclusive lock (prevent all reads and writes during this transaction)
  * `SELECT * FROM employees WHERE employee_id = 123 FOR UPDATE;`
* Make sure the transactions


### Amazon RDS best practices

This exam focuses on operations and data engineering, so let's talk about some RDS best practices and operations to optimize the performance of your database and their operational stability. 

### Amazon RDS operational guidelines

Tip number one is to **use CloudWatch to keep an eye on your memory, CPU, storage, and replica lag** on your RDS instances. Kind of goes without saying that you should use CloudWatch, right, if you're going to be caring about the performance of any system, not just RDS. And if you're going to be doing automatic backups, try to do that during the time of day that has the lowest write IOPS. If you're trying to back something up while it's still being actively written to, that gets complicated. And don't just guess. Don't just say, well, nobody hits my database at two in the morning. No, use your CloudWatch metrics to figure that out and actually base that on some real data. 

Be aware that having insufficient I-O capacity on your instances are going to cause problems if you need to recover after a failure. So the last thing you want to happen is have a database go down on you and find that you're having a hard time switching to a backup instance because your I-O capacity is insufficient to do that failover. So make sure that your database instances has as much I-O as they need. You might need to migrate to one with more. One way to get better I-O capacity is to move to general purpose or provisioned IOPS storage for your RDS instances. Also, if you have applications hitting your RDS database, make sure the time to live on the DNS for that database is set to 30 seconds or less. Again, you don't want to be in a situation where you need to failover your database and your failover strategy is to change the DNS entry from one host to another. 

And then you find out that the DNS is being cached for an hour, right? So instead of switching over in 30 seconds, you now have to sit around for an hour while your cache is for DNS on your app's timeout. So make sure you think about the time to live on DNS for your database if you intend to use DNS as your failover mechanism. And test your failovers before you need it. You know, you don't want to be figuring this out when you're under the gun because things melted down and your applications are no longer functioning. At Amazon, we used to call this game day. We would simulate a massive failure of everything at Amazon in a test environment and kind of practice how we might deal with that. So a good idea to practice failover before you actually need to do it. Make sure you have enough RAM provisioned on your RDS instances to include your entire working set. Again, you don't have to guess about this. You can monitor the read IOPS metric. And if that metric is small and stable, that implies that you're not hitting disk very much so you probably have enough RAM to include your working set. However, if it's not stable and it's higher than you expect, that probably means you're hitting disk more than you should and you need more RAM. 

Also, if your data in RDS is being exposed externally through an app somehow to the outside world or a website, you might want to think about imposing rate limits in the Amazon API gateway to protect your database. So if you want to protect your database against denial of services attacks and things like that, you can set up a rate limit in the API gateway to make sure that nobody can hit your application more than some reasonable frequency that you are designed to handle. 

* Use CloudWatch to monitor memory, CPU, storage, replica lag
* Perform automatic backups during daily low in write IOPS
* Insufficient I/O will make recovery after failure slow.
* Migrate to DB instance with more I/O
* Move to General Purpose or Provisioned IOPS storage
* Set TTL on DNS for your DB instances to 30 seconds or less from your apps
* Test failover before you need it
* Provision enough RAM to include your entire working set
* If your ReadIOPS metric is small and stable, you’re good
* Rate limits in Amazon 

### Query optimization in RDS

Also for improving performance in RDS, you can optimize your queries. Query optimization 101 is use indices whenever you can. So whenever you have a select statement, make sure you look at your queries and make sure you have the indices you need to make those efficient. And if you're not sure, you can use the explain plans to identify any indexes you might need to support that query. Another way of saying the same thing is avoid full table scans. So if you're doing a full table scan, that means there's something wrong with your query or you don't have an index that you really should have, right? So full table scans or evil indexes are the way to avoid them. Once in a while, use the analyze table command to make sure that the table itself is in good health. If you have where clauses, try to make them as simple as possible. They can cause a lot of overhead. And then there are some engine specific optimizations that we'll talk about next. 
### DB-specific tweaks

So in MySQL and MariaDB, here are some specific tips.

One is to keep your tables well under 16 terabytes and ideally less than 100 gigabytes. Otherwise, you might run into performance problems with MySQL and its close cousin, MariaDB. Make sure you have enough RAM to hold the indexes of your actively used tables. Try to have less than 10,000 tables because more than that might cause problems. And for your choice of storage engine, AWS recommends using InnoDB. If you're using PostgreSQL, there are some tips for when you're loading data into PostgreSQL initially. Apparently, this can be slow. So you want to make sure you're disabling DB backups and also multi-AZ capabilities while you're doing your initial massive load into PostgreSQL. Furthermore, there are specific parameters you can tweak to make that happen more quickly. I'd be surprised if these showed up on the exam, but some are obvious, like making sure auto-vacuum is off during that operation. So the takeaway there is that loading data into PostgreSQL can be slow. There are some specific things you need to do to speed that up. When you're not loading data, though, you do want auto-vacuum on. That's going to be important for maintaining performance going forward. And finally, for SQL Server, you want to make sure you're using RDS DB events to keep an eye on any failovers that might be happening within SQL Server. Also, if you're using SQL Server in a multi-AZ availability zone environment, do not turn on simple recover mode, offline mode, or read-only mode. All of those will break a multi-AZ deployment, so important to remember. And they do recommend that you deploy your SQL Server into all availability zones for redundancy purposes and to make sure things keep working as well as they can. Finally, if you're using Oracle, well, that's its own beast. Optimization in Oracle could be the subject of a whole course of its own, and I really doubt they're going to get into the details of that because, well, people usually use the open source options on RDS these days anyhow. So that's RDS optimization in a nutshell. So things to remember for the exam.

* MySQL, MariaDB
  * Keep tables well under 16TB, ideally under 100GB
  * Have enough RAM to hold indexes of actively used tables
  * Try to have less than 10,000 tables
  * Use InnoDB for storage engine
* PostgreSQL
  * When loading data, disable DB backups and multi-AZ. Tweak various DB parameters such as maintenance_work_mem, max_wal_size, checkpoint_timeout. Disable synchronous_commit, autovacuum, and ensure tables are logged.
  * Use autovacuum
* SQL Server
  * Use RDS DB Events to monitor failovers
  * Do not enable simple recover mode, offline mode, or read-only mode (this breaks Multi-AZ)
  * Deploy into all AZ’s
* Oracle is its own beast

### DocumentDB (NoSQLdatabase)

Now, let's talk about DocumentDB. So the same way we had Aurora as the way for AWS to implement a sort of big cloud-native version of PostgreSQL and MySQL, we have DocumentDB, which is an Aurora version for MongoDB. So MongoDB, if you don't know, there's a logo on the top right corner of your screen, it is another NoSQL database, and you need to remember this for the exam, so DocumentDB is a NoSQL database, and it's based on top of the MongoDB technology, so it's compatible with MongoDB. So MongoDB is used to store, query, and index JSON data, and you have the same similar deployment concept as Aurora with DocumentDB. So that means it's a fully managed database, it's highly available, data is replicated across three availability zones, and DocumentDB storage automatically will grow in increments of 10 gigabytes. And DocumentDB has been engineered so it can scale to workloads with millions of requests per second. So at the exam, if you see anything related to MongoDB, think DocumentDB, or if you see anything related to NoSQL databases, think DocumentDB and also DynamoDB. That's it for this lecture, I hope you liked it, and I will see you in the next lecture.

* Aurora is an “AWS-implementation” of PostgreSQL / MySQL …
* DocumentDB is the same for MongoDB (which is a NoSQLdatabase)
* MongoDB is used to store, query, and index JSON data
* Similar “deployment concepts” as Aurora
* Fully Managed, highly available with replication across 3 AZ
* DocumentDB storage automatically grows in increments of 10GB
* Automatically scales to workloads with millions of requests per seconds

### MemoryDB for Redis

So now let's talk about Amazon MemoryDB for Redis. So it's a Redis-compatible, durable, in-memory database service. So the difference between Redis and MemoryDB for Redis is that while Redis's intent is to be used as a cache with some durability, MemoryDB is really a database that has a Redis-compatible API. So it gives you ultra-fast performance with over 160 million requests per second, so really really high performance, and it's in-memory data, but it's durable data storage with multi-AZ transaction log. So this is a different kind of functioning than Redis. It will scale seamlessly from tens of gigabytes to hundreds of terabytes of storage, and the use cases for MemoryDB for Redis will be your web and mobile applications, online gaming, media streaming, and so on. So imagine you have a lot of microservices and they need access to a Redis-compatible in-memory database, then this is perfect, you use MemoryDB for Redis, you're going to get ultra-fast in-memory speed, as well as a multi-AZ transaction log, which is going to be stored across multiple AZ and will give you access to fast recovery and data durability if you need it. Okay, just an overview, that should be enough for the exam

![](/img/03/151.png)

* Redis-compatible, durable, in-memory database service
* Ultra-fast performance with over 160 millions requests/second
* Durable in-memory data storage with Multi-AZ transactional log
* Scale seamlessly from 10s GBs to 100s TBs of storage
* Use cases: web and mobile apps, online gaming, media streaming,



### Keyspaces (for Apache Cassandra)

So now let's talk about Amazon Keyspaces. And Keyspaces is a managed Apache Cassandra on AWS. So Cassandra is an open-source NoSQL distributed database. And so with Keyspaces, you get Cassandra directly managed on the cloud by AWS. So it's going to be a serverless type of service. It's scalable, it's highly available, and fully managed by AWS. And it will automatically scale tables up and down based on the application's traffic. The table's data is going to be replicated three times across multiple AZ. And to perform your queries on top of Keyspaces, you're going to use the Cassandra Query Language, or CQL. So thanks to this, you're going to get single-digit millisecond latency at any scale. And you can do thousands of requests per second. Two capacity modes to note. This is just like DynamoDB. You have the on-demand mode, and then you have the provision mode with autoscaling. And these are the same things as DynamoDB, really. You get encryption features, backup, point-in-time recovery up to 35 days. And so the use cases is going to be to store IoT device information, time series data, and generally from an exam perspective, any time you see Apache Cassandra, you just think about Amazon Keyspaces, and you're done. Okay, that's it for this refresher.

* Apache Cassandra is an open-source NoSQL distributed database
--
* A managed Apache Cassandra-compatible database service
* Serverless, Scalable, highly available, fully managed by AWS
* Automatically scale tables up/down based on the application’s traffic
* Tables are replicated 3 times across multiple AZ
* Using the Cassandra Query Language (CQL)
* Single-digit millisecond latency at any scale, 1000s of requests per second
* Capacity: On-demand mode or provisioned mode with auto-scaling
* Encryption, backup, Point-In-Time Recovery (PITR) up to 35 days
--
* Use cases: store IoT devices info, time-series data, …


### Amazon Neptune

Now let's talk about Amazon Neptune. Neptune is a fully managed graph database. So an example of what a graph dataset would be, would be for example something we all know, which is a social network. So if we look at a social network, people are friends, they like, they connect, they read, they comment, and so on. So users have friends, posts will have comments, comments have likes from users, users share and like posts, and so all these things are interconnected, and so they create a graph. And so this is why Neptune is a great choice of database when it comes to graph datasets. So Neptune has replication across three AZ, up to 15 read replicas. It's built, it's used to build and run applications that are going to be with highly connected datasets, so like a social network, and because Neptune is optimized to run queries that are complex and hard on top of these graph datasets. You can store up to billions of relations on the database and query the graph with milliseconds latency. It's highly available with replication across multiple availability zones, and it's also great for storing knowledge graphs. For example, the Wikipedia database is a knowledge graph because all the Wikipedia articles are interconnected with each other. Threat detection, recommendations engine, and social networking. So coming from an exam perspective, anytime you see anything related to graph databases, think no more than Neptune. 

![](/img/03/152.png)

* Fully managed graph database
* A popular graph dataset would be a social network
  * Users have friends
  * Posts have comments
  * Comments have likes from users
  * Users share and like posts…
* Highly available across 3 AZ, with up to 15 read replicas
* Build and run applications working with highly connected datasets – optimized for these complex and hard queries
* Can store up to billions of relations and query the graph with milliseconds latency
* Highly available with replications across multiple AZs
* Great for knowledge graphs (Wikipedia), fraud 


### Amazon Timestream

So now let's talk about Amazon Timestream. And the name indicates that it's actually a time series database. So it's fully managed, it's fast, it's scalable, and it's serverless. So what is a time series? Well, it's a bunch of points that have a time included in them. So for example, here's a graph by year. So this is a time series. Now with Timestream, you can automatically adjust the database up and down to scale capacity, and you can store and analyze trillions of events per day. The idea is that if you have a time series database, it's going to be much faster and much cheaper than using relational databases for time series data, hence the specificity of having a time series database. You can do scheduled queries, you can have records with multiple measures, and there is full SQL compatibility. The recent data will be kept in memory, and then the historical data is kept in a cost-optimized storage tier. As well as you have a time series analytics function to help you analyze your data and find patterns in near real time. This database, just like every database on AWS, supports encryption in transit and at rest. So the use cases for Timestream would be to have an IoT application, operational applications, real-time analytics, but everything related to a time series database. 

* Fully managed, fast, scalable, serverless time series database
* Automatically scales up/down to adjust capacity
* Store and analyze trillions of events per day
* 1000s times faster & 1/10th the cost of relational databases
* Scheduled queries, multi-measure records, SQL compatibility
* Data storage tiering: recent data kept in memory and historical data kept in a cost-optimized storage
* Built-in time series analytics functions (helps you identify patterns in your data in near real-time)
* Encryption in transit and at rest
* Use cases: IoT apps, operational applications, real- time analytics, …

**Timestream – Architecture**

![](/img/03/153.png)

Now in terms of architecture, Timestream is here, and it can receive data from AWS IoT, so Internet of Things. Kinesis Data Streams through Lambda can receive data as well. Prometheus, Telegraph, there are integrations for that. Kinesis Data Streams as well through Kinesis Data Analytics for Apache Flink can receive data into Amazon Timestream and Amazon MSK as well through the same process. And in terms of what can connect to Timestream, well we can build a dashboard using Amazon QuickSight, we can do machine learning using Amazon SageMaker, we can do Grafana, or because there is a standard JDBC connection into your database, any application that is compatible with JDBC and SQL can leverage Amazon Timestream. So that's it, I think from the exam you just need to remember what Timestream is at a high level, but I want to give you a bit more details as well. So that's it for this lecture, I hope you liked it, and I will see you in the next lecture.



## Amazon Redshift 

**Fully-managed, petabyte-scale data warehouse**

Next, let's dive into Amazon Redshift. Redshift is AWS's distributed data warehouse solution. So it is a petabyte-scale data warehouse that is spread across an entire cluster, but it's fully managed. So, you know, you don't think about taking care of that cluster. They take care of all the server maintenance for you. Now, given that the world of big data often involves the world of data warehouses and dealing with massive data sets, it should be no surprise that the exam spends a lot of time talking about Amazon Redshift. So we're going to spend a lot of time on it as well. A lot of depth here needed on Redshift, so pay attention, folks. This stuff is all important. 

### What is Redshift?

At a high level, what is Redshift? Well, it is a fast and powerful, fully managed, petabyte-scale data warehouse service. And Amazon claims that it delivers 10 times better performance compared to other data warehouses. So not only is it massively scalable, it's super fast and achieves that speed using machine learning, massively parallel query execution, which is called MPP, and using columnar storage, which we talked about earlier, and using high-performance disks as well. Keep in mind, Redshift is a data warehouse, so it is specifically designed for online analytic processing, OLAP. It's for querying your data and getting insights out of it on an analytical standpoint. It is not made for OLTP. For OLTP, typically, you would want more row-based storage. So you're not going to be hitting your Redshift data warehouse at massive transaction rates, expecting fast responses. It's meant as an analytics tool, which is why it's in the analytics section here. They also claim that in addition to being super fast and super scalable, it's also super cheap. They claim it is the most cost-effective cloud data warehouse, and there is no upfront cost to using Redshift, and that is in stark contrast to building out a massive data warehouse on-premises. I can tell you that firsthand. You just pay as you go for the resources that you're consuming, and that can end up being one-tenth of the cost or less of traditional data warehouses that are stored on-premises. Also, it provides fast querying capabilities over structured data using familiar SQL-based clients and BI tools, just using standard ODBC and JDBC connections. So it just looks like another relational database to the outside world, and you can connect any analytic or visualization tool you want to Redshift on top of it. It's also easily scalable. You can easily scale up or down your cluster with just a few clicks in the AWS Management Console, or with a single API call. So if you do need to scale it up or scale it down, that's very easy to do. It won't be automatic, but at least it's easy. It also uses replication to enhance your availability, and continuous backups to improve your data durability. And it can automatically recover from component and node failures. For monitoring, it integrates with CloudWatch. And for metrics for compute utilization, storage utilization, and read-write traffic to the cluster, those are all available for you free of cost within CloudWatch. And you can also add user-defined custom metrics that you can add using CloudWatch's custom metrics functionality. It also provides information on query and cluster performance using the AWS Management Console, and that helps you in diagnosing performance issues like which user or query is consuming high resources.


* Fully-managed, petabyte scale data warehouse service
* 10X better performance than other DW’s
* Via machine learning, massively parallel query execution, columnar storage
* Designed for OLAP, not OLTP
* Cost effective
* SQL, ODBC, JDBC interfaces
* Scale up or down on demand
* Built-in replication & backups
* Monitoring via CloudWatch / CloudTrail

### Redshift Use-Cases

 Some use cases that are listed for Redshift are accelerating all of your analytics workloads. So if you just want your data warehouse to be faster, you might want to move to Redshift. It uses, as you said, machine learning, MPP, and columnar storage on high-performance disks, and result caching to make it super fast. You might also want to use Redshift because you want to unify your data warehouse and your data lake. Something we'll talk about shortly is Redshift Spectrum, which is a way of importing your unstructured data in S3 as just another table in your data warehouse. So you can actually do joins and stuff across your structured data that's been imported into your Redshift servers themselves, together with data lake information that's actually stored in S3 somewhere. That's kind of cool. Maybe you just want to modernize your data warehouse and make it faster and more scalable and easier to manage. Redshift is a potentially easy way of doing that. And there are some more specific use cases that come out of the AWS Big Data White Paper. Those would include analyzing global sales data, storing historical stock trade data, analyzing ad impressions and clicks, aggregating gaming data, and analyzing social trends. These are all examples of stuff you can do with Redshift, or really any data warehouse for that matter. 

 
* Accelerate analytics workloads
* Unified data warehouse & data lake
* Data warehouse modernization
* Analyze global sales data
* Store historical stock trade data
* Analyze ad impressions & clicks
* Aggregate gaming data
* Analyze social trends

### Redshift architecture

![](/img/03/154.png)

Let's start diving deep into the architecture of Redshift itself. So basically we have clusters. That's kind of the highest level here. That encompasses this entire picture here. A cluster is the core infrastructure component of an Amazon Redshift data warehouse. And a cluster is composed of a **```Leader node```**, which you see here, and one or more **`Compute nodes`**. 

You can contain between one and 128 ``compute nodes`` depending on the node type. So it's not infinitely scalable, but 128 nodes can hold a whole lot of data. 

And each cluster can contain one or more databases. 

![](/img/03/159.png)

Now the user data is going to be stored on the ``compute nodes``. 

![](/img/03/160.png)

The `leader node` is just managing communication with the client programs, 

![](/img/03/161.png)

and all communication with the ``compute nodes``. So it's sort of the interface between your external clients to Redshift and the ``compute nodes`` under the hood. 

![](/img/03/162.png)

It receives all the queries from client applications, parses the queries and develops execution plans, which are in order instead of steps to process those queries. It then coordinates the parallel execution of those plans with the `compute nodes`, and also aggregates the intermediate results from those nodes. Finally, the `leader node` will return those results back to the client applications. Let's talk about the `compute nodes` in more depth. 

![](/img/03/163.png)

So `compute nodes` are responsible for executing the steps specified in the execution plans that it's getting from the `leader node`, and transmitting data among themselves to serve those queries. It then sends those intermediate results back to the `leader node` for aggregation, before being sent back to the client applications. Now each compute node has its own dedicated CPU, memory, and attached disk storage, which are determined by the node type you choose. Alright, so within the compute node we have node slices. So every compute node is divided into slices, and a portion of the node's memory and disk space is going to be allocated to each slice, where it processes a portion of the workload assigned to that node. The number of slices per node is determined by the node size of the cluster. Alright, that's a lot of depth there, but really you need to know it. So a cluster consists of a `leader node` and many `compute nodes`, one or many `compute nodes`, and a compute node in turn consists of node slices that process chunks of the data being given to it.


### Redshift Spectrum & Performance

**Redshift Spectrum**

Next, let's dive into Redshift Spectrum. Spectrum allows you to query exabytes. Exabytes, mind you. We're past terabytes here, or petabytes. We're in big data land here for sure. Exabytes of unstructured data in S3 without loading it into your cluster. So in very much the same way that Athena could use the AWS Glue catalog to make tables on top of your S3 data, Redshift Spectrum can do the same thing. It's very much the same idea. It's just that instead of having this console-based query SQL engine in Athena, it actually just looks like another table in your Redshift database. So that way you can have tables that embody your S3 data lake alongside tables that embody data that's actually stored on the Redshift cluster itself, and you can treat them as the same thing and join between them whatever you want to do. So it's really cool. It allows you to run queries against exabytes of unstructured data in S3 without the need to load that data into the Redshift cluster itself or to transform that data in any way. It provides limitless concurrency by allowing multiple queries to access the same data simultaneously in S3. It can scale out to thousands of instances if needed, so your queries will run quickly regardless of data size. And it lets you separate storage and compute capacity, allowing you to scale each independently. So with Spectrum, all of your storage is being done in S3. Spectrum is just doing the compute part of analyzing that data. Redshift Spectrum currently supports many open source data formats including Avro, CSV, Grok, Ion, JSON, ORC, Parquet, RCFile, Regexerta, Sequence Files, Text Files, and TSV. So just about any common open source data format you can imagine, if you have that sitting in an S3 bucket somewhere, Redshift can parse that out and make queries against it. Spectrum also currently supports gzip and snappy compression, so if you do want to compress your S3 data to save space and save bandwidth, you can do that too.  So they've pretty much thought of everything here.

![](/img/03/155.png)

• Query exabytes of unstructured data in S3 without loading
• Limitless concurrency
• Horizontal scaling
• Separate storage & compute resources
• Wide variety of data formats
• Support of Gzip and Snappy compression


**Redshift Performance**

Going back to Redshift as a whole here, how is Redshift so fast? Well again, it uses Massively Parallel Processing or MPP to do that. Data and query loads are automatically distributed across all nodes, and adding nodes to the data warehouse is made easy and also enables fast query performance as the data warehouse grows. So it will do all of its queries in parallel, and if you need more speed or more capacity, just add more nodes to your cluster and it will automatically take advantage of that extra capacity. It also uses columnar data storage, so your data is organized by column, as column-based systems are ideal for data warehousing and analytics for large dataset query, because typically you're just looking at specific columns of a large number of columns. So you can save a lot of bandwidth and a lot of lookup time by organizing your data by columns instead of by rows. The columnar data is stored sequentially on the storage media and requires far less I.O., thereby improving query performance. And by using columnar storage, each data block stores values of a single column from multiple rows. So as records enter the system, Amazon Redshift is transparently converting the data to columnar storage for each of the columns. This helps you to avoid scanning and discarding unwanted rows. Now, columnar databases are generally not well suited for OLTP, online transaction processing. So remember, an anti-pattern for Redshift is OLTP. It's meant for OLAP, like any data warehouse would be. Redshift uses a block size of one megabyte, which is more efficient and further reduces the number of I.O. requests needed to perform any database loading or other operations that are part of a query execution. It can also do column compression. So compared to row-based data stores, columnar data stores can generally be compressed much more, as it's all sequentially stored on disk in the same type of data format. Multiple compression techniques are often applied for better results. So indexes or materialized views are not required with Redshift, and hence it uses less space compared to traditional relational database systems. It will automatically sample the data and select the most appropriate compression scheme when the data is loaded into an empty table. Compression is a column-level operation that reduces the size of the data when it is stored. Compression can conserve storage space and reduces the size of data that is read from the storage. It reduces the amount of disk I.O. and therefore improves your query performance. So when you're loading data into a Redshift cluster, usually you use the copy command to do that most efficiently. That allows you to copy data into your cluster in a distributed, parallelized manner. And when you issue that copy command, it will automatically analyze and apply compression automatically. You don't have to do anything, it just does it for you. But if you want some insight into what's going on there, it offers an analyze compression command, which will perform compression analysis and produce a report with the suggested compression encoding for the tables analyzed.

• Massively Parallel Processing (MPP)
• Columnar Data Storage
• Column Compression


### Redshift Durability and Scaling

**Redshift Durability**

Now let's talk about the specifics of Redshift's durability and scalability. Redshift replicates all of the data within the data warehouse cluster when it is loaded automatically. Also, your data is continuously backed up to S3 for you. Three copies of the data are maintained, on the original, on a replica, on compute nodes, and in a backup in S3. So your data is stored in three different places. There's the original copy within your cluster, there's a backup replica copy within your cluster as well, and furthermore, it does periodic backups into S3 for you. So for disaster recovery, Redshift asynchronously replicates your snapshots to S3 in another region as well. It can enable automated snapshots of the data warehouse cluster with a one-day retention period by default. You can extend that retention period up to 35 days if you want. If you turn the retention period down to zero, however, automated backups will be turned off. Now if you need to restore your cluster from a backup, you just choose one of the automated backups, and then AWS will provision a new data warehouse cluster and restore your data to it. You can then switch over to the newly restored cluster from the old one. Redshift replicates the data within the data warehouse cluster and continuously backs up that data to S3. It will mirror each drive's data to the other nodes within the cluster as well. Redshift will automatically detect a failed drive or node and replace it for you automatically. In the event of a drive failure, the Redshift cluster will remain available with a slight decline in performance of certain queries, while Redshift rebuilds that drive from a replica of the data on that drive which is stored on another drive within that node. Single-node clusters do not support data replication because there's nothing to replicate it to. In that case, you would have to restore your cluster from a snapshot in S3 instead. In the event of an individual node failure, Redshift will automatically detect that node failure and replace the failed node in your data warehouse cluster. Until that replacement node is provisioned and added to the database, the cluster will be unavailable for queries and updates. Most frequently accessed data from S3, however, is loaded first in the new node, so you can resume querying that data as quickly as possible. Single-node clusters do not support data replication, and hence you will again need to restore the cluster from a snapshot in S3 in the case of a node failure on a single-node cluster. AWS recommends at least two nodes in your cluster for production purposes. Now in the event of a Redshift cluster availability zone outage, in this case you will not be able to use your cluster until power and network access to the availability zone are restored because Redshift is currently limited to a single availability zone. However, you can restore the cluster from any existing snapshot to a new availability zone within the same region. So in that case, too, your most frequently accessed data will be restored first from S3 so you can resume your queries as soon as possible. Amazon is working to remove that restriction, however. As of November 2022, they announced multi-availability zone support specifically for RA3 clusters.

• Replication within cluster
• Backup to S3
• Asynchronously replicated to another region
• Automated snapshots
• Failed drives / nodes automatically replaced
• However – limited to a single availability zone (AZ)
• Multi-AZ for RA3 clusters now available


**Scaling Redshift**

How does Redshift scale? Well, Redshift clusters support both vertical, which is increasing the node instance type, and horizontal, which is increasing the number of nodes, scaling. Your requested changes will be applied immediately when you modify your data warehouse cluster. Here's how the process works. So when you're scaling, an existing data warehouse cluster will remain available for read operations and a new data warehouse cluster is created during scaling operations. When that new cluster is ready, the existing cluster will be temporarily unavailable while a CNAME record for the existing cluster is flipped to a point in the new data warehouse cluster. That only takes a few minutes, and it's usually done during some sort of a maintenance window. Redshift will move the data from the compute nodes in the existing data warehouse cluster in parallel to the compute nodes in the new cluster.

• Vertical and horizontal scaling on demand
• During scaling:
• A new cluster is created while your old one remains available for reads
• CNAME is flipped to new cluster (a few minutes of downtime)
• Data moved in parallel to new `compute nodes`


### Redshift Distribution Styles

When we discussed the Redshift architecture, we talked about how the data in your table is distributed across many compute nodes and many slices within those nodes. Now there are several different ways of actually doing that distribution that you need to understand. So when data is loaded into a table, Redshift will distribute the table's rows to the compute nodes and slices according to the distribution style that you chose when you created the table. The two primary goals of data distribution are to distribute the workload uniformly among the nodes in the cluster and to minimize data movement during query execution. There are four different distribution styles, and they are as follows. 

* AUTO
  * Auto-distribution. If you don't specify a distribution style, Amazon Redshift uses auto-distribution. And based on the size of the table data, Redshift will assign an optimal distribution style for you. And that might be even, key, or all. So let's dive into each one of those independently. 
  * Redshift figures it out based on size of data
* EVEN
  * First, let's talk about even distribution. So in even distribution, regardless of the values in any particular column, the leader node distributes the rows across the slices in a round-robin fashion. So it's just going to step through each individual slice and keep assigning new data to each slice in a circular manner. This is appropriate when a table does not participate in joins, or when there is not a clear choice between key distribution or all distribution. So even distribution just tries to spread things out as evenly as possible without thinking about trying to cluster data together that might be accessed at the same time. 
  * Rows distributed across slices in round-robin
  * EVEN distribution: ![](/img/03/156.png)
* KEY
  * Here, the rows are distributed according to the values in one column. So the leader node will place matching values on the same node slice. And matching values from the common columns are physically stored together. So this can come in handy if you're typically going to be doing queries based on a specific column in your data. By using key distribution, you can make sure that all the data associated with a specific key value will be physically located on the same slice. And that can speed up your queries. So the diagram here is showing you that as new rows are coming in from your incoming data, those keys will be hashed and set to a specific slice based on how that key is hashed.
  * Rows distributed based on one column
  * KEY distribution: ![](/img/03/157.png)
* ALL
  * And with all distribution, a copy of the entire table is distributed to every node. That ensures that every row is co-located for every join that the table participates in. The all distribution multiplies the storage required by the number of nodes in the cluster. So it takes much longer to load, update, or insert data into multiple tables. All distribution is really only appropriate for relatively slow-moving tables. That is, tables that are not updated frequently or extensively. As the cost of redistribution is low, small dimension tables do not benefit significantly from all distribution. Now, if you ever need to view the distribution style of the table, you can query the pgClassInfoView or the svvTableInfoView. The ReflectiveDistStyle column in pgClassInfo will indicate the current distribution style for that table.
  * Entire table is copied to every node
  * ALL distribution: ![](/img/03/158.png)




#### Importing / Exporting data

Getting it out of it efficiently, this is the topic. Let's talk about getting data into your Redshift tables and also getting it out of it efficiently. This is a topic that the exam seems to like a lot. So your go-to command for getting data into a Redshift table is the COPY command. It's going to be the most efficient way to load up your Redshift table. You can use the COPY command to read from multiple data files or multiple data streams simultaneously. And that can be done from Amazon S3, from Elastic MapReduce, DynamoDB, or any remote host using the SSH protocol. Now you're going to need role-based or key-based access control to authenticate things there, but once you have all the security hooked up, it's pretty straightforward. If you are importing from S3, it will require a manifest file, which is just a JSON formatted file listing all the data files that you want to load. And also, obviously, an IAM role to give you access to that S3 bucket from Redshift. If you want to get data out of Redshift, the UNLOAD command is the way to do that. So that's going to be just a quick way to unload data from a database table to a set of files in an S3 bucket. Also something to think about is enhanced VPC routing. So that will force all of your COPY and UNLOAD traffic between your cluster and the repositories through your Amazon VPC. If you do not set that up, all of your traffic will be routed through the internet. So you want to make sure your VPC is correctly set up and configured, or the COPY and UNLOAD will fail. You need to make sure your VPC endpoints, your NAT gateway, the internet gateway, are all set up properly to get this all happening within your VPC if you want to avoid routing this data through the internet. So another fine point that the exam might expect you to know. There are some newer features in 2023, so I wouldn't expect to see this on the exam until 2024 and beyond. But if you are watching in 2024 or beyond, this might be important to know. So one is auto-copy from Amazon S3. That just sits there monitoring an S3 bucket, and as new data is seen in that bucket, it will automatically copy that into your Redshift cluster. So that's a way to simplify importing data from S3 even further. You can just sit there looking for new data all the time and automatically copy that into Redshift for you. It's a lot easier. Don't have to sit around and run those copy commands by hand anymore. There's also something called Amazon Aurora Zero ETL integration, and in the same spirit as auto-copy from S3, that sits around and just automatically replicates data from an Aurora database into Redshift all the time. So if you have some Aurora relational database out there and you want to replicate that data into your Redshift tables, that is a way to very easily do that and just keep on doing it automatically. There's also something called Redshift streaming ingestion now, and that will sit around looking for new data on a Kinesis data stream or MSK for your managed Kafka cluster. 

* COPY command
  * Parallelized; efficient
  * From S3, EMR, DynamoDB, remote hosts
  * S3 requires a manifest file and IAM role
* UNLOAD command
  * Unload from a table into files in S3
* Enhanced VPC routing
* Auto-copy from Amazon S3
* Amazon Aurora zero-ETL integration
  * Auto replication from Aurora -> Redshift
* Redshift Streaming Ingestion
  * From Kinesis Data Streams or MSK

#### COPY command: More depth

A little more depth on the copy command, the exam seems to be focusing on this more and more. So remember, you want to use copy to load large amounts of data from outside of Redshift into a Redshift table on your Redshift cluster. If you see a question about how do I efficiently load data into Redshift from outside, odds are the answer is the copy command. Now, if your data is already in Redshift in some other table, you don't want to use copy. Copy is for external data that's being imported into your Redshift cluster. If your data is already in another table within Redshift itself, you want to use either the insert into select statement or create table as to actually create a table that's just a view of the other table that you can refer to. So remember, create table as is where you're going to be using to refer to data that's already in Redshift in some other table, and if it's outside of Redshift, you want to use the copy command. Some neat features of the copy command, it can actually decrypt data as it's loaded from S3. So if your S3 data is encrypted, copy can decrypt it as it is loaded in, and it can do that very quickly because it has a hardware accelerated SSL capability to keep that decryption as fast as possible. It can also speed things up by compressing the data as it's sent across. So it supports gzip, lzop, and vzip2 compression to speed up those data transfers even further when you're using a copy command. Another neat feature of the copy command is automatic compression. This is an option that will analyze the data that's being loaded and automatically figure out the optimal compression scheme for storing it. So if you have data that can compress very well, automatic compression can figure out how do I store this data efficiently. That's used to optimize the usage of your actual storage on your cluster. There's one special case that the documentation calls out, and I wouldn't be surprised if it shows up on the exam. If you have a narrow table, that means that you have a table that has a lot of rows but very few columns, you want to load that with a single copy transaction if at all possible. The thing is, if you have multiple copy transactions, there's a bunch of hidden metadata columns that appear. And these hidden columns, to keep track of where things left off, consume too much space. So if you're going to be loading a table that has many rows but very few columns, try to do that within one copy command. Don't break it up over several copy commands. It's also important to remember that copy is designed to parallelize things and do things as efficiently as possible. So if you can do things in one copy command, why not? It's always going to work well.

* Use COPY to load large amounts of data from outside of Redshift
* If your data is already in Redshift in another table,
  * Use INSERT INTO …SELECT
  * Or CREATE TABLE AS
* COPY can decrypt data as it is loaded from S3
  * Hardware-accelerated SSL used to keep it fast
* Gzip, lzop, and bzip2 compression supported to speed it up further
* Automatic compression option
  * Analyzes data being loaded and figures out optimal compression scheme for storing it
* Special case: narrow tables (lots of rows, few columns)
  * Load with a single COPY transaction if possible
  * Otherwise hidden metadata columns consume too much space


#### Redshift copy grants for cross-region snapshot copies

It's actually pretty amazing the level of depth the exam expects from you on this stuff. So let's talk about a very specific case here. Let's say that you want to copy a snapshot from Redshift automatically to another region, so you have cross-region replication of your snapshots for backup purposes. So let's say you have a KMS-encrypted Redshift cluster, and you have a snapshot of that cluster being saved in some S3 region. Now you want to copy that snapshot to another region for even better backup. The way you would do that is as follows. In your destination AWS region, you create a KMS key if you don't have one already, and then you set up a snapshot copy grant, specifying a unique name for that snapshot copy grant in the destination region. As part of setting up that copy grant, you specify the KMS key ID that you're creating it for. Then back in the source AWS region, you will enable copying of snapshots to the copy grant that you just created. So by using a copy grant that you set up using a KMS key, you can securely copy KMS-encrypted snapshots for your Redshift cluster across to another region. Another bit of connectivity trivia is dblink. And dblink is an extension that allows you to connect your Redshift cluster to a PostgresQL instance, which might be hosted using Amazon's RDS service. At a high level, you might want to do that to get the best of both worlds between the columnar storage of Redshift and the row-based storage of Postgres. Or you might be using it as a way to copy and sync data between a PostgresQL instance and Redshift. So if you want a very efficient way of copying data and keeping data synchronized between Postgres and Redshift, the dblink extension is a way to do that. The way it works is like this. Basically, you launch a Redshift cluster and you launch your PostgresQL instance as well in the same availability zone, mind you. You would then configure the VPC security group for the Amazon Redshift cluster to allow an incoming connection from the RDS PostgresQL endpoint. You would then connect to the RDS PostgresQL instance and run the SQL code you see here to establish that dblink connection between the PostgresQL instance and Amazon Redshift. So just remember dblink exists to connect Redshift to PostgresQL and get the best of both worlds, or maybe to copy and synchronize data between the two more efficiently.

• Let’s say you have a KMS-encrypted Redshift cluster and a
snapshot of it
• You want to copy that snapshot to another region for backup
• In the destination AWS region:
• Create a KMS key if you don’t have one already
• Specify a unique name for your snapshot copy grant
• Specify the KMS key ID for which you’re creating the copy grant
• In the source AWS region:
• Enable copying of snapshots to the copy grant you just created



#### DBLINK

Another bit of connectivity trivia is dblink. And dblink is an extension that allows you to connect your Redshift cluster to a PostgresQL instance, which might be hosted using Amazon's RDS service. At a high level, you might want to do that to get the best of both worlds between the columnar storage of Redshift and the row-based storage of Postgres. Or you might be using it as a way to copy and sync data between a PostgresQL instance and Redshift. So if you want a very efficient way of copying data and keeping data synchronized between Postgres and Redshift, the dblink extension is a way to do that. The way it works is like this. Basically, you launch a Redshift cluster and you launch your PostgresQL instance as well in the same availability zone, mind you. You would then configure the VPC security group for the Amazon Redshift cluster to allow an incoming connection from the RDS PostgresQL endpoint. You would then connect to the RDS PostgresQL instance and run the SQL code you see here to establish that dblink connection between the PostgresQL instance and Amazon Redshift. So just remember dblink exists to connect Redshift to PostgresQL and get the best of both worlds, or maybe to copy and synchronize data between the two more efficiently.

* Connect Redshift to PostgreSQL (possibly in RDS)
* Good way to copy and sync data between PostgreSQL and Redshift PostgreSQL instance

```sql
CREATE EXTENSION postgres_fdw;
CREATE EXTENSION dblink;
CREATE SERVER foreign_server
  FOREIGN DATA WRAPPER postgres_fdw
  OPTIONS (host '<amazon_redshift _ip>', port '<port>', dbname '<database_name>', sslmode
  'require');
CREATE USER MAPPING FOR <rds_postgresql_username>
  SERVER foreign_server
  OPTIONS (user '<amazon_redshift_username>', password '<password>');
```


#### Integration with other services

How does Redshift integrate with other AWS services? Well, let's go through a few of them. So Amazon S3. You can use parallel processing to export your data from Amazon Redshift data to multiple data files on S3. And of course, you can also import data from S3 or even sit on top of it using Amazon Redshift. Amazon DynamoDB. So by using the copy command, you can also load a Redshift table with data from a single Amazon DynamoDB table. So it's possible to import data from DynamoDB into Redshift using the copy command as well. On EMR hosts or EC2 instances, you can import data using SSH. So again, via the copy command, you can load data from one or more remote hosts, such as EMR clusters or whatever you're running on EC2 for that matter. It also integrates with the AWS Data Pipeline. So you can automate the data movement and transformation in and out of Redshift tables using Data Pipeline. And finally, the AWS Database Migration Service, or DMS, can migrate your data into Amazon Redshift for you, or at least help you with the process. So the Database Migration Service is a whole big topic of its own. Basically, it's a set of tools that allow you to migrate data from some existing data warehouse into Amazon Redshift. And often there's a lot more to that than you might imagine. 

![](/img/03/164.png)

**Redshift Workload Management (WLM)**

You will also need to at least know what Redshift Workload Management is, WLM for short. It's a way to help users prioritize workloads so that short, fast-running queries are not stuck behind long-running, slow queries. The way it works is by creating query queues at runtime according to service classes. And configuration parameters for various types of queues are defined by those service classes. Now you can modify the WLM configuration to create separate queues for long-running queries and for short-running queries, thereby improving system performance and user experience. You can set all this up using the Amazon Redshift Management Console, the Amazon Redshift Command Line Interface, or the Amazon Redshift API.

* Prioritize short, fast queries vs. long, slow queries
* Query queues
* Via console, CLI, or API

#### Concurrency Scaling

The topic of scaling and tuning your Redshift cluster comes up a lot in the exam as well. One feature you need to know about is called Concurrency Scaling. This feature allows you to automatically add cluster capacity to handle sudden increases in concurrent read queries. So if you have very bursty access to your Redshift cluster where you have these sudden floods of read queries that come in from outside, Concurrency Scaling can automatically scale up your cluster to handle it. It can support virtually unlimited concurrent users and queries this way. It just keeps on adding more and more capacity as needed as your read queries increase in volume. And you can use workload management queues, which we'll talk about again very shortly, to manage which queries are sent to the Concurrency Scaling cluster. So you can manage through what queue you assign your queries to, which ones can actually take advantage of Concurrency Scaling and which ones do not. This can allow you to, for example, segregate those read queries that you think might be bursty in nature or vary in time and their frequency, and use that to automatically scale out the capacity for that specific query. It kind of reduces the risk of inadvertently adding a bunch of capacity for some offline job that doesn't really need to run fast, necessarily. So you can pick and choose which queries take advantage of Concurrency Scaling. Obviously, it's not free, so you want to give some thought as to which queries can actually have this capability of just automatically adding more and more capacity as needed.

• Automatically adds cluster capacity to handle increase in concurrent read queries
• Support virtually unlimited concurrent users & queries
• WLM queues manage which queries are sent to the concurrency scaling cluster


#### Automatic Workload Management

Workload Management, WLM, comes in a couple of different flavors. One is called Automatic Workload Management. So with Automatic Workload Management, you can define up to eight different queues that are managed for you. And by default, you have five queues that have an even memory allocation between them, but you can change that, obviously, if you need to. So the idea is that if you have a bunch of large queries going into a queue, like big hash joins or something like that, the concurrency will be lowered on that queue automatically for you. And if you have a bunch of small queries going into a queue, like inserts or scans or simple aggregations, the concurrency level on that queue might be raised. Concurrency is just, how many queries can I run at once, right? So larger queries, obviously, need more capacity. You want to perhaps lower concurrency on those so that they get more resources, whereas small queries can get away with fewer resources and you can run more of them at once. By separating these queries into their own queues, we can take better advantage of the hardware that we have. So each automatic workload query queue can be configured in a bunch of different ways. One thing you can do is set a priority value. That just defines the relative importance of queries within a workload. You can also set concurrency scaling mode. So basically, that's what we talked about before with concurrency scaling. This is where you can say, I want this particular queue to have access to a concurrency scaling cluster and have the ability to automatically add more and more resources, more and more servers under the hood to handle the capacity that queue needs. That costs money, obviously, so you want to think carefully about whether or not that's enabled. You can also assign a set of user groups to a queue, and you can do that by specifying a user group name or by using wildcards. So when a member of a listed user group runs a query, that query will automatically run within the corresponding queue. So you can assign queues to query queues based on users. You can also set up a query group. All that is is a label. So basically, at runtime, you can assign a query group label to a series of queries, and that will define which queue that query goes into. So basically, a tag, a label, that's assigned to the query itself can define which queue it goes to. You can also set up query monitoring rules. These are pretty cool. They allow you to define metrics-based performance boundaries for workload management queues, and you can specify what action to take when a query goes beyond those boundaries. So for example, you might have a queue dedicated to short-running queries, and you might have a query monitoring rule that aborts those queries if they run for more than 60 seconds. So that way, you can enforce that your short-query queue is actually handling short queries, and if something goes wrong with one of those queries, it's not gonna hold up all the other queries in that queue. So a very useful tool there. You can also do things like kick queries off to a different queue if it violates some query monitoring rule. 
* Creates up to 8 queues
* Default 5 queues with even memory allocation
* Large queries (ie big hash joins) -> concurrency lowered
* Small queries (ie inserts, scans, aggregations) -> concurrency raised
* Configuring query queues
  * Priority
  * Concurrency scaling mode
  * User groups
  * Query groups
  * Query monitoring rules


#### Manual Workload Management

The other flavor of WLM is manual workload management, and by default, this comes with one queue with a concurrency level of five. Again, concurrency level is how many queries can I run at once within this queue. In addition, there's a super user queue with a concurrency level of one. This is a queue that's intended for administrative queries that are happening that always must run no matter what. You can define up to eight manual queues, and you can have a concurrency level on a queue up to 50. Each of those queues allows you to define whether or not the concurrency scaling cluster is available to it, where they can automatically add more capacity to that queue. You can also set a manual concurrency level to that queue, defining how many queries I wanna be able to run at once within it. You can assign user groups to it and query groups like we talked about before with automatic queues, just allowing you to automatically route queries to a queue based on what user is running it or what query label has been attached to the query. You can also define the memory allocated to a given queue, the timeout value for running a query within that queue, and again, any query monitoring rules that you might have. You can also enable what's called query queue hopping. If you have a query that times out within a given queue, you can configure things to have it hop to the next queue and try it again on a different queue that might have a higher timeout or more resources available to it. 

* One default queue with concurrency level of 5 (5 queries at once)
* Superuser queue with concurrency level 1
* Define up to 8 queues, up to concurrency level 50
  * Each can have defined concurrency scaling mode, concurrency level, user groups, query groups, memory, timeout, query monitoring rules
  * Can also enable query queue hopping
    * Timed out queries “hop” to next queue to try again

#### Short Query Acceleration (SQA)

One more thing to talk about here is short query acceleration or SQA. The idea here is to automatically prioritize short-running queries over longer-running queries. The short queries will run in their own dedicated space, so they don't end up waiting in a queue behind longer queries. So this can be used in place of workload management queues if you just want to accelerate short queries. It can work with create table as statements, remember that, and also read-only queries or select statements. So these are both candidates for short query acceleration. It can automatically run those in their own space so that they don't get stuck behind longer analytic queries. And it works by using machine learning, pretty cool. So it actually tries to predict a query's execution time using machine learning algorithms automatically. So it can kind of guess based on the query itself how long it might take and figure out whether or not that should go into the dedicated space for short queries or not. And you can configure how many seconds you consider to be short. So that's one dial that you have on short query acceleration. So remember, short query acceleration is an alternative to WLM workload management if all you want to do is accelerate short queries and make sure that they don't get stuck behind longer queries. And it can work with create table as statements or read-only select statements. 

* Prioritize short-running queries over longer-running ones
* Short queries run in a dedicated space, won’t wait in queue behind long queries
* Can be used in place of WLM queues for short queries
* Works with:
  * CREATE TABLE AS (CTAS)
  * Read-only queries (SELECT statements)
* Uses machine learning to predict a query’s execution time
* Can configure how many seconds is “short”


#### Resizing Redshift Clusters



#### VACUUM command

Finally, you need to know what the vacuum command does with Redshift. Vacuum is a command used to recover space from deleted rows and to restore the sort order. So basically, it cleans up your table. There are four different types of vacuum commands. The first one is vacuum full. This is the default vacuum operation. It will resort all the rows and reclaim space from deleted rows. There's also vacuum delete only, which is the same as a full vacuum, except that it skips the sorting part. So it's just reclaiming deleted row space and not actually trying to resort it. You can also do a vacuum sort only, which will resort the table but not reclaim this space. And finally, there's vacuum reindex. That's used for reinitializing interleaved indexes. Remember, we talked about the different sort keys you can have, one of them being interleaved. So reindex will reanalyze the distribution of the values in the table sort key columns and then perform a full vacuum operation after that. 

* Recovers space from deleted rows and restores sort order
* VACUUM FULL
* VACUUM DELETE ONLY
  * Skips the sort
* VACUUM SORT ONLY
  * Does not reclaim space!
* VACUUM REINDEX
  * Re-analyzes distribution of sort key columns
  * Then does a full VACUUM


#### Redshift anti-patterns

And lastly, let's talk about the anti-patterns for Redshift. This also comes straight out of the AWS Big Data white paper of things they do not want you to use Redshift for. One is for small data sets. So remember, Redshift's strength is that it is massive and highly scalable. If you just have a small, tiny table that you want to store, RDS might be a better choice for that. OLTP, again, they cannot stress enough that Redshift is made for analytic queries, for OLAP. If you need to do transactional, very fast queries, you want to be using RDS or DynamoDB instead. And they're also listing unstructured data for me, which is kind of strange because Redshift's spectrum kind of exists to let you query unstructured data in S3. But if you do need to do some ETL, you should do that first with EMR or Glue ETL or something like that first. And it is not appropriate for storing blob data, that being large binary files. If you do need to store large binary files in your data warehouse, it's best to just store references to those files where they reside in S3 and not the files themselves. So that's a lot about Redshift. But again, you do need a lot of depth for Redshift for the exam. It will be well worth your while to understand and remember everything in these slides.

* Small data sets
  * Use RDS instead
* OLTP
  * Use RDS or DynamoDB instead
* Unstructured data
  * ETL first with EMR etc.
* BLOB data
  * Store references to large binary files in S3, not the files themselves.


### Resizing Redshift Clusters

So, what if concurrency scaling isn't enough, and you need to actually resize your cluster on the fly? You need to add more capacity to it. Well, there's a few ways of doing that, and you need to understand the difference between them. So, one's called elastic resize. You can use this to quickly add or remove nodes of the same type. So if you're happy with the type of nodes, the type of EC2 instances that are running your Redshift cluster, you can add more or remove them using elastic resize. With elastic resize, your cluster will only be down for a few minutes, while it adds that capacity. And it tries to actually keep those connections open across the downtime as well, so you might not even drop any queries. It might not just keep that connection held open and let it resume once that added capacity is in place on your cluster. Now, one limit of elastic resize is that for certain DC2 and RA3 node types, you're limited to either doubling or halving the size of that cluster. You don't really need to remember what the specific types are for the exam, but for certain types, that is what you need to do. You can only double or halve them. If you need to do something a little bit more involved, then you need to go back to what's called classic resize, and that allows you to actually change the node types and or the number of nodes. The thing with classic resize is that if you're actually changing the node types, it could take hours or even days for your cluster to become writable again. So your cluster will be in a read-only state for the entire period of time that it takes to provision that new hardware and swap it out in your cluster. That can be a very lengthy experience. So you want to use elastic resize when you can, but if you need to change your node type, you have to use classic resize. That's the main difference there. One technique for dealing with things in that classic resize scenario is to use what's called snapshot restore resize, and this is a strategy for keeping your cluster available during a classic resize operation. So if you do need to change your node type, or you need to add an amount of capacity that an elastic resize won't allow you to do, what you can do is use a snapshot command to make a copy of your cluster, and then resize that new cluster. So your data is still going to the old cluster while your new cluster is being resized and going through the classic resize process. Once your new cluster is finally done, you can then shift your traffic to that new cluster that's been created. So snapshot, restore, then resize is a way of migrating from one cluster to another using classic resize to minimize downtime.

* Elastic resize
  * Quickly add or remove nodes of same type
    * (It *can* change node types, but not without dropping connections – it creates a whole new cluster)
  * Cluster is down for a few minutes
  * Tries to keep connections open across the downtime
  * Limited to doubling or halving for some dc2 and ra3 node types.
* Classic resize
  * Change node type and/or number of nodes
  * Cluster is read-only for hours to days
* Snapshot, restore, resize
  * Used to keep cluster available during a classic resize
  * Copy cluster, resize new cluster


### RA3 Nodes, Cross-Region Data Sharing, Redshift ML

#### Newer Redshift features

Let's discuss some of the newer features in Redshift that have come out in the more recent years. In 2020, I think it was, or end of 2019, RA3 nodes were introduced. This is a special node type that's optimized for Redshift, so that was kind of a big deal, and they've been building more and more on top of that over the years. The big idea here, though, is that it comes with managed storage, so they have decoupled compute and storage capabilities with the RA3 node. Basically, their observation was that storage capabilities and computing capabilities were evolving at different rates, so by having the ability to independently scale computing and storage capacity, that lets you make better use of the resources that you have for your Redshift cluster, so that's kind of a big deal. Another thing that came out in 2020, I think, yeah, 2020, was Redshift Data Lake Export, and the idea there is that you can dump a Redshift query right into S3 in Apache Parquet format, so if you have a big query on a big data warehouse in Redshift, you can dump that out to a Data Lake just sitting in S3 pretty easily using Data Lake Export. The motivation of doing that is that the Parquet format is twice as fast to unload, and it also consumes up to 6x less storage, so it's a very fast and compact way of exporting data to a Data Lake, and that Data Lake that you end up with will be compatible with Redshift Spectrum, Athena, EMR, or SageMaker, anything that can read from S3. It's also automatically partitioned for you, so you don't necessarily have to think about that. Also, more recently, in 2022 or into 2021, they've introduced spatial data types, so now there are geometry and geography data types available in Redshift, so if you do need to do things with mapping, those sorts of applications, you can do that in Redshift now with those new data types. And also a new feature is cross-region data sharing, new for 2022. It's sort of been evolving over time, but it's really evolving more lately. The idea here is that you can share live data across Redshift clusters without having to copy them, so we talk a lot about replicating data across different regions and different entities and how much of a pain that can be, so with cross-region data sharing, they've really strove to simplify that as much as possible. So that allows you, again, to share live data across regions, that's a new thing, and even across accounts, if you want to, in a secure manner, very easily. To do that, you do need to use that new RA3 node type that we talked about. It's only supported on our RA3 node types, so that's a big deal, cross-region data sharing, new easy way of managing that problem. 

* RA3 nodes with managed storage
  * Enable independent scaling of compute and storage
  * SSD-based
* Redshift data lake export
  * Unload Redshift query to S3 in Apache Parquet format
  * Parquet is 2x faster to unload and consumes up to 6X less storage
  * Compatible with Redshift Spectrum, Athena, EMR, SageMaker
  * Automatically partitioned
* Spatial data types
  * GEOMETRY, GEOGRAPHY
* Cross-Region Data Sharing
  * Share live data across Redshift clusters without copying
  * Requires new RA3 node type
  * Secure, across regions and across accounts



#### Amazon Redshift ML

Another more recent feature is Amazon Redshift ML, or machine learning, and I'm not going to go into a ton of depth on this, because generally speaking, machine learning topics are going to be in the machine learning exam, not the data analytics exam, but still, you should at least know this exists, it might get referenced in the exam. This was introduced in mid-2021, so it is fair game right now. Conceptually, there's this high-level diagram that Amazon gives us that says you start by collecting your data into a data warehouse, which is probably S3, and that Redshift ML encompasses this whole system here, where you're creating a machine learning model just by using a SQL command, using create model, automatically that will kick off to Amazon SageMaker to automatically tune and train your model. Specifically, it's using SageMaker Autopilot, but I'll walk through that in a second here. It will then deploy your model, so you can just hit it with SQL commands in Redshift to get predictions in real time, using SQL commands. The way this would go down in practice is that you would start off by having Redshift export your training data for your machine learning model into Amazon S3. SageMaker Autopilot is the specific technology that's going to sit around there, it's under the train section of this diagram, and it will pre-process your data and try to find the ideal hyperparameters, the best model to use and the best parameters for that model. Once your model has been trained, it's then able to make predictions, and Redshift just registers your prediction function as a SQL function in your Redshift cluster. You can just go ahead and hit your Redshift database to get predictions based on the model that you trained and deployed using Redshift machine learning. Now, when you do create model, that kicks off a lot of stuff under the hood, right? Building and training a machine learning model can be computationally intensive, and that does cost money. You will see a separate line item in Amazon SageMaker in your AWS bill if you're using Redshift ML. You'll also pay for any underlying storage used in S3 for storing the training data and for the data that has to go back and forth between Redshift and SageMaker as it's going through this whole process. So that's Redshift ML in a nutshell. Again, not something you probably have to worry about too much for the exam, but for the sake of completeness, it's an important feature in Redshift.

![](/img/03/165.png)



### Redshift security

So a couple of specific security concerns I want to talk about with Redshift. We'll talk about security a lot more later in the course, but here are a few things that might show up on the exam. One is the issue of using a hardware security module, or an HSM, with Redshift. And if you're using an HSM with Redshift, you need to use a client and a server certificate to configure a trusted connection between Redshift and the HSM. Now if you're trying to do this after the fact, you're trying to add an HSM to an existing Redshift cluster, you're going to have to copy it over. So if you want to migrate an unencrypted cluster to an HSM encrypted cluster, you're going to have to create the new encrypted cluster first, and then move your data over to that encrypted cluster. So that's kind of the nitty-gritty of using an HSM with Redshift. You need a client and server certificate to do it. Also, there's a cool thing where you can use grant or revoke commands in SQL to actually define access privileges for individual users or for groups of users in Redshift. So an easy way to manage permissions in Redshift is to say something like, grant select on table foo to Bob. That would grant the permission to Bob to do select statements on the table named foo. And you can do things like grant all or grant drop, let people drop data as well. This can all be managed individually to a user or group just using the grant command just like any other SQL command or the revoke command if you want to take those permissions away. So that's the fundamental way that you manage security and access permissions in Redshift.

* Using a Hardware Security Module (HSM)
  * Must use a client and server certificate to configure a trusted connection between Redshift and the HSM
  * If migrating an unencrypted cluster to an HSM-encrypted cluster, you must create the new encrypted cluster and then move data to it.
* Defining access privileges for user or group
  * Use the GRANT or REVOKE commands in SQL
  * Example: grant select on table foo to bob;


### Redshift Serverless

So, a pretty recent development, just like EMR is going serverless, so is Redshift. Redshift serverless is now available as well, and this promises to automatically scale and provision your Redshift cluster for your workload. So, again, you don't have to think about how much capacity you need, it will figure it out for you. The idea is to optimize your costs and performance, so since you're only paying for the capacity that you're actually using, that should save you some money. And it will also optimize your performance, because if you are going to run a very demanding query of some sort, it can automatically scale that up for you without you having to think about it, which makes life a lot easier. Under the hood, it's using some sort of fancy machine learning, I won't go into details about it, but ML to maintain performance across what might be a variable and sporadic workload, so even if you're hitting it with ad hoc queries, it will learn how to deal with that. The main purpose of this, though, is to make it really easy to spin up a Redshift cluster, right, so I don't have to think about capacity anymore, I can just go ahead and spin up Redshift and it will automatically figure out the capacity that I need and that's all I get billed for, which is great. So, if I want to spin up a little development environment or a test environment, I can do that in one little page here, just go ahead and set up my new database and it will automatically just give me the capacity that I need and no more, which is a lot easier. If I want to do just a quick ad hoc business analysis thing, spin up a quick cluster, answer a question, get out, can do that too, don't have to think about it too much, they just made it a lot easier to set up your cluster, so that makes it easier for other people to use it and encourages you to use it for more types of applications. Once you have spun it up, you just get back a serverless endpoint of some sort and you also get a URL for connecting to it through JDBC or ODBC, or if you're just doing a quick ad hoc thing, you can just use the query editor in the console as well, like we've seen before, you don't have to connect to it externally if you don't want to. 

* Automatic scalling and provisioning for you workload
* Optimizes costs & performance
  * Pay only when in use
* Uses ML to maintain performance across variable & sporadic workloads
* Easy spinup of development and test environments
* Easy ad-hoc business analysis
* You get back a serverle endpoint, JDBC/ODBC connection, or just query via the console´s query editor

#### Redshift Serverless: Getting Started

So getting started, the only hard part right now is actually setting up the IAM role needed to set up Redshift serverless, so you need to start off by creating an IAM role by hand that has this policy attached to it to the right here. You need to make sure that you have access to the Redshift serverless action by hand, and I'm sure they'll make that easier eventually, but for now, you do need to set that IAM role up yourself. Once you have that though, when you're setting up your new Redshift serverless cluster, all you need to do is tell it what your database name is, the user credentials for your admin user, which VPC you want to be running within, any custom encryption settings you might want to use, by default it's just going to use an AWS-owned KMS key, which is usually fine, and any special settings that you might want for audit logging, but that's it. You just fill out one page of that information and you have a Redshift serverless cluster waiting for you to use it, which is kind of awesome. After the fact, you can add snapshots and recovery points later on after you've created it. Those are all still supported as well, so you can have periodic snapshots of your data and the ability to roll stuff back from the previous day or whatever using recovery points later on. That's all still there. Let's talk more about how resource scaling actually works in Redshift serverless. 

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": "redshift-serverless:*",
      "Resource": "*"
    }
  ]
}
```

* Need an IAM role with this policy
* Define your
* Database name
* Admin user credentials
* VPC
* Encryption settings
* AWS-owned KMS by default
* Audit logging
* Can manage snapshots & recovery points after creation

#### Resource Scaling in Redshift Serverless

So because we're shifting our thinking to serverless, they can't really bill us in terms of servers anymore. They're not really exposing what those servers are. So instead, they're going to have us think about what they call Redshift Processing Units, or RPUs, and that's their new billing metric. So you pay for RPU hours down to the per second level, plus any storage that you're using. So the billing in Redshift serverless is based on how many Redshift Processing Units you're using per hour, plus any associated storage fees. So that's charged per gigabyte over time units. You can specify your base RPUs. So kind of like you can specify initial capacity with EMR serverless, same idea with Redshift serverless. You can adjust your base capacity. Now by default, it will be set to automatic, so you don't have to think about it at all. But if you do want to improve your query performance and you know that you're going to need a little bit more capacity for that out of the gate, you can give it a hint. You can say that I want between 32 and 512 RPUs as my base level in my serverless cluster to start with. I can also set my maximum RPUs. So if I want to make sure that I never go beyond a certain resource consumption to control my costs, you can do that too. Of course, they want you to use it to increase your throughput. So although in practice, you probably want to use max RPUs to ensure that you're not billed for more than you expect, you can also use it to improve the throughput of the job if you think the maximum RPUs is set too low.

* Capacity measured in Redshift
Processing Units (RPU’s)
* You pay for RPU-hours (per second) plus storage
* Base RPU’s
  * You can adjust base capacity
  * Defaults to AUTO
  * But you can adjust from 32-512 RPU’s to improve query performance
* Max RPU’s
  * Can set a usage limit to control costs
  * Or, increase it to improve throughput


**Redshift Serverless**

A few more things about Redshift serverless. So basically, it does everything that Redshift can, with a few exceptions. Partner groups are not supported with Redshift serverless. Workload management also doesn't work with it. It wouldn't make much sense if it did, right? The whole point is that you don't have to think about that too much. AWS partner integration, currently not supported with Redshift serverless. And also, maintenance windows and version tracks are not supported with Redshift serverless. That's a little bit... That one's worth talking about a little bit. With normal Redshift, you have these maintenance windows, so you kind of know when updates are going to be applied to your cluster and you can plan accordingly. With Redshift serverless, if a software update is being rolled out to Redshift itself, you will get your connections dropped without any warning. So there's no real advanced planning for that with Redshift serverless. That might be something to think about when you're deciding whether or not to use this for now. Also, there are no public endpoints with Redshift serverless. You must access it within a VPC. It will not make that accessible publicly yet. That might change.

* Does everything Redshift can, except:
  * Parameter Groups
  * Workload Management
  * AWS Partner integration
  * Maintenance windows / version tracks
  * No public endpoints (yet)
  * Must access within a VPC

**Redshift Serverless: Monitoring**

Monitoring is something they talk about in the documentation for Redshift serverless. That did also change a bit. So there are a bunch of different views that it provides for monitoring purposes, like sysquery history, sysload history, sysserverless usage, and a few more. They do what you think they do. So basically, it's a history of the queries. It's also a query detail view if you want more information, historical information about the load on your serverless cluster, the usage of that cluster. This can all be useful in troubleshooting performance issues. Maybe your maximum RPUs is set too low, for example, and that might be a way to figure that out. Also, it publishes data to CloudWatch. Connection and user logs are enabled by default. There's also optional user activity log data if you want to publish that as well. That will be published under the AWS Redshift serverless CloudWatch topic. Metrics that are published in CloudWatch include queries completed per second, query duration, queries running, stuff like that, and you can break that down by various dimensions such as the database name, the latency, which is split up into short, medium, or long, the query type, and the stage of the query, the stage that the query is in as it completes. So that's all available to you as well. But in a nutshell, Redshift serverless pretty much works the same way as Redshift. The only difference is that you're not thinking about the underlying capacity, the underlying servers. It's doing that for you. So instead of being built by server usage explicitly, you're being built by RPUs. And you have a few ways of controlling that if you need to, including the base and maximum RPU capacity for Redshift serverless.


* Monitoring views
  * SYS_QUERY_HISTORY
  * SYS_LOAD_HISTORY
  * SYS_SERVERLESS_USAGE
  * …and many more
* CloudWatch logs
  * Connection & user logs enabled by default
  * Optional user activity log data
  * Under /aws/redshift/serverless/
* CloudWatch metrics
  * QueriesCompletedPerSecond, QueryDuration, QueriesRunning,etc.
  * Dimensions: DatabaseName, latency (short/medium/long), QueryType, stage

### Redshift Materialized Views

You need to know about materialized views for the exam as well. Let's talk about what those are in Redshift. So you know what a view is in a database, it's just basically taking the results of a query and making that look like another table you can query subsequently. A materialized view is a little bit different. So it's actually pre-computing the results of that query and storing the results of that query and making that look like another table. So it's different from a normal view, if you will, in that it's actually storing the results of the query and not just the query itself. So as you can see, this might be a pretty important performance optimization, right? So if you have a complex query in your data warehouse on a large table, you can query that materialized view just like any other table or view, based on those saved results from a more complex query. So if you need to build a query on top of some more complicated query, you can store the results of that complicated query into a materialized view and then query those results repeatedly from other queries. So since you're using those pre-computed results and not accessing the base tables, that can be a very important performance optimization. Of course, it comes at the cost of having a synchronization problem, right? So if the results of your materialized view change because the underlying stuff that you're querying changes, that materialized view needs to be explicitly refreshed somehow. So an example of where you might want to use this is if you have a predictable and recurring query, like populating a dashboard from Amazon QuickSight or something like that. So if you know your dashboard is going to need the results of a given set of queries, maybe you can pre-compute those results using materialized views and then build your dashboard against that materialized view to make it more performing instead of running that query every time you want to look at your dashboard. 

![](/img/03/166.png)

* Contain precomputed results based on SQL queries over one or more base tables.
  * This differs from a “normal” view in that it actually stores the results of the query
* Provide a way to speed up complex queries in a data warehouse environment, especially on large tables.
* You can query materialized views just like any other tables or views.
* Queries return results faster since they use precomputed results without accessing base tables.
* They're particularity beneficial for predictable and recurring queries e.g. population dashboards like Amazon QuickSight

#### Using Materialized Views

How do I use them? They're pretty easy. So instead of create view, you just say create materialized view and that's it. So instead of a view that's based on the query, you're creating a snapshot of the results of that query and making a view out of it. How do I keep them refreshed? So like I said, there's a synchronization problem because I'm storing the results and not just the query itself. There might be a synchronization issue where the underlying data that I'm querying that materialized view from changes. So to keep the materialized view in sync with those changes, I can manually and explicitly refresh that when I need to by using the refresh materialized view command. You can also set the auto refresh option when you're creating it to just make that happen automatically. So it depends how much control you want over it. If you want to have control over how often it's being refreshed to maintain performance, maybe you only need to refresh it once a day because you're going to be using this for a dashboard that's queried once a day. Then maybe just refresh that materialized view once a day as opposed to automatically refreshing it every time something changes. Once you have a materialized view, you can query it just like any other table or view. It looks just like anything else. And you can also stack them on top of each other. So you can have materialized views that are built from other materialized views, which is pretty cool. An example of where that might be useful is if you have some expensive join operation among multiple tables, you could store the results of that join as a materialized view and then have other materialized views that have specific queries using that joined data. So you can stack them on top of each other as well, which can also be used to accelerate your query performance.

```sql
CREATE MATERIALIZED VIEW tickets_mv AS
    select catgroup,
    sum(qtysold) as sold
    from category c, event e, sales s
    where c.catid = e.catid
    and e.eventid = s.eventid
    group by catgroup;
```

* CREATE MATERIALIZED VIEW…
* Keeping them refreshed
  * REFRESH MATERIALIZED VIEW…
  * Set AUTO REFRESH option on creation
* Query them just like any other table or view
* Materialized views can be built from other materialized views
  * Useful for re-using expensive joins

### Redshift Data Sharing / Data Shares

A cool feature of Redshift is data sharing. If you want to share data amongst Redshift clusters, that allows you to securely share live data across Redshift clusters for read purposes only. So if you want to publish your data to other Redshift clusters and let other people read your data, this is a very good way to do it. Why would you want to do that? Well, one is workload isolation. So if you're sharing your data to another cluster, if that other cluster gets bogged down, it's not going to affect the performance of the producer, the main cluster that you're sharing that data from. So if you want to make sure that some rogue department in your organization can't bring down the performance of your Redshift cluster, maybe you just share your data to their Redshift cluster. And if they have performance issues, well, it won't affect you. Cross-group collaboration like that is one use case of doing this. So if you do have another department or another organization that needs access to your data in a read manner, that's one way of sharing that data with them across groups within an organization. And also, it can be used for not just sharing data across groups, but also between environments. So a good application of data sharing is sharing data between development, test, and production environments. This would allow me to develop changes to an application or whatnot against the same data that's in production, but again, having that isolation so that my development traffic and what I'm doing in development can't affect what's going on in production. This assumes that what you're doing can be done with read-only data, of course. You might also want to do this for licensing access to your data through AWS Data Exchange. So if you want to publish your data and basically sell it to other AWS users, data sharing is how you would actually do that with AWS Data Exchange. What can you share? You can share entire databases. You can share schemas. You can share tables. You can share specific views and or user-defined functions. So you have fine-grained access control over who can see what, and it could be any or all of those things.

![](/img/03/167.png)

* Securely share live data across Redshift clusters for read purposes
* Why?
  * Workload isolation
  * Cross-group collaboration
  * Sharing data between dev/test/prod
  * Licensing data access in AWS Data Exchange
* Can share DB’s, schemas, tables, views, and/or UDFs.
  * Fine-grained access control

 I touched on it earlier, but data sharing relies on a producer-consumer architecture where all of that fine-grained security is being controlled by the producer, the person publishing that data. Again, we have isolation so that the performance of the producer cluster is unaffected by any consumers of that data using data sharing. However, data will be live and transactionally consistent, so those consumers are going to get a live view and a transactionally consistent view of what's happening in the producer cluster. In order for data sharing to work, both clusters must be encrypted and they must be using RA3 node types. If you are doing data sharing across regions, that may involve transfer charges, so keep an eye on your bill in that case. There are three different kinds of data shares. There's standard, which we've been talking about, just one cluster to another. Also, AWS Data Exchange, if you want to share data to the data exchange. And the third type of data share is managed by AWS League Formation.

* Producer / consumer architecture
  * Producer controls security
  * Isolation to ensure producer performance unaffected by consumers
  * Data is live and transactionally consistent
* Both must be encrypted, must use RA3 nodes
* Cross-region data sharing involves transfer charges
* Types of data shares
  * Standard
  * AWS Data Exchange
  * AWS Lake Formation - managed

### Redshift Lambda UDF

Redshift Lambda UDFs, User Defined Functions, are something you need to know about, and it's a pretty amazing functionality. This allows you to use functions in AWS Lambda inside your SQL queries on Redshift. And that's powerful, right? Because Lambda functions can be written in pretty much any language you can dream up, and they can do pretty much anything you can dream up. So you could have a Lambda function that calls out to other services, maybe like hit some AI service, right? It can access other systems that might be external. It can also integrate with location services. These are just some examples of what you might do with an AWS Lambda function that's accessed from inside your SQL query. To use them, you just use Create External Function to register your Lambda UDFs that you use within your queries. And on the Redshift table itself, you need to grant usage on language XFUNC in order for this to work and everything to have the right permissions to talk to Lambda. An example of what this might look like, let's imagine that we have a Lambda underscore multiply UDF that we defined. So we could say select A, B from T1 where Lambda underscore multiply A comma B equals 64. So this could be used to say I want to select all the rows in my table where the product of columns A and B equals 64. And this is a very perhaps silly example here of overthinking things, but in this case, we would actually call out to AWS Lambda to perform that multiplication. Obviously, far more complicated things could be done there. Anything that AWS Lambda can do could be done within that Lambda underscore multiply function. So the idea here is that we're tying in Lambda with Redshift through UDFs. Pretty neat. Now to actually register that function, here's what the syntax would look like. So this is a different example here. We're going to create an XFUNC underscore sum UDF here that takes two integers and adds them together, presumably. So the syntax for defining that XFUNC underscore sum UDF would be create external function, the name of that function, and then a list of its parameters that it expects. What it returns, in this case, another integer. So it takes two integers and returns an integer. That's presumably the sum of them. Volatile Lambda and then the name of the Lambda function we're calling. And then finally, we have to pass in the IAM role associated with that Lambda function to give it the permissions it needs to access what it needs to access. 

![](/img/03/168.png)

* Use custom functions in AWS Lambda inside SQL queries
  * Using any language you want!
  * Do anything you want!
    * Call other services (AI?)
    * Access external systems
    * Integrate with location service
* Register with CREATE EXTERNAL FUNCTION
* Must GRANT USAGE ON LANGUAGE EXFUNC for permissions

A little more detail on that IAM role. So there is an AWS Lambda role IAM policy you can use to grant permissions to Lambda on your cluster's IAM role, or you could roll your own policy. Just make sure that you're allowing Lambda invoke function as part of that policy. So you need to create a role with that policy and then pass that in through that parameter on your create external function command like we saw previously. It's also possible to invoke functions in other accounts in the same region using IAM role chaining that way. So you could use somebody else's Lambda UDF using role chaining. So how does this all work? What happens when it actually goes to Lambda? So Redshift will be communicating with your Lambda function using JSON data. So here's what your Lambda function might see coming in. You'll see a request ID, the cluster it came from, the user of the database that it came from in Redshift, the name of the external function, and then a query ID, and finally the number of records that it wants you to compute. So in this case, I'm saying I want you to figure out four things. Here's the list of arguments for each of those four things. And then your Lambda function does something with that and gives back a response. That response might look like this. It will say success, true or false. If there was an error message, you can specify that as well. I'm going to say I'm returning four records here that you asked for, and here are the results. A list of the results for all four records that you asked for. So pretty straightforward, right? Basically Redshift says, okay, well, I'm calling out to Lambda. Here, Lambda, here's the parameters you want. Give me back a response, and I'll work that into my response in SQL. So that's Redshift Lambda UDS, user-defined functions that call out to Lambda in a nutshell. Pretty cool functionality.

* Use AWSLambdaRole IAM policy to grant permissions to Lambda on your cluster’s IAM role
  * Or roll your own policy, to allow lambda:InvokeFunction
* You also need this IAM role in your CREATE EXTERNAL FUNCTION command
  * Possible to invoke functions in other accounts in the same Region, using IAM role chaining
* Redshift communicates with Lambda using JSON

```sql
{
  "request_id" : "23FF1F97-F28A-44AA-AB67-266ED976BF40",
  "cluster" : "arn:aws:redshift:xxxx",
  "user" : "adminuser",
  "database" : "db1",
  "external_function": "public.foo",
  "query_id" : 5678234,
  "num_records" : 4,
  "arguments" : [
    [ 1, 2 ],
    [ 3, null],
    null,
    [ 4, 6]
  ]
}
--to--
{
  "success": true, // true indicates the call succeeded
  "error_msg" : "my function isn't working", // shall only exist when success != true
  "num_records": 4, // number of records in this payload
  "results" : [
    1,
    4,
    null,
    7
  ]
}
```

### Redshift Federated Queries

![](/img/03/169.png)

Let's talk about Redshift Federated Queries. This is a way to query and analyze across databases, across warehouses, and even across data lakes. Now let's specifically talk about the case of tying together Redshift with other databases, with RDS databases in particular. So what we can do is tie Redshift to Amazon RDS or Aurora for PostgreSQL and MySQL currently. Maybe that will expand in the future. But this is exciting because it means I can access live data from RDS and query that from Redshift. So this avoids the need for ETL, right? I don't need to worry about importing data from those relational databases from Aurora or RDS into Redshift itself. I can just query those databases in place directly and avoid the need for moving that data around. So that allows me to create a query in Redshift that hits tables that are in external relational databases like PostgreSQL and MySQL that are in RDS. So that's pretty cool. It offloads the computation as well to those remote databases. So that also reduces data movement further. It means that if I have some complex query, I can break that out and spread out that load to those RDS instances to take up some of that slack.


* Query and analyze across databases, warehouses, and lakes
* Ties Redshift to Amazon RDS or Aurora for PostgreSQL and MySQL
  * Incorporate live data in RDS into your Redshift queries
  * Avoids the need for ETL pipelines
* Offloads computation to remote databases to reduce data movement


How does it work? So first you need to make sure that you have connectivity between your Redshift cluster and the RDS or Aurora instances that you want to talk to. Now you need to make sure they're either in the same VPC subnet or use VPC peering to make them visible to each other. VPC peering again will only work if you don't have overlapping IP address ranges between the two. How to access it? So Redshift needs to know how to sign in and access those RDS or Aurora instances, right? So the credentials for doing so must be stored in AWS Secrets Manager for this to work. And then you include those secrets in the IAM role that you give to your Redshift cluster for accessing those external databases. To create an external table that is hitting those external RDS or Aurora tables, you can say create external schema. And you can also use that same syntax for connecting to S3 or to Redshift Spectrum as well. So remember earlier I said this wasn't just for federating to RDS and Aurora. You can also federate to data lakes this way. Same idea. Over on the right, by the way, is an example of the IAM role that includes the access to AWS Secrets Manager that's needed for accessing those external databases. And if you want to get a quick view of what external schemas are currently available and defined, the svv underscore external underscore schemas view will contain that list for you. 

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
    "Sid": "AccessSecret",
    "Effect": "Allow",
    "Action": [
      "secretsmanager:GetResourcePolicy",
      "secretsmanager:GetSecretValue",
      "secretsmanager:DescribeSecret",
      "secretsmanager:ListSecretVersionIds"
      ],
    "Resource": "arn:aws:secretsmanager:us-west-2:123456789012:secret:my-rds-secret-VNenFy"
    },
    {
    "Sid": "VisualEditor1",
    "Effect": "Allow",
    "Action": [
      "secretsmanager:GetRandomPassword",
      "secretsmanager:ListSecrets"
      ],
    "Resource": "*"
    }
  ]
}
```

* Must establish connectivity between your Redshift cluster and RDS / Aurora
  * Put them in the same VPC subnet
  * Or use VPC peering
* Credentials must be in AWS Secrets Manager
* Include secrets in IAM role for your Redshift cluster
* Connect using CREATE EXTERNAL SCHEMA
  * Can also connect to S3 / Redshift Spectrum this way
* The SVV_EXTERNAL_SCHEMAS view contains available external schemas


Let's look at an example here. 

```sql
CREATE EXTERNAL SCHEMA apg
FROM POSTGRES
DATABASE 'database-1' SCHEMA 'myschema'
URI 'endpoint to aurora hostname'
IAM_ROLE 'arn:aws:iam::123456789012:role/Redshift-
SecretsManager-RO'
SECRET_ARN 'arn:aws:secretsmanager:us-west-
2:123456789012:secret:federation/test/dataplane-apg-creds-
YbVKQw';
SELECT count(*) FROM apg.lineitem;
count
-------
11760
```

Now remember, with federated queries, you only have read-only access to those external data sources. And you may incur additional costs on those external DBs based on the traffic and load that you're putting on them. So it doesn't come for free as part of Redshift. Any load you're putting on Aurora or RDS or the data lake will be in addition, right? So remember, you can query RDS and Aurora from Redshift, but it doesn't go the other way around. This is a one-way connection, and it's a read-only connection from Redshift to RDS and Aurora. Let's take a look at this example on the right here. So here we're saying create external schema APG. So we're going to be referring to this external table as APG within my Redshift queries here. From Postgres, that means I'm going to talk to a Postgres database out there in RDS or Aurora. The database name that I'm talking to is called database1. The table within that database I'm talking to is called my schema. However, I'm renaming it to APG here. Then we say URI with the endpoint to the Aurora hostname that I'm talking to. I pass in that IAM role that we saw on the previous slide. That includes all the secrets manager stuff that I need. And then the ARN of the secret itself that's needed to connect to that Postgres Aurora instance. Once I've done that, I can then access that external table just like any other table. I can say select count star from APG dot line item. APG again is referring to this external Postgres table, my schema under the database1 database in Postgres remotely. And it just works like any other table at that point. Pretty cool. So that's Redshift federated queries in a nutshell. Something the exam expects you to know.

• Read-only access to external data sources
• Costs will be incurred on external DB’s
• You can query RDS/Aurora from Redshift, but not the other way around



### Redshift System Tables and System Views

Just a few notes on Redshift system tables and views. As with most relational databases, you can get information about the system itself by querying its internally maintained tables and views that are just there for you automatically. So these contain information about how Redshift itself is functioning. So at a very high level, there are different types of system tables and views. And let's just go through that high level because there's far too many individual tables and views to go into here. You just need to kind of know what they are and what they're for, right? So there are system views that just start with SYS. Those are the Sys views, and they are used for monitoring query and workload usage primarily. There are also system tables that start with STV. These monitor transient data, containing snapshots of the current system data. And then there are SVV views that contain metadata about database objects that reference back to those STV tables. There are also STL views. These are generated from logs that are persisted to disk. And then there are SVCS views, which are details about queries on both main and concurrency scaling clusters. For specifically queries on main clusters, SVL views will have that information. And the specific tables and views that are available will vary based on whether you're using a provision instance or a serverless instance, but they do have a lot in common. Let's take a look at this example on the right here. So here's an example where we're analyzing execution time of recent queries. So if we kind of break it down here, we can see that we're getting this from STL query, right? So STL query is an STL view. That means that it's generated from logs persisted to disk. And we're joining that on SVL Qlog. So again, SVL is details about queries on main clusters. So SVL Qlog would probably be the query log, right? We're doing a join there between those two based on the query field there. So we're joining up SVL Qlog with STL query to get those recent queries. Specifically saying we're doing a WHERE clause to get it within the past one day, right? So by getting that information on recent queries, we can then select the attributes of those queries, like the start time, the end time, how long that time took. We're gonna compute that and compute the execution time using the big DIP command and the status of that at the end. So just an example of using system tables and system views for getting some information about how Redshift is performing for you. So just remember those are there for you and what they can be used for.

```sql
SELECT q.query, q.starttime, q.endtime,
  datediff(seconds, q.starttime, q.endtime) as 
  execution_time, 
  q.aborted, 
  qr.queue_time, 
  qr.exec_time, 
  qr.total_exec_time
FROM stl_query q
JOIN svl_qlog qr ON q.query = qr.query
WHERE q.starttime > (GETDATE() - INTERVAL '1 day')
ORDER BY q.starttime DESC;

-- Example: analyze execution time of recent queries
```

* Contains info about how Redshift is functioning
* Types of system tables / views
  * SYS views: Monitor query & workload usage
  * STV tables: Transient data containing snapshots of current system data
  * SVV views: metadata about DB objects that reference STV tables
  * STL views: Generated from logs persisted to disk
  * SVCS views: Details about queries on main & concurrency scaling clusters
  * SVL views: Details about queries on main clusters
* Many system monitoring views & tables are only for provisioned clusters, not serveless.

### Redshift Data API

Something that seems to be coming up more on the exam is the Redshift Data API, so let's spend a little bit of time talking about that in some detail. It is a secure HTTP endpoint for SQL statements to your Redshift cluster, and that can be a provisioned cluster or a serverless cluster, and that can handle both individual and batch queries. So it offers, among other things, a REST endpoint that you can integrate with your own applications, and that's what we're illustrating on the bottom of the screen here. So you can have users talking to some application that you've built. That application can then go through the AWS SDK and call the Amazon Redshift Data API through it, and through that API execute queries against your Redshift cluster, wherever it might be. So let's use that outside of AWS potentially. It is asynchronous, so you'll make a call to request a query, either in a batch or an individual query, and then you have to make another call with the identifier that it gives back to you to retrieve the results of that later on. It does not require managing connections like you would with a moral database, so you don't need to worry about having the right drivers installed in your application or anything like that. It's all abstracted away from you through the Data API. And from a security standpoint, you don't have to worry. Passwords are not sent through the API either. It uses either AWS Secrets Manager to manage credentials, or you can set up temporary credentials in Redshift for your application. Either way, the password is not sent across the wire as part of the API. And because it can be invoked by the AWS SDK, that means you can access Redshift through any language supported by the SDK. That includes C++, Go, Java, JavaScript, .NET, Node.js, PHP, Python, and Ruby. And as with most services, it integrates with CloudTrail, so if you need to monitor what's going on or archive what's being performed through that API, CloudTrail can capture those API calls for you and store them in S3 somewhere. And it's not just for application integration.

![](/img/03/170.png)

* Secure HTTP endpoint for SQL statements to Redshift clusters
* Provisioned or serverless
* Individual or batch queries
* Asynchronous
* Does not require managing connections
* No drivers needed
* Passwords not sent via API
* Uses AWS Secrets Manager or temporary credentials
* Can be invoked by AWS SDK
* C++, Go, Java, JavaScript, .NET, Node.js, PHP, Python, Ruby
* AWS CloudTrail captures API calls

#### Redshift Data API: Other use cases

![](/img/03/171.png)

It also integrates with many other services within the AWS ecosystem, and they're all visualized here on the right here. So like I said, we can use the REST endpoint to integrate it with your own application. That's what we talked about. But you can also integrate it with a variety of other services. A very popular one would be using AWS Step Functions together with Redshift Data API to orchestrate a ETL process, extract, transform, and load. So if you want to set up a data processing workflow using Step Functions, you can call Redshift Data API as part of that workflow and set up an event-driven ETL pipeline using Data API. You can also access Redshift Data API through a SageMaker notebook. So if you need to hit Redshift from a SageMaker notebook, you can do that as well. And it also integrates with Amazon EventBridge, and this is an important one in the context of data engineering. So you can stream data from the Data API to someplace else using EventBridge by monitoring the data coming out of the Data API. So you could, for example, stream that data from an application to Lambda by streaming that data using EventBridge. And to do that, you would just set the withEvent parameter to true to accomplish that kind of streaming. You can also schedule Data API operations using EventBridge. So EventBridge can be used to schedule operations on a consistent schedule or in response to some sort of other event. So EventBridge plus Data API is something you should know about. 

* Application integration
  * REST endpoints
* It can also integrate with a variety of other services
  * ETL Orchestration with AWS Step Functions
    * Serverless data processing workflows
    * Event-driven ETL
  * Access from SageMaker notebooks
* Use with Amazon EventBridge
  * Stream data from application to Lambda
    * Set WithEvent parameter to true
  * Schedule Data API operations

#### Redshift Data API: Finer Points

Some finer points on using the Data API. So there's a bunch of maximum limits on what you can do with it. And by default, the query duration can only last for 24 hours. You can only have 500 active queries at a time. The result size, which is gzipped, can only be up to 100 megabytes. The result will only be retained for 24 hours. So if you don't get it after a full day, it's going to be gone. Your query statement size can only be 100 kilobytes. And this next one's a little bit more important. The packet for query size, that's the amount of data per row in your results, is 64 kilobytes. So if you were to get back an error message from your Data API call saying that packet for query is too large, it's telling you that there's too much information per row in the data that you're retrieving. And that maximum limit is 64 kilobytes. Also, there's an eight-hour maximum retention time for client tokens for accessing the API. And there are various TPS quotas per API call. For example, for execute statement, you're limited to 30 transactions per statement. Speaking of API calls, these are the ones we have available to you. There's both execute statement for executing a single SQL statement or batch execute statement for executing multiple ones at once. And once you have that statement submitted, you can describe the statement or the table behind it. And again, this is asynchronous. So once you call an execute statement call or batch execute statement call, it's going to just give you back an identifier, not the actual result. So you then need to turn around and pass that identifier back into getStatementResult to later retrieve the result of that query. You can also cancel a statement that's in progress using cancelStatement using that same ID. And there is some security around making sure that you can't snoop in on other people's queries. So you do need to have the same IAM role or permission to operate on the given statement across execute and getStatementResult or what have you. So you can't go and get a statement result for somebody else's execute statement call, for example, obviously. Also, the cluster must be in a VPC. So unless your Redshift cluster is in a virtual private cloud, it won't work.

* Maximum…
  * Query duration: 24 hours
  * Active queries: 500
  * Query result size (gzip’ed): 100 MB
  * Result retention time: 24 hours
  * Query statement size: 100 KB
  * Packet for query (data per row) : 64 KB
  * Client token retention time: 8 hours
  * Transaction per Second quotas per API (30 for ExecuteStatement)
* API Calls
  * ExecuteStatement, BatchExecuteStatement
  * DescribeStatement, DescribeTable
  * GetStatementResult, CancelStatement
  * Users must have same IAM role or permissions to operate on a given statement
* Cluster must be in a VPC

### Redshift - Hands On

All right, let's get a little bit of hands-on experience using Redshift. What I'm going to do is load up a dataset of Amazon reviews from the magazine category. I don't think Amazon even sells magazines anymore, which is a shame. Give me paper over a screen any day, I say, but I digress. So, this does involve some real money. Now, Redshift, at this time, gives you a sort of a free credit if you're using it for the first time. So, odds are you can do this for free. Even if you don't, the charges that we're going to incur are going to be less than `$`1 if you do this all within one hour. However, big disclaimer here. If you mess up, if you do not follow the instructions here, if you select the wrong instance type for Redshift, or if you forget to shut it down when you're done, it could cost you thousands of dollars. So, if you are at all nervous about that, do not follow along hands-on. Just watch me do it. There are risks here, and I am not responsible if you don't follow the instructions and end up with a big bill at the end of the month. But if you do follow instructions and do follow along, it should be either free or under $1. First thing you want to do if you are following along is to get the course materials. If you haven't already downloaded them from earlier in the course, head on over to sundog-education.com slash aws-data-engineering. You'll find a handy link to the course materials there. Just download that to your computer and unzip them wherever you want. And when you're done with that, it should look like this. There should just be a magazine reviews folder in there. And that's where our data lives. 

So, what do we got here? Well, this `citation.txt` file is just a text file with the required credits here for the data set, where it came from, and the paper that they want you to cite whenever you use this data. So, there's that. There's two things here. One is the actual reviews data for these **`magazine subscriptions`**. These are people who came to Amazon.com and wrote a product review for each magazine. And this is a data dump from, well, 2019, I guess it is. Let's take a look at what that looks like. Open that up in my favorite text editor. And you can see this is in JSON format. And it's pretty wordy. So, we have the actual review test for every line here, along with the reviewer ID that identifies the individual reviewer. The ASIN is basically the primary key here. 

So, in the Amazon world, the ASIN represents the product identifier. So, this is a unique identifier for this magazine. So, that's an appropriate thing to use for our primary key, right? We also have the human readable name for this reviewer. And there could be more than one ID for each unique name, right? That's not like a one-to-one mapping there necessarily. And the other thing we care about here is the overall field. So, that is what the overall review score is for this magazine. So, basically, Ted really liked whatever B00005N7P0 is for a magazine and gave it five stars. All right. 

So, what we're left here is the problem of understanding what the heck B00005N7P0 is, right? And that's where this other file comes in, the metadata file, also in JSON format. Let's take a look at that as well. And this is where it gives you information about each individual ASIN. You can see there's a lot of stuff in here, including lists of recommended other magazines and things like that. But the actual name of the magazine is under the brand here, Reason Magazine. At least that's the brand of the publisher more consistently. And it gives you information about its main category. And there's its ASIN down there buried there. So, again, here is the primary key that we're going to use in the metadata as well. This is the unique identifier for this magazine. And this file contains all the metadata surrounding that magazine. So, you can probably guess where we're going with this. We're going to be doing some joins to marry these two datasets together, right? All right. 

```shell
├── dea-materials
│   ├── AWSCertifiedDataEngineerSlides.pdf
│   ├── AllSlides_v3.1.pdf
│   ├── MagazineReviews
│   │   ├── citation.txt
│   │   ├── metadata
│   │   │   └── meta_Magazine_Subscriptions.json
│   │   └── reviews
│   │       └── Magazine_Subscriptions.json
```

```json
{
  "overall": 5.0, 
  "vote": "9", 
  "verified": false, 
  "reviewTime": "11 8, 2001", 
  "reviewerID": "AH2IFH762VY5U", 
  "asin": "B00005N7P0", 
  "reviewerName": "ted sedlmayr", 
  "reviewText": "for computer enthusiast, MaxPC is a welcome sight in your mailbox. i can remember for years savorying every page of \"boot\" (as it was called in beginning) as i was (and still am) obcessed with PC's. Anyone, from advanced users - to beginners looking for knowledge - can profit from every issue of MaxPC. the icing on the cake is the subscription that comes with a CD-ROM as it is packed with demos, utilities, and other useful apps (very helpful for those not blessed with broadband connections). Until I discovered the community of hardware enthusiast web sites, MaxPC, formerly \"boot\", was my only really informative source for computing news and articles. To this day, i consider my subscription to it worth more than 10 subscriptions to most other computing mags. I can't wait until they merge with DVD media and maybe end up offering more info on Divx codecs, encoding your own movies, and best bang for the buck audio and video equipment. Try a few issues (with CD)and you may get hooked...", "summary": "AVID READER SINCE \"boot\"  WAS THE NAME", 
  "unixReviewTime": 1005177600
}
{
  "overall": 5.0, 
  "vote": "9", 
  "verified": false, 
  "reviewTime": "10 31, 2001", 
  "reviewerID": "AOSFI0JEYU4XM", 
  "asin": "B00005N7P0", 
  "reviewerName": "Amazon Customer", 
  "reviewText": "Thank god this is not a Ziff Davis publication.  MaxPC will actually tell you if a product is bad. They will print just what they think about something; no sugar coating. I would compare their style to Car and Driver. Technical, but they know how to have a good time.", 
  "summary": "The straight scoop", 
  "unixReviewTime": 1004486400
}
...

```


So, the first thing we need to do is put this somewhere where Redshift can get it. Now, it is possible to upload data directly from a local disk in Redshift, but it is limited to five megabytes. And this data is more than five megabytes. So, we've got to get it in there through some intermediate channel. S3 would be appropriate, right? So, let's go ahead and upload this into an S3 bucket. So, on to your AWS console here. Go on to S3. There's probably a link to it at your little top bar here, or you can just search for it if you need to. 


And let's go ahead and create a `new bucket` for this activity. 

![](/img/03/172.png)

I'm going to call it DEA-C01-Sundog. Now, remember, bucket names need to be globally unique. So, if you try to use that bucket name, it's not going to work because I already took it. So, substitute Sundog with your own name, some unique identifier, that will make that bucket name unique to you. All right, we're going to leave ACLs disabled because I'm going to set the public access at the top level. 

![](/img/03/173.png)

So, let's talk about security here for a moment. So, the best practice would be if you were dealing with some sort of sensitive or proprietary or personally identifiable information here, obviously you would not want to make this data publicly accessible. I am, however, going to do that for this exercise just to make it easier and to avoid the need for you to watch me put all of my personal identifiers for my AWS account as we go. So, in the real world, you would probably not disable block all public access. But just to make things easier here, I am going to disable block all public access and actually make this entire bucket publicly accessible. 

![](/img/03/174.png)

Now, there are legitimate reasons for doing that. If you're making a static website, for example, and hosting the content from S3, that would be a legitimate reason to do this. But keep in mind, this is not something you would normally do in a security conscious environment. I'm just doing it to make this exercise a little bit simpler. And I will talk through how to do this correctly in a more secure way as we go through. All right, so we're going to go ahead and do that. We're going to disable bucket versioning because we're just playing around here. We don't need to have backups of any of this data. No tags. Default encryption is fine. And let's go ahead and `create our bucket`.

![](/img/03/175.png)

All right, so let's select the bucket we just made. There it is. Again, yours will be named differently. And we need to do a little bit more to make that data accessible to Redshift. So in order for Redshift to read data from S3, it needs permissions not only to get objects within this bucket, but also to list what's in this bucket. So let's go ahead and take care of those permissions first.

![](/img/03/176.png)

So let's go to permissions, and you can see that block all public access is off. However, we need to define a bucket policy to allow the permissions we want. Now I'm gonna cheat here and just copy it in. So rather than make you watch me type, what I'm gonna ask you to do is just hit pause here when we're ready. So let's hit edit on the bucket policy. 

![](/img/03/177.png)

And before you hit pause and start typing this in, make sure that you take care to replace the resource name here with your own bucket. So you can see that the bucket name that I just used is right there, and also again right here. So you will need to substitute that with your own bucket name.

![](/img/03/178.png)

![](/img/03/179.png)

Also, if you want to actually do this in a secure manner and not open this up to the entire world, there are two things you would do differently there. Of course, first of all, you would not do what I did to enable public access **`"Principal": {},`**. And also, instead of opening up the principal field to star, which means everyone, you'd want to lock that down to a specific IAM user, right? Or maybe even to the to the Redshift service itself. So rather than star, you can lock that down to, you know, ARM colon AWS colon IAM colon colon, whatever your IAM identifier is, okay? So in the real world, in a secure setting, you would want a more secure bucket policy here. Remember, we want to follow what's called the principle of least privilege. So you want things locked down as tightly as possible and only provide the permissions that are needed for the task at hand. So this is a case of do as I say and not say as I do. But again, to make life easier, just go ahead and hit pause and copy this into your own bucket policy. Again, substituting that bucket name for your own bucket name. Come back when you're done. 

```sql
{
	"Version": "2012-10-17",
	"Statement": [
		{
			"Sid": "Statement1",
			"Principal": "*",
			"Effect": "Allow",
			"Action": [
				"s3:ListBucket"
			],
			"Resource": "arn:aws:s3:::dea-sundog"
		}
	]
}
```

All right, once you've typed all that in, we can go ahead and save it.

![](/img/03/180.png)

And it took it, so that means you at least passed some syntax checking at least. And let's go ahead and upload our data into this bucket.

![](/img/03/181.png)

 So back to objects, let's click on upload. And you can just drag and drop them here. So let's go back to our course materials and we're going to just move them in from this top level here. So I'm going to select the metadata and reviews folders and drag those in, like so.

 ```shell
├── dea-materials
│   ├── AWSCertifiedDataEngineerSlides.pdf
│   ├── AllSlides_v3.1.pdf
│   ├── MagazineReviews
│   │   ├── citation.txt
│   │   ├── metadata
│   │   │   └── meta_Magazine_Subscriptions.json
│   │   └── reviews
│   │       └── Magazine_Subscriptions.json
 ```

![](/img/03/182.png)

![](/img/03/183.png)

![](/img/03/184.png)

And go ahead and hit upload when you're ready. It's a fair amount of data, so it might take a minute. And there we have it. All right, so if we go back to view that bucket, we should see that we now have a metadata folder and a reviews folder. And within each folder we have a single file.

![](/img/03/185.png)

All right, so now that our data is in S3 and in theory accessible to Redshift. And because I made it publicly accessible, it's accessible to everybody right now. Again, not the best practice, but remember to lock that down to the appropriate principle in the real world. Let's go ahead back to services and go to Amazon Redshift. 

![](/img/03/186.png)

Again, you can just search for it here. And let's go and make a Redshift cluster. Now, right now they're really pushing Redshift serverless. This is probably the future of Amazon Redshift, but it actually costs more than creating a single cluster with a single machine of a low-cost machine. So again, just to err on the side of caution here in case you forget to shut this down, we're going to go ahead and use a traditional cluster here where we're actually managing our own resources instead of using Redshift serverless. The process of actually importing data and querying it is going to be similar in both cases, but I just want more control. So yeah, I want more granular control. I'm going to create a cluster from scratch here.

--- 

All right, now be careful here. See this here? $4,759 per month? You don't want that. You do not want to select the default options here because I don't think you want to get that bill at the end of the month. But don't worry, we're going to get that down to something much more manageable. All right, so our cluster needs an identifier. What should we call it? How about AMZ reviews? 


![](/img/03/189.png)

And I am going to choose the node type. I do not want an RA3 4x large. This is what you might be using in the real world for an actual data warehouse, but we're just playing around here. So I'm going to select the cheapest one I can find. That's going to be DC2.large, and that only costs 25 cents per node per hour, which is a lot cheaper than \$3.26 per hour. And I only need one node. I don't care about redundancy because, again, we're just playing around here, right? And you can see we've already taken that down to $182 per month, which is a lot more manageable. And if you do the math, using this for an hour is going to be like less than a dollar. So this is more like it, right? But do be careful. Don't skip that step. Again, I'm not responsible if you choose the wrong setting there and end up with a big bill at the end of the day. And also remember to shut this cluster down before you're done. So if you don't watch this exercise to the end and forget to do that, again, not my fault, okay? 

![](/img/03/200.png)

All right, let's go on and continue setting up our cluster here. So our admin username needs a username. Let's leave it with AWS user. I'm going to go ahead and give it a password of my own here. So go ahead and type in a password and remember what it is.

![](/img/03/201.png)

Now the next thing we need to do is create an IAM role for this cluster to be able to do what it needs to do. And we need to go and create one right now.

![](/img/03/202.png)

It says here the key is to have it with the Amazon Redshift all commands full access policy attached. There's more to it than that, though. We can say create an IAM role under manage IAM roles here. 

![](/img/03/203.png)

So let's go ahead and select that. I have a lot of them. There it is. So select your own there, of course, and create IAM role as default. All right, so that automatically created one for me there. That's great, thank you. 

![](/img/03/204.png)

Let's go ahead through these other options here as well. So we do need to define a subnet group. We can't just leave this as is. So I'm gonna have to go ahead and choose a VPC that has a subnet. You can see that one already has one called Redshift subnet. If you need to create one, you might have to go to the VPC UI and go make one. Availability zone, I'm gonna choose the one that I'm currently in. Enhanced VPC routing. Don't need that right now.

![](/img/03/205.png)

That's network traffic between your cluster and data through a VPC instead of through the internet. So that would be more secure to turn that on, but again, since we're making everything publicly accessible anyhow, doesn't really do much good. The Redshift cluster itself, however, is not publicly accessible. Definitely don't want to do that because Redshift can get expensive real fast. Let's see what else is in here. 

Database configuration. So our database name will be dev, remember that. Database port is the standard 5439. And encryption, note that that's disabled by default there. So if you were dealing with any sort of sensitive or personal information, obviously you'd want to change that. Let's go ahead and encrypt that just because we can. So I'll use KMS with the default key. It's better than nothing, which was the default there. 

Maintenance. This isn't going to be running long enough to need a maintenance window, but we'll just go with the current defaults there. That's fine. Alarms. Again, in the real world, of course, you would want alarms. But things like disk usage and you could create an SNS topic to notify you and send an alarm if you exceed the disk usage of 70 percent there or whatever you want it to be. However, I don't care because we're just playing around. So I'll leave that to no alarms. 

Backups. I'm going to set that to zero because, again, I'm just playing around. I don't care if I lose this data. It's not real data. Again, in the real world, of course, you'd want to think about this, right? So fortunately, it can automatically back up your data. However, that can get expensive. Also, note that you're saying, do I want to configure a cross-region snapshot? Something to think about here is where are you allowed to store your data? There could be regulations in place that restrict where your data can be, especially if it involves personally identifiable information. Different countries might have different laws surrounding that, and you might not want to be storing your data in a different country that has different laws you don't even know about. 

![](/img/03/206.png)

All right, so this all looks good. Let's say create cluster. 

 And now we just wait for that to spin up. All right, after a few minutes, our cluster has spun up, so let's go ahead and connect to it and do stuff with it. So you can see here we have three very convenient ways of connecting to it. It will give you a link for connecting through a SQL client of your own, or if you have a JDBC or ODBC driver that you want to use, it can generate that for you as well. However, we're going to keep it easy and just use their console-based query editor here. So I'm going to click on **`query data`**.

![](/img/03/207.png)


![](/img/03/209.png)

![](/img/03/210.png)

![](/img/03/211.png)

![](/img/03/212.png)

![](/img/03/213.png)

So JSON is not necessarily structured data. It could be semi-structured. And you could have these weird nested structures within there that aren't really supported by Redshift. So this may or may not go well. Let's go ahead and see what happens, though. 

![](/img/03/214.png)

So this may or may not go well. Let's go ahead and see what happens, though. All right, so we're going to make a new table under the public schema. And let's give this table a name. Let's call it Reviews. We need to choose an IAM role, so let's just select the one we created earlier. And double-check the mapping here of our data. So it's trying to extract a schema from that JSON file itself. And for the most part, it did a pretty good job, right? So these all make more or less sense. However, it couldn't figure out this last one. There's no data type you see under Image. So presumably, that's some complex data structure that we just can't represent directly in Redshift. Now, if we had a proper ETL pipeline in place, maybe we could restructure that data in a way that would be more appropriate for a relational database. But we haven't got to that part of the course yet. So instead, I'm just going to delete that field because, well, I don't need it for what I'm doing here. So that's one easy solution. Everything else looks like it should be okay. So the other thing we can do is make a primary key while we're at it. So like we said, the ASIN is the unique identifier for each individual item being reviewed. So that seems like a reasonable thing to use for something to join these tables together by. So let's go ahead and select that ASIN column and make that our primary key. And now we should be able to create a table. 


![](/img/03/215.png)

![](/img/03/216.png)

![](/img/03/217.png)

![](/img/03/218.png)

And now we should be able to create a table. One last check of everything. Looks good to me. Go for it. Off it goes. So as we speak, it's copying that data from that JSON file in S3 and loading that into a new table that we call reviews. And it succeeded. Very good. 

Let's do the same thing for the metadata. So back to load data from a S3 bucket. And this time we'll select our metadata file. Under your bucket name, then metadata, then meta **`magazine subscriptions`**.

And this data is a little bit more messy. So one problem is that some of the fields are longer than 255 characters. So when we try to put that into a VARCHAR 255, it's not going to work. So let's go to data conversion parameters here. 

![](/img/03/220.png)

And say that we're going to truncate data to fit the column specification.

So if we have a column that is too wide for our data type, we'll just cut it off. That's fine for our purposes here. We'll choose an IAM role again. Again, we'll select the public schema. And we will make a new table. With the following table name. We'll call it metadata. All right. 

So again, we have some issues with structures that it couldn't deal with. So wherever you see a blank data type, that means that we just couldn't force JSON data into a relational database column. So it turns out the only thing I actually care about in this is the brand column and the async column. Those are the only things I'm going to be actually working with for my use case. So everything else I don't really care about. 

![](/img/03/224.png)

![](/img/03/223.png)

Let's go ahead and delete this async colon field here. Because async is the actual primary key. This is under the details. And I don't want to get any confusion between these two different asyncs. So that's going to go. And additionally, anything that does not have a data type is going to go. Because presumably, most of these are arrays. They can be lists of things. So for example, every individual item can have more than one category. But there's no good way of representing that in this database. Same thing with description. Also by, that's a list of recommendations that go along that feature. Also view, same kind of idea. And again, the image is something that I can't seem to handle. But I do not care. Because again, for my use case, all I'm going to be looking at are the asyncs and the brands. 

![](/img/03/221.png)

So before I forget, let's make the async the primary key as well. 

![](/img/03/225.png)

![](/img/03/226.png)

And we should be good to go. Create table. Double check everything. Load data. And it worked. All right, so if we refresh this little panel here, we should be able to see those two new columns that we created. Those two new tables, rather. Under dev, public, tables. And there they are, metadata and reviews. Very cool. All right, so let's see if it works.

All right, so let's see if it works. All right, so we're going to be working within the Amazon Reviews dev database here. And let's just run some SQL, shall we? Let's start with something basic like SELECT STAR FROM METADATA, LIMIT 10. And we have data. As you can see, there's a lot of missing data as well. So if we were actually building an ETL pipeline, there'd be a lot of missing data to deal with and figure out what we want to do about that. But importantly, the ASINs came through that we care about, and the brands also came through that we care about. Those are basically the publishers of the magazines in question. 

![](/img/03/227.png)

Let's do the same thing for the Reviews data and make sure that looks okay. So let's start from Reviews, LIMIT 10, and run that. Looks good. So we have the overall review score, 1 to 5 stars, and the ASINs, those are all we really care about here. Again, lots of nulls here, lots of missing data. So in a real pipeline, we want to dig into why that is and maybe deal with that better. But for our use case, we don't care.

![](/img/03/228.png)

So let's do something more interesting. Let's write a query that sorts the highest rated brands. So how about a little bit of a challenge for you? Let's see if you can write a SQL query that joins these two tables together and gives me back what brands have the highest average ratings. So we have these ASINs here in the Reviews table and the overall ratings for each, and we can join that with the metadata table to figure out the brand of that magazine in each case. So your challenge, should you choose to accept it, is to write a query that sorts the highest rated brands based on their average overall score. And go ahead and hit pause if you want to try that on your own, but if you don't, I'll just give you the answer here. Looks like this. So we're going to select... First of all, we have some aliases here. So the metadata table is being abbreviated to M, Reviews has an alias of R. So we're going to select M.brand, the brand name, the average of the overall from the Reviews table. We're going to label that as average review score from the Reviews table. We're doing a inner join, metadata M on R.ASIN equals M.ASIN. So we're joining these two tables together based on the ASIN field, grouping them by brand so that we're looking at every brand together and computing the average score for each brand, and then finally sorting the final results by average review score, which is that average result up here. Let's run that and see what happens. And there you have it. So this looks a little bit suspicious. So there's an awful lot of magazines with an average review score of five. And I got to wonder if maybe they just don't have a lot of data, like maybe there's only one review for each of these brands and that one review was five stars, right? 

![](/img/03/229.png)

So let's get a little bit more context around this. Your next challenge, if you are up to it, is to extend this query to restrict this to only brands that have 10 or more reviews and actually print out how many reviews each result has. So that will filter out those ones where we just don't have enough data to really say definitively what that average score really means. So if you want to take a crack at that yourself, hit pause and have a go at it. If not, I will just paste in the answer here. So you see here I threw in a having clause here, having countR.asin greater than 10. So we're going to restrict this query only to brands that have more than 10 ratings. And we're also going to print countR.asin as an additional column in our results so we can get that added context about how many reviews actually went into that average calculation. So let's run that. And that's a little bit more meaningful now. So we see we still have a few that have an average review score of 5.0. And coming from more than 10 reviews, I guess that's meaningful. So people really like the publications coming from KMT Communications, KBS Communications, Clay & Clay, and Canadian Home Publishers, and Historical Enterprises, LLC. But down here we get a little bit more real. 

![](/img/03/230.png)

So, you know, these things that are slightly less than 5, I don't know, they feel more authentic to me because, you know, it's unlikely that everybody loves something. But we can see here that genuinely, you know, Prehistoric Times and Maker Media, oh, yeah, Make Magazine, I like that one. That is a good magazine. These are all very well received by the reviewers in this data set. So that's a fun little example of using Redshift with some real-world data. And if you want to play around with this some more, you can, but just remember the clock is ticking here. You are being charged by the hour that this cluster is running. So in order to save your pennies, I would recommend closing out of this and shutting down your cluster. So do not forget this next step because if you leave this cluster running, you will be billed for it forever, constantly, whether you're using it or not. You don't want that, right?

![](/img/03/231.png)

So let's go back to Services and go back to Redshift. Go back to the cluster we just made, AMC Reviews. And I'm going to go to Actions and say Delete. And to make sure that I really want to delete this, I'm going to say Delete because this does in fact delete all the data in there.


![](/img/03/232.png)

However, it's still going to exist in S3. We just copied it over from S3, right? So quick note there, by the way, we could have used Redshift Spectrum to query that directly in S3 in place, but doing so would have required me to create a data catalog first, and we haven't covered that yet in this course, so that's why I didn't do it that way. 

Let's go ahead and delete our copy of that in this cluster and make sure that deletion happens because if not, you will continue to be billed for this cluster forever. You don't want that. I'm going to wait for that to go from Status Modifying to Status Something Else. 

![](/img/03/233.png)

Let's go back to our Clusters view here and keep an eye on it. Still wait for that to delete. Oddly, it's taking longer to delete this cluster than it did to create it, but let's wait another minute here. We'll come back when that's done for good. All right, about five minutes later, it finally says it successfully deleted AMC Reviews. Let's go ahead and refresh this to make sure. Sure enough, my cluster's gone, so I'm not going to get billed for that anymore. All right, so that is Redshift in action, hands-on. We copied in some data from S3 into a Redshift cluster, as opposed to accessing it in place. We'll do that later in the course. But yeah, that's Redshift, and I'm not going to have you delete the S3 bucket that we made We're going to reuse that data later on in the course, and S3 doesn't cost too much. When you're done with the course, you can delete that bucket, but for now, if you're going to continue on, keep it there for now. Moving on.

![](/img/03/234.png)

