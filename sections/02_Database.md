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

3:54

### DynamoDB in Big Data

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

* Table must have provisioned read and write capacity units
* **Read Capacity Units (RCU)** – throughput for reads
* **Write Capacity Units (WCU)** – throughput for writes
* Option to setup auto-scaling of throughput to meet demand
* Throughput can be exceeded temporarily using **“Burst Capacity”**
* If Burst Capacity has been consumed, you’ll get a **“ProvisionedThroughputExceededException”**
* It’s then advised to do an **exponential backoff** retry