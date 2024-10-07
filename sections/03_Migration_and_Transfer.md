**Migration and Transfer**

**Moving data into AWS**

This next section is pretty quick. Migrating and transferring data is, of course, a big piece of data engineering, but Amazon's offerings in this space are relatively straightforward. You just need to know what tools they offer for migration and transfer and how to choose between them. Stefan will be covering this section, where he'll teach you about the AWS Application Discovery Service, the AWS Database Migration Service, AWS DataSync, the AWS Snow family, and the AWS Transfer family. As always, you'll have a few hands-on examples, and we'll wrap up with a quiz written in the style of exam questions. So, let's learn how to move data into AWS from wherever it may be.


- [Application Discovery Service \& Application Migration Service](#application-discovery-service--application-migration-service)
  - [AWS Application Discovery Service](#aws-application-discovery-service)
  - [AWS Application Migration Service (MGN)](#aws-application-migration-service-mgn)
- [DMS – Database Migration Service](#dms--database-migration-service)
  - [DMS Sources and Targets](#dms-sources-and-targets)
  - [AWS Schema Conversion Tool (SCT)](#aws-schema-conversion-tool-sct)
  - [DMS - Continuous Replication](#dms---continuous-replication)
  - [AWS DMS – Multi-AZ Deployment](#aws-dms--multi-az-deployment)
  - [AWS Application Discovery Service (AWS DMS) - Hands On](#aws-application-discovery-service-aws-dms---hands-on)
- [AWS DataSync](#aws-datasync)
- [AWS Snow Family](#aws-snow-family)
  - [AWS Snow Family - Hands on](#aws-snow-family---hands-on)
- [AWS Transfer Family](#aws-transfer-family)
- [Compute](#compute)
  - [EC2 in Big Data](#ec2-in-big-data)
  - [EC2 Graviton-based instances](#ec2-graviton-based-instances)
- [AWS Lambda](#aws-lambda)
  - [What is Lambda?](#what-is-lambda)
  - [Lambda Integration - Part 1](#lambda-integration---part-1)


### Application Discovery Service & Application Migration Service


#### AWS Application Discovery Service

So there are two use cases when you move to the cloud. For example, you want to start fresh and you want to leverage the cloud directly. In this case, you don't need to do a migration. But if you have on-premises servers and data centers and you want to migrate to the cloud, then you need to plan your migration. And a way to do this is to plan your migration using the AWS Application Discovery Service. So the idea is that you're going to scan your servers and you're going to gather information about the server installation data and the dependency map thing, which are going to be important for migrations, so you can understand how to migrate and what to migrate first. So there are two types of migration you can do. One is called the agent-less discovery using Connector, and it gives you information around your virtual machines, your configuration, your performance history such as CPU, memory, and disk usage. Or you can run an agent to do an application discovery agent, and this gives you more updates and more information from within your virtual machines, for example, system configuration, performance, processes are running, and the details of all the network connections between your systems, which is good to get your dependency mapping. Now, all this results data can be viewed within another service called the AWS Migration Hub. So this Application Discovery Service really helps you to map out what you need to move and how they are interconnected. But then you actually need to move.

* Plan migration projects by gathering information about on-premises data centers
* Server utilization data and dependency mapping are important for migrations
* Agentless Discovery (AWS Agentless Discovery Connector)
* VM inventory, configuration, and performance history such as CPU, memory, and disk usage
* Agent-based Discovery (AWS Application Discovery Agent)
* System configuration, system performance, running processes, and details of the network connections between systems
* Resulting data can be viewed within AWS Migration Hub

#### AWS Application Migration Service (MGN)

And the simplest way to move from on-premises to AWS is using the AWS Application Migration Service, also called MGM. So this used to be called CloudEndure Migration, but now it's been replaced. And the idea is that using the AWS Application Migration Service, so MGM, you can do re-hosting, also called lift-and-shift solution, in which you convert your physical, virtual, or other services on other clouds to run natively on AWS. How does that work? Well, say you have a corporate data center with OS, apps, and databases, and they're all on disk. What's going to happen is that you're going to run the Application Migration Service, and then the replication agent that you have to install on your data center is going to perform a continuous replication of your disks, so that you have, for example, low-cost EC2 instances and EBS volumes that get this replication of data. Now, the day you're ready to perform a cutover, you can actually move from staging to production and have a bigger EC2 instance of the size you want, as well as EBS volumes that match the performance you need. So the idea is that you replicate data, and then at some point do a cutover. And that is, by far, the simplest way of doing it. So this supports a wide range of platforms, operating systems, and databases, and this gives you minimal downtime, as well as reduced costs, because you don't need to hire complex engineers to do this. This is done automatically by this service.

![](/img/04/01.png)

• The “AWS evolution” of CloudEndure Migration, replacing AWS Server Migration Service (SMS)
• Lift-and-shift (rehost) solution which simplify migrating applications to AWS
• Converts your physical, virtual, and cloud-based servers to run natively on AWS
• Supports wide range of platforms, Operating Systems, and databases
• Minimal downtime, reduced costs

### DMS – Database Migration Service

So let's say you wanted to migrate a database from your on-premise systems to the AWS cloud. In this case, you should use DMS for Database Migration Service. It's a quick and secure database service that allows you to migrate your database from on-premise to AWS. But the cool thing is that it's resilient and it's self-healing. And all alongside the migration, the source database remains available and it supports many types of engines, such as homogeneous migration, so from Oracle to Oracle or Postgres to Postgres, but also heterogeneous migrations, for example, if you want to migrate from Microsoft SQL Server all the way to Aurora. And it supports continuous data replication using CDC, which is Change Data Capture. Finally, to use DMS, you need to create an EC2 instance, and that EC2 instance will perform the replication tasks for you. So very simply, your source database may be on-premise, and then you're running an EC2 instance that has the DMS software, and it will pull the data from the source database in continuously and put it in the target database. So the question is, what are the sources and what are the targets? 

![](/img/04/02.png)

* Quickly and securely migrate databases to AWS, resilient, self healing
* The source database remains available during the migration
* Supports:
  * Homogeneous migrations: ex Oracle to Oracle
  * Heterogeneous migrations: ex Microsoft SQL Server to Aurora
* Continuous Data Replication using CDC
* You must create an EC2 instance to perform the replication tasks


#### DMS Sources and Targets

So the question is, what are the sources and what are the targets? You don't need to remember them all, but they're important to see once you understand the concepts behind DMS. So the sources can be on-premises databases or EC2 instances based database such as Oracle, Microsoft SQL Server, MySQL, MariaDB, PostgreSQL, MongoDB, SAP, and DB2. It can also be Azure databases such as Azure SQL Database. It could be Amazon RDS, any database including Aurora. It could be Amazon S3 and DocumentDB. In terms of targets, well, we have different options as well. We have on-premises and EC2 instances databases, so we can have Oracle, Microsoft SQL Server, MySQL, MariaDB, PostgreSQL, SAP. We can also have any database on Amazon RDS. We can have Redshift, DynamoDB, Amazon S3, the Open Source Service, Kinesis Data Streams, Apache Kafka, DocumentDB, and Amazon Neptune, as well as Redis and Babelfish. So you don't need to remember all of these, of course, but the general idea is that DMS can help you take a database, for example, an on-premises database, and have it put and exported and migrated onto a target that is pretty much any database that AWS is offering. If you understand this, then you have the general idea behind DMS. 

SOURCES:
* On-Premises and EC2 instances databases: Oracle, MS SQL Server, MySQL, MariaDB, PostgreSQL, MongoDB, SAP, DB2
* Azure: Azure SQL Database
* Amazon RDS: all including Aurora
* Amazon S3
* DocumentDB

TARGETS:
* On-Premises and EC2 instances databases: Oracle, MS SQL Server, MySQL, MariaDB, PostgreSQL, SAP
* Amazon RDS
* Redshift, DynamoDB, S3
* OpenSearch Service
* Kinesis Data Streams
* Apache Kafka
* DocumentDB & Amazon Neptune
* Redis & Babelfish

#### AWS Schema Conversion Tool (SCT)

So what if the source database, the target database, do not have the same engine? Then you need to use something called AWS SCT, or Schema Conversion Tool, and it will convert the database schema from one engine to another. So for example, if you're using a PowerTP, we can migrate from SQL Server or Oracle to MySQL, PostgreSQL, or Aurora. You can see on the left-hand side, the database engine is different from the one on the right-hand side. But also, you can transform for another exponent, such as Teradata or Oracle, all the way to Amazon Redshift. So the idea is that here, the source database has a different engine than the target database. In the middle, we have DMS, but it's now also running SCT, or Schema Conversion Tool. And the thing to note going into the exam is that you do not need to use SCT if you are migrating the same database engine. So if you're doing on-premise PostgreSQL to RDS PostgreSQL, it is the same database engine, and it's PostgreSQL, and so this work, we will not use SCT. But if you're doing something such as Oracle to PostgreSQL, then you will need to use SCT. So just so you know, the database engine is PostgreSQL, but RDS is just a platform that we're using to run this database engine.

* Convert your Database’s Schema from one engine to another
* Example OLTP: (SQL Server or Oracle) to MySQL, PostgreSQL, Aurora
* Example OLAP: (Teradata or Oracle) to Amazon Redshift
* Prefer compute-intensive instances to optimize data conversions

![](/img/04/03.png)

* **You do not need to use SCT if you are migrating the same DB engine**
* Ex: On-Premise PostgreSQL => RDS PostgreSQL
* The DB engine is still PostgreSQL (RDS is the platform)

#### DMS - Continuous Replication

So how do you go to set up a continuous replication for DMS? Well, you would have a corporate data center, for example, with an Oracle database as a source, and an Amazon RDS database from MySQL DB as a target. And as you can see, we have two different kinds of types of database. So in this case, we have to use SCT, otherwise it will not work. So the Schema Conversion Tool. So we set up a server with AWS SCT installed, and we can set it up on-premises. This is best practice. And then we will do the schema conversion into our Amazon RDS database running MySQL. Then we can set up a DMS replication instance that will do the full load and the change data capture of CDC to have continuous replication. And so it will perform the data migration by reading the database on-premises, the source Oracle database, and inserting the data into your private subnets. That's it. So that's all you need to know for DMS. This is for Database Migration Service. Remember, you need to use SCT whenever you have two different kinds of databases that you're migrating from. 

![](/img/04/04.png)


#### AWS DMS – Multi-AZ Deployment

And for DMS, there is a multi-AZ deployment. So when you have this, then you have a DMS replication instance in one AZ, and then you have synchronous replication of that instance into another AZ, which is going to be a standby replica. So the benefits of using this is, of course, to be resilient to a failure in one specific AZ, but also gives you data redundancy, will eliminate IO freezes, and will minimize latency spikes. So that's it for this lecture. I hope you liked it. And I will see you in the next lecture.

* When Multi-AZ Enabled, DMS provisions and maintains a synchronously stand replica in a different AZ

* Advantages:
  * Provides Data Redundancy
  * Eliminates I/O freezes
  * Minimizes latency spikes

![](/img/04/05.png)


#### AWS Application Discovery Service (AWS DMS) - Hands On

![](/img/04/06.png)


Let's take a look at the different options offered by the `AWS Database Migration Service (DMS)**. We'll go through the steps to figure out the right type of migration for your needs.

**1. Discovering NSS with DMS Fleet Advisor**
The first option is to `discover your on-premises database inventory` using AWS DMS `Fleet Advisor`. This tool analyzes your databases and identifies potential migration paths, providing results in hours. It saves you a lot of planning time by avoiding the need for third-party tools or hiring migration experts.

**1. Schema Conversion with DMS SCT**
The second option is to `convert database schemas`. Using the `DMS Schema Conversion Tool (SCT)`, you can convert schemas from one database technology to another. This is useful to understand the types of transformations needed for your migration.

**3. Migration Options**
Next, we have the actual migration of your data, which includes `two types of migrations`:
   - `Homogeneous Data Migration`: This type involves migrating between the same database technologies (e.g., Oracle to Oracle). For these migrations, you might use `native database tools` for faster results.
   - `Heterogeneous Data Migration`: This type is used when migrating between different database technologies (e.g., Oracle to Aurora or PostgreSQL). 

In the case of heterogeneous migrations, you can choose between:
   - `Serverless Replication`: Easier to manage since no resources need to be managed. However, it doesn't support all database engines.
   - `Instance-based Migration`: An `EC2 instance` is used behind the scenes for the migration. The EC2 instance is managed by DMS, which performs the migration tasks. This is the recommended option if serverless replication doesn’t support your database technology.

So I'm going to show you instance-based migrations, in which we're going to create an application instance and just look at the options.

![](/img/04/07.png)

So you would give it a name, then you would give it a description, and then you configure the instance. So of course, based on how much data has to be transiting, you would need to size up your instance. So you have different options from T3 micro all the way to really large instances. And it's really up to you to see what is going to be necessary based on what load you are getting. You choose a DMS version and see the DMS release notes to see what are the new updates given by different engine versions. And then you choose high availability. So if you're doing a production workload of replication, it's usually better to be in multiple availability zones in case one of these AZ goes down. If you're doing a very simple migration in dev or test, then single AZ may be sufficient. So we use multi-AZ. And sometimes you get restrictions around the type of database class you need. So let's just go for single AZ to get no warning. Then how much storage you want on your EC2 instance. And then the connectivity and security. So the IP, the VPC, the subnet group, whether or not you want your instance to be publicly accessible. And then you have more advanced settings if you wanted to. And maintenance settings to do maintenance on the operating system of your instance. When you're good, you click on create replication instance. But we're not going to do this because this would cost us money.

![](/img/04/08.png)

![](/img/04/09.png)

![](/img/04/10.png)

And we don't have databases anyway to migrate to and from. So just cancel this. But I just want to show you the options.

![](/img/04/11.png)
So once you have chosen a replication instance, on the left-hand side, we can create what's called an endpoint. So endpoints allow you to define where your databases are. So you can create an endpoint. And it can be a source endpoint or a target endpoint. If it's a database in RDS, you have an easy selector by choosing this option. If not, then you can set up the endpoint identifier, then the source engine type. So you have all of these options in here for source engine. And if you choose target, you have all of these options right here as well. And then you would choose the endpoint settings. So either a wizard to get guided to connect or an editor and to provide it as a JSON format. So these are very specific to your database. And this is why I don't go deep into it. But they provide information on how to connect to your source or your target database. So then you can test your connection from a specific replication instance as well. 

![](/img/04/12.png)

So when you have your source and your target defined, then you would go into `database migration task`.

![](/img/04/13.png)

And this is where you can create a task and say, hey, here is my replication instance I want to use. Here is my source database endpoint. Here is my target database endpoint. And what is my migration type? So here we can migrate existing data. Or we can also migrate the existing data and replicate ongoing changes. So that's for continuous data replication. Or just do the replication of data changes only. Again, we can have a task setting. So what settings do we want? We use wizard and we choose all these settings. Or we just specify it as JSON to see everything that can be configured for your DMS task. And as you can see, that is a lot of settings. This is why maybe JSON editor may be better at some point. Okay. Then you can do an assessment and do some configuration and then create your task. Obviously, we're not doing any of this because this would cost us some money. But you get the idea. 

![](/img/04/14.png)

![](/img/04/15.png)

But you get the idea. You create a replication instance, then endpoints, and then finally a database migration task. 

And DMS is evolving towards `serverless replication`, where you can just create a replication directly from database to database without managing any replication instance. And then you have more features around schema conversion and fleet advisor recommendations. And as well as homogeneous data migrations to leverage the 90th database tooling to perform these migrations. So that's it for DMS and its overview. That should be enough for the exam.

### AWS DataSync

So now let's talk about AWS DataSync. And DataSync is a service that is appearing now quite a lot at the exam. It's a very simple one, but you need to know what it does at its core. So the idea, as the name indicates, is to synchronize data. Therefore, move large amounts of data to and from places. And these places can be, for example, on-premises or other cloud locations into AWS. So you would connect to your server using the MFS, the SMP, the HDFS, or other protocols. And it needs an agent to run on-premises or on the other cloud to do that connection. And you can, for example, do other types of migrations. For example, you can move data from one AWS service to another AWS service. And this requires no agent. I will show you what it means. So the idea is that you can synchronize data to Amazon S3, including any storage classes, even Glitch here, Amazon EFS to store into your network file system, or Amazon FSx, and it supports all of them. Now, the replication tasks are not continuous. They are scheduled. So you can make DataSync run hourly, daily, or weekly. So there's a lag, okay, but the data is going to be synchronized on a schedule. On top of it, DataSync has the ability to keep the file permissions and the metadata. That means the security and so on. So that means that it's compliant with the NFS POSIX file system and the SMB permissions. This is very important because at the exam, this will be the unique option that will preserve the metadata of your file when moving them from one location to another. One DataSync agent can be quite powerful. It can run one task, can use up to 10 gigabits of data per second, although if you don't want to max out your network, you can set up a bandwidth limit. So let's have a look at what it means in the diagram. 

* Move large amount of data to and from
  * On-premises / other cloud to AWS (NFS, SMB, HDFS, S3 API…) – needs agent
  * AWS to AWS (different storage services) – no agent needed
* Can synchronize to:
  * Amazon S3 (any storage classes – including Glacier)
  * Amazon EFS
  * Amazon FSx (Windows, Lustre, NetApp, OpenZFS...)
* Replication tasks can be scheduled hourly, daily, weekly
* **File permissions and metadata are preserved** (NFS POSIX, SMB…)
* One agent task can use 10 Gbps, can setup a bandwidth limit


**NFS / SMB to AWS (S3, EFS, FSx…)**

![](/img/04/16.png)

So here is the use case of synchronizing your on-premises files using the SMB or NFS protocol into AWS, and that could be S3, EFS, or FSx. So you have your on-premises and then your AWS region where DataSync is running. So here is your NFS or SMB server, and what you have to do is to install on-premises the AWS DataSync agent, and you will tell it to connect to your NFS or SMB server, and then the DataSync agent will establish a connection and also connect in an encrypted fashion into the DataSync service. From there, you can tell it to go wherever you want. There could be any storage class for your Amazon S3 buckets, or it could be AWS EFS, or it could be Amazon FSx. And the synchronization can happen one way, from on-premises to AWS, but you can also synchronize from AWS back into on-premises. This is why it's called DataSync. It can work any way. Now, sometimes at exam, we will tell you that we want to use DataSync, but we don't have the network capacity to do so. Therefore, what you have to think about is to use the AWS Snowcone device specifically, because the Snowcone device comes with the DataSync agent pre-installed on it. So you can run Snowcone on-premises, then it will pull your data, run the DataSync agent, then it will be shipped back into your AWS region, and then synchronize the data to the storage resources of AWS. So that is to show the architecture of synchronization from on-premises to AWS, or it could be, for example, another cloud to AWS using the DataSync agent.

**Transfer between AWS storage services**

![](/img/04/17.png)

But you can also use DataSync to just synchronize between different AWS storage services. For example, you want to synchronize between Amazon S3, or Amazon EFS, or Amazon FSx, back into Amazon S3, Amazon EFS, or Amazon FSx. And for this, again, we will use the AWS DataSync service, and it will copy the data, of course, but also the metadata will be kept between the different AWS storage services, which is very important, and again, something that can come up in the exam. So to remind you, DataSync can pretty much synchronize between anything, but it is not continuous, it is scheduled tasks that can be happening hourly, daily, weekly, and also it will preserve metadata and your file permissions. And finally, you need to run the DataSync agent if you are connecting to an NFS or SMB server. Okay, that's it for this lecture, I hope you liked it, and I will see you in the next lecture.

### AWS Snow Family

* Highly-secure, portable devices to collect and process data
at the edge, and migrate data into and out of AWS

![](/img/04/20.png)

So now let's talk about the AWS Snow family. So this is a highly secure portable device used to collect and process data at the edge and migrate data in and out of AWS. So we have two kinds. We have the Snow Cone. It's a small device. And we have the Snowball Edge. It's a much bigger device. So each have their own specificity. So the Snow Cone is for very small storage capacity. You have the option between 8 to 14 terabytes. And usually you use the Snow Cone when you have a very small migration size of up to terabytes. Snowball Edge is a much bigger device. You have different flavors of it. But the storage capacity can go, for example, from 80 terabytes to 210 terabytes. And you want to use them when you have migration size up to petabytes, where you order many, many different Snowball Edge devices.

**Data Migrations with AWS Snow Family**

![](/img/04/18.png)

So why do we need Snowball devices in the first place? Well, let's talk about data migrations. So here is a table of how long it takes to transfer 10 terabytes or 100 terabytes or 1 petabyte based on your connection speed. So, for example, if you want to transfer 100 terabytes and you have a 1 gigabit per second connection, then it would take you 12 days. So obviously, this would also take you all your bandwidth from your company. So when you have a limited connectivity or limited bandwidth, or your network cost can be very high, or, for example, you can't maximize your lives because you are sharing the bandwidth, or you have issues with connection stability, then you want to use the AWS Snow family, because they're offline devices and they allow you to perform data migrations. So the rule of thumb, if it takes you more than a week to transfer data over the network, then you should use Snowball devices. 

**Diagrams**

![](/img/04/19.png)

So how does that work? Well, this is a direct upload from directly our clients into Amazon S3. But with the Snow family, we order a Snowball or a Snow Cone device. It gets delivered to us. Then we load the data onto it. We ship it back to AWS. And AWS is going to import it directly into your Amazon S3 bucket. So this is an offline data transfer. 

**Snow Family – Usage Process**

1. Request Snowball devices from the AWS console for delivery
2. Install the snowball client / AWS OpsHub on your servers
3. Connect the snowball to your servers and copy files using the client
4. Ship back the device when you’re done (goes to the right AWS facility)
5. Data will be loaded into an S3 bucket
6. Snowball is completely wiped

So we first request the Snowball device from the AWS console for delivery. Then we install the Snowball client or something called AWS Ops Hub on our servers to transfer data. Then we connect the Snowball device to our server and we start copying files in the client. And then we ship back the device when we're ready. It goes directly into an AWS facility. The data is going to be loaded onto an Amazon S3 bucket. And then your Snowball is completely wiped and can be sent to another customer. So this is for the data migration process. But you also have edge computing. 

**What is Edge Computing?**
* Process data while it’s being created on an edge location
* A truck on the road, a ship on the sea, a mining station underground...
* These locations may have limited internet and no access to computing power
* We setup a Snowball Edge / Snowcone device to do edge computing
* Snowcone: 2 CPUs, 4 GB of memory, wired or wireless access
* Snowball Edge Compute Optimized (dedicated for that use case) & Storage Optimized
* Run EC2 Instances or Lambda functions at the edge
* Use cases: preprocess data, machine learning, transcoding media

So what is edge computing? Well, this is to process data created on an edge location. Which means it could be a truck on the road or a ship on the sea or a mining station on the ground. In these locations, they may have limited or no internet. And they may have no access to computing power. So here, again, we order a Snowball Edge device or a Snow Cone device. And then we do edge computing. Snow Cone comes with a very, very simple CPU and very little memory. But it still can be helpful for some use cases. And then on Snowball Edge, you have the compute-optimized instance, which is dedicated for that use case, which has a lot of processing power. Or the storage-optimized also has some processing power. And when you have a Snowball Edge, for example, device, you can run EC2 instances or Lambda functions directly on your Snowball Edge device directly at the edge. So the use cases for edge computing is to pre-process data where it's created. Or to do machine learning, again, on data where it's created. Or to transcode via what is being shipped back to AWS. So that's it. We've seen Snow Cone and Snowball Edge devices. They're used for data migration and edge computing. I hope you liked it. And I will see you in the next lecture.

#### AWS Snow Family - Hands on

![](/img/04/21.png)

So let's have a look at the AWS Snow Family service. So from the console, you can actually order a Snow Family device, and you have to name a job, for example, DemoJob. So we need to choose a job type so we can import data into Amazon S3, which is the most common use case. We can also export data from Amazon S3. You can order a Snow device just for local compute and storage only. This gives you some kind of device, that server that can run in a remote place without being connected to the internet. Or you can import virtual types into the AWS Storage Gateway. So we'll choose the first option, Import into Amazon S3.

![](/img/04/22.png)

And here we have several options for Snow devices. So we have Snowcone, Snowcone SSD, Snowball Edge Storage Optimized with 80TB, and then we have the Compute Optimized and the Compute Optimized with GPUs. So it's possible that AWS will add options over time that may not re-record this video, so that's fine. I always cover what's at the exam, so don't worry. And so, as you can see, we have several options, and we see in the pre-lecture the use case for each option, so I won't go over them. But this is a nice way to know what you're getting. Okay, so let's say we go, for example, with the Compute Optimized Snowball Edge. 

![](/img/04/23.png)

Then we need to choose a pricing option. So we have on-demand pricing, multi-pricing, or then commitment for one or three-year period. Then we have the storage type, so it's an S3 data transfer. And then what AMI do we want to use on it? So this is a compute type of instance, so we can actually load an AMI, for example, this Amazon Linux 2 for Snow family. You can create your own AMI to have your own compute needs. Then where do you want to load the data onto? So what S3 buckets are available to you? I have this one right here, but you can create your own S3 buckets. 

![](/img/04/24.png)

Next, we have several features and options we can use. So we have IoT Greengrass for Snow to have IoT capability on your Snow device, but it's not needed. And then we can do remote device management by using OpsHub or the Snowball client. So I'll click on Next.

![](/img/04/25.png)

Then what type of encryption do we want on our Snowball device? So we have this encryption key right here, where you can create your own KMS key and encrypt it with that. Then you need to grant access, of course, to Amazon S3 and SNS to publish actions and to send data. So you need to create the service role. And then you need to add an address, so where you want to ship it, which country, which state, and so on, as well as a shipping speed. And the SNS notification for your job status changes. And then you'll get the job summary. So that's it. You've seen all the options for Snow devices and the Snow family. 

![](/img/04/26.png)

![](/img/04/27.png)

Remember, it's most commonly used to send data into Amazon S3 and out of Amazon S3. And it gives you remote compute capabilities. We've seen also the different kind of devices, which are Snowcode, Snowball, and Snowmobile. All right, that's it. 


### AWS Transfer Family

* A fully-managed service for file transfers into and out of Amazon S3 or Amazon EFS using the FTP protocol
* Supported Protocols
  * AWS Transfer for FTP (File Transfer Protocol (FTP))
  * AWS Transfer for FTPS (File Transfer Protocol over SSL (FTPS))
  * AWS Transfer for SFTP (Secure File Transfer Protocol (SFTP))
* Managed infrastructure, Scalable, Reliable, Highly Available (multi-AZ)
* Pay per provisioned endpoint per hour + data transfers in GB
* Store and manage users’ credentials within the service
* Integrate with existing authentication systems (Microsoft Active Directory, LDAP, Okta, Amazon Cognito, custom)
* Usage: sharing files, public datasets, CRM, ERP, …


![](/img/04/28.png)

Now let's talk about the AWS Transfer Family. So the idea is that you want to send data in and out of Amazon S3 or EFS, but you don't want to use the S3 APIs, you don't want to use the EFS network file system, you just want to use the FTP protocol. In this case, you need to use the Transfer Family service from AWS. So it supports three kinds of protocols. It supports AWS Transfer for FTP, so the File Transfer Protocol, FTP. FTP-S, which is the File Transfer Protocol over SSL, so encrypted form. Or SFTP, which is the Secure File Transfer Protocol. Now you want to be an expert on those, just know that FTP is unencrypted, whereas FTP-S and SFTP are encrypted in flight. Now the idea is that using the FTP protocol, you can upload to S3 or EFS. The Transfer Family is a fully managed infrastructure. It's scalable, reliable, and highly available, so you don't have to manage all that capability. And in terms of pricing, you're going to pay per provision and points per hour, plus a fee per gigabyte of data transferred in and out of the Transfer Family. You can store and manage the user's credentials for that service within the service, or you can also integrate with existing authentication systems, such as Microsoft Active Directory, LDAP, OCTA, Amazon Cognito, or any custom source. The usage of this is obviously to have an FTP interface into Amazon S3 or EFS, so as to share files, to share public data sets, to do CRM, ERP, etc., etc. So just a diagram for you to understand, the Transfer Family has three flavors, and the users can access directly using the endpoints out of the FTP, or optionally you can use a DNS called Route 53 to provide your own hostname into the FTP service. And then the FTP service, so the transfer for FTP service will have an IAM role that will be assumed to send or issue the files from Amazon S3 or Amazon EFS, and this is done transparently, you don't have to set many things up. And finally, if you wanted to secure the Transfer Family services, then you could authenticate your users using an external authentication system, such as Active Directory, LDAP, or all the things I just said in this previous slide. So that's it, just so you know this feature at a high level. I hope you liked it, and I will see you in the next lecture.

### Compute

An important part of data processing is the computing resources allocated to it. Although data processing is going to be covered in the context of other sections in this course as well, understanding the basics of Amazon EC2, AWS Lambda, AWS Serverless Application Model, and AWS Batch are things you need to know. Stefan and I will be switching off on different topics within this section. I'm going to assume that you know the basics of EC2 already and just focus on how it fits into data engineering here. Although the data engineering exam is associate-level, it's definitely a more advanced associate-level exam, and I'm assuming you at least have cloud practitioner-level knowledge coming into this. So, let's dive into the computing services you need to know about for the exam.

#### EC2 in Big Data

So there's one service we've been using without really knowing it is EC2. And EC2 has a good role in big data, especially behind the launch modes. So we know that on-demand, spot, and reserved instances. So let's do a quick review again. So spot means that it's going to be a very, very cheap instance, but AWS can take it away from you at any time. So you need to be able to tolerate lots of instances. That means usually if you do big data with spot instances, there needs to be some sort of checkpointing features. So machine learning algorithm, Spark, Hive, all these things are meant to tolerate loss, so they would be great candidates to run your big data processing jobs or your machine learning jobs on spot instances. Similarly, if you have a very long-running cluster or database, like an EMR cluster running for over a year or RDS database running for over a year, then you need to start reserving your instances because by reserving your instances ahead of time, you're going to get a tremendous discount. So this is for stuff that's going to be very stable over time.

* On demand, Spot & Reserved instances:
  * Spot: can tolerate loss, low cost => checkpointing feature (ML, etc)
  * Reserved: long running clusters, databases (over a year)
  * On demand: remaining workloads
* Auto Scaling:
  * Leverage for EMR, etc
  * Automated for DynamoDB, Auto Scaling Groups, etc…
* EC2 is behind EMR


#### EC2 Graviton-based instances 

Quick note about AWS Graviton, kind of a big deal within Amazon, because it's their own processor, their own family of processors that they came up with. And this powers several EC2 instance types that you might want to know about. There are general-purpose, compute-optimized, memory-optimized, storage-optimized, and even an accelerated computing family that's used for Android game streaming in particular, and also for machine learning inference. I wouldn't worry too much about the specific letters and numbers here. It wouldn't hurt to know them, but I wouldn't worry about it too much. But they do brag about this offering the best price-performance ratio. Of course, this does come at the expense of running on a proprietary processor type, and you need special software that will run on it, right? So it can't just run anything. However, a lot of the data engineering services that are covered in this course can run on AWS Graviton instances. So internally, their own services can run on it, and Graviton can be a very cost-effective choice for what kind of instance type to run it on. For example, MSK, RDS, MemoryDB, ElastiCache, OpenSearch, EMR, Lambda, and Fargate are all available to be deployed on AWS Graviton-based instances, the ones listed above. So just keep in mind, Graviton is Amazon's own family of processors. It offers very good price-performance ratios, and a lot of the data engineering services that we've listed here can run on top of them with no extra effort on your part.

* Amazon’s own family of processors, powers several EC2 instance types
  * General purpose: M7, T4
  * Compute optimized: C7, C6
  * Memory optimized: R7, X2
  * Storage optimized: Im4, Is4
  * Accelerated computing (game streaming, ML inference): G5
* Offers best price performance
* Option for many data engineering services
  * MSK, RDS, MemoryDB, ElastiCache, OpenSearch, EMR, Lambda, Fargate

### AWS Lambda

**Serverless data processing**

#### What is Lambda?

* A way to run code snippets “in the cloud”
  * Serverless
  * Continuous scaling
* Often used to process data as it’s moved around
  

AWS Lambda. Lambda is a serverless data processing tool that you can use. Let's talk about what that means. So what is Lambda? Basically, it's a way to run little snippets of code in the cloud. So if you have a little bit of code in pretty much any language you can imagine, Lambda can run that for you without you having to worry about what servers to run it on. So you don't have to go and provision a bunch of EC2 servers to run your code. Lambda does that for you. It thinks about the actual execution of your code. You just think about what the code itself does. So it is a serverless method of actually running little bits of code that will scale out continuously without you having to do anything. Lambda will automatically scale out the hardware that it's running on as needed, depending on how much data is coming into it. So you can see how this fits into a big data world. If you have a lot of data flowing in, Lambda will automatically scale itself up and down to handle the processing of that data dynamically. In the context of big data, it's often used to process data as it's moved around from one service to another. So some services don't talk directly to other services in AWS, but Lambda can be used as a glue between anything pretty much. So it can sit there and get triggered by some other service sending data into it, like a Kinesis data stream, reformat that information into a format required by some other service, send that data to another service for further processing, and maybe retrieve that data and send it back. So Lambda is just a way of running little stateless bits of code in the cloud that is often used to just glue different services together in big data. 

**Example: Serverless Website**

![](/img/04/29.png)

Let's look at an example. It's not always used for big data. A very common use of Lambda is in use of a serverless website. And it's actually possible to build a website without having any servers that you manage at all. This is called a serverless website, and you can do this with AWS Lambda. Now you normally can't do this with a really dynamic website, but if you can build your website by just having static HTML and AJAX calls embedded within that HTML, well, you can serve that from S3 or something, right? And then the AJAX calls are all you need to deal with. So maybe you have an API gateway in Amazon that sort of serves as the wall between the outside clients and you, the interior of your system there. So let's say, for example, you have a website where someone needs to log in. That login request might go through the API gateway, which in turn would then get sent off to AWS Lambda. Lambda would say, okay, the website wants this person to log in. It could turn around and craft a request to Amazon Cognito to say, do you authenticate this user or not? Amazon Cognito will come back and say, sure, here's your token. Lambda can then format that result and send it back to the website. So Lambda is sort of the glue there between the website API and Cognito on the back end. Similarly, let's say that we're building like a chat application on our front end there. Maybe we need to retrieve the chat history for that user ID once they've logged in. So again, the API gateway would receive that request. Lambda would be triggered by that API request and say, okay, I need to go get the chat history for this user ID. How do I craft that request to DynamoDB? It would then turn around, craft that request to DynamoDB for that chat history, talk to DynamoDB to get that, and then send it back through the API gateway back to the website. So here you see Lambda often fills a role as sort of the glue between different services. 

**Example: Order history app**

![](/img/04/30.png)

That's exactly how we're going to use it in our own application here. Very soon we're going to build this out. You'll recall that in our order history app, previously we used a Kinesis consumer application running on an EC2 host. This would serve as the glue between Kinesis data streams and Amazon DynamoDB. Now we don't want to have to manage EC2 servers for such a simple task as that. We can use Lambda instead. That's a much better choice. So we're going to build a Lambda function that actually sits between our data stream that you see in server logs coming in and between DynamoDB where we want to store that data longer term. Lambda's just going to sit there waiting for triggers of events from the data stream, and each trigger event will actually have a batch of events that needs to go through and extract each individual record and then turn around and write that into DynamoDB. So again, Lambda is just the glue between the data stream and some other service, in this case DynamoDB.

**Example: Transaction rate alarm**

![](/img/04/31.png)

Later in the course we'll build out a transaction rate alarm. Its only purpose is to notify us when something unusual is happening on our system. So in this case we have a Kinesis data stream that's receiving events that says something weird is going on that requires someone's attention. Lambda's going to be triggered by those data stream events, and it will then turn around and craft an SMS request to actually send out a message to your cell phone notifying you that something requires your attention. So yet again, Lambda's the glue between two different services that don't talk directly to each other, but because Lambda's just code, it can do anything. It can transform that data in any way, it can talk to any service on the backend you can imagine, it can do whatever you want. So it's sort of the magic glue that enables you to put different components together in creative ways, and this is just one example of that.


#### Lambda Integration - Part 1


**Why not just run a server?**
* Server management (patches, monitoring, hardware failures, etc.)
* Servers can be cheap, but scaling gets expensive really fast
* You don’t pay for processing time you don’t use
* Easier to split up development between front-end and back-end

So what's the big deal about serverless processing of your code? Why not just run it on a server? Well, I mean, even though Lambda is still running on servers under the hood, they're not servers that you manage, and that's a big deal. So Amazon has people at pagers to keep those servers working and dealing with all the patches and monitoring and hardware failures and stuff, so you don't have to. You don't think about that at all when you're running AWS Lambda. All you think about is the code that you have running on it. And yes, even though individual servers can be cheap, scaling those servers out can get very expensive very fast. And if you scale out a fleet to run your function to manage your peak capacity, you're going to end up paying for a lot of processing time that you're not even using when you're off peak. With Lambda, you only pay for the processing time that you're actually consuming, so that can be huge from a cost-saving standpoint. And although it's not so big of a deal in the big data world, if you're dealing with something like a serverless website, it does make it easier to split up your development between front-end development and back-end development. So all the back-end work can happen in your Lambda functions while your front-end developers worry about all the static stuff on the front-end client website. 

**Main uses of Lambda**
* Real-time file processing
* Real-time stream processing
* ETL
* Cron replacement
* Process AWS events

So there are several main uses of Lambda. One is real-time file processing. So as new data is coming into S3 or some other destination within AWS, Lambda can be triggered and actually process that file however you want to. And that can include doing some rudimentary ETL, that's extract, transform, and load. So Lambda is just code. You can do whatever you want in there, and that includes doing ETL on incoming data. You can also do real-time stream processing, as we discussed, just listening for new events coming in on a Kinesis stream or a Kinesis Firehose stream. You can also use Lambda as a cron replacement. This is interesting. You can use time as a trigger for a Lambda function as well. So just like you can trigger events using cron on a Linux system, you can trigger off Lambda events on some fixed schedule as well. So if you need something to happen once a day or once an hour or once a minute or whatever schedule you want, you can actually set that up to call your Lambda function periodically. And that might come in handy for kicking off daily batch jobs or something like that. And also it can process arbitrary events from AWS services. As we'll see, there's a very long list of AWS services that can generate triggers to Lambda. 

**Supported languages**
* Node.js
* Python
* Java
* C#
* Go
* Powershell
* Ruby

Pretty much any language you want to develop in is supported by Lambda, which is great. Node.js, Python, Java, C Sharp, Go, PowerShell, Ruby. So you can process data and do whatever you want with it in pretty much whatever language you want. And this is huge because it means that any system that has an interface in any of these languages is fair game for your Lambda code. So remember, Lambda's code. And with code, anything is possible. So you don't have to just limit yourself to serving as a glue or a transformation between different AWS services. You can actually talk to other services outside of AWS if you want or other libraries even that might enable you to perform more interesting transformations on that data. So really you can use your imagination when you're using Lambda to do a lot of different stuff. It really makes a lot of things possible all without managing servers in the process, which is awesome.

**Lambda triggers**

![](/img/04/32.png)

Now if you are using Lambda to serve as sort of the glue between AWS services, there's a long list of services here that actually trigger Lambda events for you. So any of these services can fire off triggers to Lambda functions whenever something interesting happens from these services. And this is just a partial list of them, guys. So also Alexa, and there's also a manual indication you can do with Lambda functions as well. Let's call out a few of the more common ones that are relevant to big data, though. And that would be S3, Kinesis, DynoDB, SMS and SQS, and IoT. So you can integrate Lambda with S3. So whenever a predefined thing happens to an object within S3, that can act as an event data that will invoke a Lambda function. So for example, a new object is created in S3. You might have a Lambda trigger that picks up the creation of that object, turns around, parses out that information, and sticks it in Redshift or something. Same story with DynoDB. Every time something is changed in a DynoDB table, that can trigger event data that invokes a Lambda function as well. And that allows for real-time event-driven data processing for the data arriving in DynoDB tables. You can integrate Lambda with Kinesis streams, and then the Lambda function can read records from a stream and be processed accordingly. Now under the hood, Lambda is actually polling the Kinesis stream. The stream is not pushing data into Lambda. That can be an important distinction in some contexts there. So remember, when you have Kinesis streams talking to Lambda, Kinesis isn't actually pushing that data into Lambda, as the architectural diagrams might suggest. Lambda actually polls that stream periodically and collects information from it in a batch manner. You can also integrate Lambda with IoT. So when some device starts sending data to the IoT service, that can then invoke Lambda and process it accordingly, as defined in your Lambda function. And it integrates with Kinesis spirals. In that case, Lambda can transform the data and deliver that transformed data to S3 or Redshift or Elasticsearch or whatever you want. It can output to pretty much anything, because you can write code to do what you want in response to these triggers. All AWS services have APIs you can call, so really the sky's the limit as to what you can call downstream from your Lambda function. You can do anything you want. These are just ones that can trigger a Lambda function for you automatically. Now, as long as these services are all under the same account, you can set up IAM roles to allow Lambda to access them. So as long as all the services you're using are under that same account, pretty much Lambda can talk to them if you set up their proper IAM role.


**Lambda and Amazon Opensearch Service**

![](/img/04/33.png)

**Lambda and Data Pipeline**

![](/img/04/34.png)

**Lambda and Redshift**

![](/img/04/35.png)

**Lambda + Kinesis**

![](/img/04/36.png)