- [Data Engineering Fundamentals](#data-engineering-fundamentals)
  - [Types of Data (Structured, Unstructured, Semi-Structured](#types-of-data-structured-unstructured-semi-structured)
  - [Properties of Data (Volume / Velocity / Variety)](#properties-of-data-volume--velocity--variety)
  - [Data WareHouse vs Data Lakes (and Lakehouse)](#data-warehouse-vs-data-lakes-and-lakehouse)
    - [Data Warehouse](#data-warehouse)
    - [Data Lake](#data-lake)
    - [Comparing the two](#comparing-the-two)
    - [Choosing a Warehouse vs. a Lake](#choosing-a-warehouse-vs-a-lake)
  - [What is a "Data Mesh"?](#what-is-a-data-mesh)
  - [Managing and Orchestrating ETL Pipelines](#managing-and-orchestrating-etl-pipelines)
  - [Common Data Source and Data Formats](#common-data-source-and-data-formats)
    - [Data Sources](#data-sources)
    - [Common Data Formats](#common-data-formats)
  - [Quick Review of Data Modeling, Data Linage, Schema Evolution](#quick-review-of-data-modeling-data-linage-schema-evolution)
  - [Database Performance Optimization](#database-performance-optimization)
  - [Data Sampling Techniques](#data-sampling-techniques)
  - [Data Skew Mechanisms](#data-skew-mechanisms)
  - [Data Validation and Profiling](#data-validation-and-profiling)
  - [SQL Review](#sql-review)
    - [Aggregation](#aggregation)
    - [Grouping](#grouping)
    - [Nested grouping, sorting](#nested-grouping-sorting)
    - [Pivoting](#pivoting)
  - [JOIN´s](#joins)
  - [SQL Regular Expressions (a quick intro)](#sql-regular-expressions-a-quick-intro)




## Data Engineering Fundamentals

### Types of Data (Structured, Unstructured, Semi-Structured

So, once again, our focus in this section will be a bit unusual because we’re not primarily discussing AWS topics. Instead, we’re going back to review some fundamental concepts of data engineering that extend beyond AWS. The exam guide does indicate that you need to know some of these concepts, not just AWS services. Therefore, we won't cover everything about data engineering here, just the topics explicitly mentioned in the exam guide. One such topic is the three types of data, so let’s go over that. We have structured, unstructured, and semi-structured data, and understanding these concepts is key to succeeding on the exam. Let's review each one.

**structured data** 

* we mean data that is organized in a defined way, usually following a specific schema, and is typically found in `relational databases`. 
* This is data that has been organized into distinct columns with specific data types, and the relationships between these columns have already been thought through, making it easy to query. 
* Structured data is characterized by its ability to be easily queried; because of its defined structure, you can use SQL queries effectively since you already know the column names, data types, and what to expect. 
* It is organized into rows and columns and has a consistent structure without surprises—it has been cleaned up already. 
* Examples of structured data include 
  * `database tables` like those in Oracle, Redshift, MySQL, PostgreSQL, and others. 
  * A well-structured `CSV` file would also qualify as structured data, provided it maintains consistent columns and doesn’t have irregularities like varying numbers of columns per line.
  * An `Excel` spreadsheet is another example; although it’s possible to create an unstructured spreadsheet filled with random data, a typical Excel file organized into well-defined rows and columns would be considered structured.

**unstructured data**

* which lacks a predefined structure or schema. 
* It’s a collection of data that we need to understand and organize before we can query it. 
* Unstructured data cannot be queried directly without preprocessing to extract meaning and determine how to index it. 
* This type of data can come in various formats, such as 
  * raw text files without a fixed format—like processing all the text on Wikipedia, Reddit, or in a book, for example, for a full-text search or training a language model. In these cases, preprocessing is necessary to index the content. 
  * Other examples include videos, audio files, and images, which require extracting metadata since you can’t directly search these formats. For instance, with a video, you need to extract data like the transcript, creation date, length, creator, and topic. Similarly, emails and word processing documents, which are primarily raw text, need to be indexed to understand their content before you can query them.

**semi-structured data** (Somewhere in between these two). 

* This type of data isn’t as strictly organized as structured data but contains some level of structure, often in the form of tags, hierarchies, or other patterns. The structure exists, but it requires some effort to extract it. It might include elements that are tagged or categorized in some way. 
* Semi-structured data is more flexible than structured data but not as chaotic as unstructured data. You can mix different types of content to some extent, but there’s still enough structure to extract the needed information. 
* Examples include `XML` and `JSON` files, where different schemas can exist within the same document, but tags help identify the content. The schema may not be consistent, but it does exist, making it semi-structured. Email headers are another example; they might contain various pieces of information like dates, subjects, and the unstructured content in the body. `Log files` are a crucial example in data engineering—they might come in different formats, with inconsistent data across lines, requiring significant parsing to understand each line's content and structure.

In summary, semi-structured data has some inherent structure, but it might not be consistent throughout the entire document. That’s the essence of unstructured and semi-structured data, which are key concepts highlighted in the exam guide for data engineering fundamentals.



### Properties of Data (Volume / Velocity / Variety)

Now, the exam guide also requires you to understand the properties of data, known as the three Vs: Volume, Velocity, and Variety. You might come across references to a fourth V, "Veracity," in other materials, but the exam guide specifically focuses on the three, so we'll stick with Volume, Velocity, and Variety for now and set aside Veracity.

**Volume**

Volume refers to the amount of data you're dealing with at any given time. This could significantly impact your decisions on how to store and manage that data. 
* For example, you might be dealing with gigabytes, terabytes, or even petabytes of data, and the size will determine the challenges you'll face in storing, processing, and analyzing it. 
* If you need to transfer a large volume of data from an on-premises environment to AWS, the amount of data could influence your decision—whether to upload it via the Internet using the AWS Management Console or to use AWS Snowmobile for physical data transport. 
* For instance, a social media platform might generate terabytes of data daily from posts, images, and videos. Managing such a high volume requires scalable storage and processing solutions capable of handling large-scale queries and analyses. 
* Similarly, a large retailer might collect several years' worth of transaction data, potentially amounting to petabytes. Handling big data requires distributed systems capable of parallel processing, as opposed to a single, monolithic relational database, which might suffice for smaller datasets. 

Thus, Volume influences your choices when designing data engineering pipelines.

**Velocity**

Velocity pertains to the speed at which new data is generated, collected, and processed. It raises the question of whether you can handle the data in batches or if you need to process it in real time. 
* High-velocity data might require real-time or near-real-time processing capabilities—a distinction the exam often explores. This could determine whether you use Kinesis Data Streams or Kinesis Data Firehose, for instance. 
* Rapid ingestion and processing are crucial for specific applications, dictating whether you process data in batches at the end of the day or continuously in real time as it comes in. 
* Examples of high-velocity data include 
  * sensor readings from Internet of Things (IoT) devices, which might stream data every millisecond. While AWS seems to be reducing the emphasis on IoT in the exam, it still serves as an example of streaming data. 
  * A more relevant example could be high-frequency trading systems, where milliseconds matter, and real-time processing ensures the correct order of transactions and consistent data handling.

**Variety**

Variety relates to the types of data you're dealing with and ties back to our earlier discussion on data types. What format does the data take? Where does it come from? Is it structured, semi-structured, or unstructured? The data might originate from multiple sources, each with a unique format requiring different handling methods. 
* For example, a business might analyze structured data from relational databases, unstructured data from emails, and semi-structured data from JSON logs. How do you integrate these disparate data types? Or consider a healthcare system that collects data from electronic medical records (structured), health devices, and patient feedback forms (unstructured). 

The variety of data impacts your data storage strategies—possibly requiring multiple storage solutions while maintaining a unified query system across all data stores. This diversity necessitates a flexible approach to handling different data formats.

So, remember the three Vs —**Volume**, **Velocity**, and **Variety**— as they are key concepts for the exam.



### Data WareHouse vs Data Lakes (and Lakehouse)

Let's review data warehouses and data lakes, focusing on how they differ and when you might choose one over the other. We'll begin with data warehouses. Data warehouses represent a more traditional approach to data engineering. They serve as centralized repositories optimized for analysis, where data from various sources is stored in a structured format. The key point here is the structured format—meaning data is organized in a predefined way, much like a traditional relational database.

#### Data Warehouse

![](/img/01/03.png)

Data warehouses are designed for complex queries and analysis. The data stored in them is cleaned, transformed, and loaded using the ETL (Extract, Transform, Load) process. Typically, data warehouses employ a star or snowflake schema, which we’ll discuss further in the context of data modeling. They are optimized for read-heavy operations, meaning they are designed to handle large amounts of data from multiple sources and organize it in a way that allows for efficient querying for different applications.

For example, in AWS, Amazon Redshift is the primary data warehouse offering. While the exam won’t focus on other cloud providers, it's worth noting that similar technologies exist, such as Google BigQuery and Microsoft Azure SQL Data Warehouse. Back in the day, before Amazon Redshift was developed, Amazon relied on a massive Oracle database for this purpose, which was costly and difficult to maintain.

![](/img/01/01.png)

To illustrate the concept of a data warehouse, imagine we’re at Amazon, and we have a large data warehouse that ingests data from various sources. We might have a repository of clickstream data from raw server logs, purchase data showing what customers are buying, and catalog data detailing the items being purchased. These data sources are interconnected; for example, both purchase data and clickstream data would need to reference catalog data to understand what customers are clicking on and buying. All of this data is consolidated into a massive data warehouse, where different views can be created for various types of queries or applications. Different departments or applications might require specific subsets of data, with tailored indices and views. For instance, the accounting department might have access to data that other teams don’t need, while those analyzing customer behavior may have a different view. There could even be a separate data mart for machine learning purposes, allowing data scientists to extract consumer behavior data to train models, such as a recommendation engine.

#### Data Lake

![](/img/01/02.png)

Now, let's contrast this with a data lake. Unlike a data warehouse, a data lake is a large storage repository that holds vast amounts of raw data in its native format. This can include structured, semi-structured, and unstructured data. The approach here is to store all the data in one place without predefining a schema, allowing for the flexibility to determine how to use it later.

Data lakes allow for storing large volumes of raw data without requiring upfront processing or schema definition. Data is loaded "as is," with minimal preprocessing. This flexibility supports batch, real-time, or stream processing and allows for data transformation and exploration as needed. In AWS, Amazon S3 is commonly used as a data lake, where all logs and data can be stored. The structure can be extracted later using tools like AWS Glue, which can help define the schema for the unstructured data in S3. Once the schema is defined, tools such as Amazon Athena can use the Glue data catalog to understand and query the data. Importantly, the data remains in its raw format in S3; it's not replicated or moved into a relational database.

For alternative platforms, Azure Data Lake or HDFS within the Hadoop ecosystem serve similar purposes for storing data lake data.

#### Comparing the two
**Schema:**
* Data Warehouse: Schema-on-write (predefined schema before writing data)
  * Extract – Transform – Load (ETL)
* Data Lake: Schema-on-read (schema is defined at the time of reading data)
  * Extract – Load – Transform (ELT)

So again, on schema, the data warehouse is what we call schema on write. We know what our schema is, what our structure of that data is, before we write it to disk. So when we're processing data warehouse information, we will extract the data from its source, we will transform it into the schema that we know up front, and then we will load it into our database. Okay? As opposed to a data lake, which is schema on read. We don't know what that structure is until we go to read that raw data. So with a data lake, instead of ETL, we do ELT, extract, load, and transform. We extract the information from its source, we load it into S3 or wherever it's going, and then we transform it later on when we need to. We might have multiple kinds of transformations happening on that raw data for different types of application. But the data itself is stored in its raw format. That's the main difference. Data warehouse is storing structured, transformed data. A data lake is just storing the raw data in its original form.

**Data Types:**
Data warehouses, again, are more appropriate for structured data. When you know what you've got, data lakes can handle both structured and unstructured data. So don't care what you throw into S3, it can be anything. It's just an object store, right?

* Data Warehouse: Primarily structured data
* Data Lake: Both structured and unstructured data


**Agility:**
Data warehouses tend to be less flexible because you do have that predefined schema, and sometimes you change your mind on what that schema is. And when you do, that usually involves some big data migration project that involves a downtime of the data warehouse and very expensive people to make sure that it goes smoothly. As opposed to a data lake, since we're just taking raw data and we don't really know what the structure is or care up front, you can just keep on throwing more stuff in there and maybe change that schema over time as you go and deal with that at a layer above it all. So it's a little bit more agile that way.

* Data Warehouse: Less agile due to predefined schema
* Data Lake: More agile as it accepts raw data without a predefined structure


**Processing:**
Just to reinforce: data warehouses typically use an ETL pipeline where we extract information, transform it, and then load it into the system, into a database in its transformed state. As opposed to data lakes, which use ELT, where we're just loading stuff in in its raw form. Or maybe we're just loading stuff in there for storage purposes, and we'll figure out if we want to do stuff with it and analyze it later on. You know, we just want to hoard it away somewhere, maybe. That's still a data lake.

* Data Warehouse: ETL (Extract, Transform, Load)
* Data Lake: ELT (Extract, Load, Transform) or just Load for storage purposes


**Cost:**
Data warehouses tend to be more expensive because, well, A, it takes more people and more expensive tools to do all that transformation and structuring, and we need to optimize that more for complex queries. So a lot more thought needs to go into how that data is organized to ensure quick processing and quick querying later on. Data lakes can be more cost-effective. Things like S3 are pretty darn cheap. But if you do have large amounts of data, it can add up fast, of course, as with anything.

* Data Warehouse: Typically more expensive because of optimizations for complex queries
* Data Lake: Cost-effective storage solutions, but costs can rise when processing large amounts of data


#### Choosing a Warehouse vs. a Lake

How do you choose one over the other? 

**Use a Data Warehouse when:**
* You have structured data sources and require fast and complex queries.
* Data integration from different sources is essential.
* Business intelligence and analytics are the primary use
cases.

Just to recap, use a data warehouse if you have structured data sources, you know what that schema is up front, and you require fast and complex queries. So because you know that schema, you know what indices to build, you know how to optimize those queries up front. Data integration from different sources is essential, so your data warehouse typically ingests data from different sources. Otherwise, it's just a database. And business intelligence and analytics are the primary use cases of a data warehouse.

**Use a Data Lake when:**
Data lakes are going to be appropriate when you have a mixture of structured, semi-structured, or unstructured data. Maybe you need something more scalable, more cost-effective, because you have a massive amount of this data. And maybe you don't really know what you're going to be doing with this data in the future. I mean, I think that's usually the case, right? So that data lake brings more flexibility in storage and processing down the road. You know, you don't need to go and reload everything just because you want to analyze your data a different way. Data lakes are typically used with advanced analytics, machine learning, and data discovery systems. So it's very common to have a data lake of raw log data, raw behavior data, whatever it might be, extract that into features for machine learning and train a model, you know, stuff like that.
* You have a mix of structured, semi-structured, or
unstructured data.
* You need a scalable and cost-effective solution to store
massive amounts of data.
* Future needs for data are uncertain, and you want flexibility in storage and processing.
* Advanced analytics, machine learning, or data discovery are key goals.


* Often, organizations use a combination of both, ingesting raw data into a data lake and then processing and moving refined data into a data warehouse for analysis.

Sometimes, you use both, right? So it's okay to have both a data warehouse and a data lake. You might even have the same data in each for different applications, right? So maybe you store the raw data into a data lake and have more flexibility of what you do with it there. But if you want to do, you know, BI stuff and analytics, maybe you also load it into a data warehouse. That's more optimized for those use cases. So it's not a one or the other kind of choice, necessarily. It's more about having the right tool for the right job, right?

**Data Lakehouse**

One more option is called the data lakehouse. This concept was popularized by the Databricks community. Its definition is a hybrid data architecture that combines the best of both worlds, incorporating features of both data lakes and data warehouses. It aims to provide the performance, reliability, and capabilities of a data warehouse while maintaining the flexibility, scalability, and low-cost storage of a data lake. The data lakehouse supports both structured and unstructured data and allows for schema-on-write and schema-on-read. It provides capabilities for both detailed analytics and machine learning tasks and is typically built on cloud or distributed architectures such as AWS. It can also benefit from technologies like Delta Lake, which bring ACID transactions to big data. We'll talk more about ACID transactions later—they refer to the guarantees a database makes regarding how data is processed and handled.

Some examples:

* `AWS Lake Formation`, used together with S3 and Redshift Spectrum, is an example of a data lakehouse where you have a large amount of data stored in S3, but Redshift Spectrum allows you to query it as if it were a data warehouse, even though the underlying storage is not structured—it’s actually accessing S3. This hybrid approach combines the benefits of both data warehouses and data lakes. This seems to be the direction the industry is heading towards. 
  
* `Delta Lake` built on top of Apache Spark, 
* Databricks Lakehouse Platform, and Azure Synapse Analytics. While we won’t go into detail about those last three, as they're outside the scope of AWS, AWS Lake Formation with S3 and Redshift Spectrum is definitely a scenario you should pay attention to in this course.


### What is a "Data Mesh"?

![](/img/01/04.png)

Let's touch on a more recent buzzword in the world of data engineering, and that's a data mesh. The exam might expect you to know what this term means, at least, if not in more depth. It's not really about technology so much. It's more about organization and the management and how you structure access and ownership of data within a larger organization. So it's more about governance and organization than actual technologies.

The idea, though, is that individual teams own their own data. So if a given team or department or whatever it is in your organization is an expert in a certain type of data, or they collect that data, or that's their responsibility, they are responsible for maintaining that data and also offering it as a data product to other organizations, other teams, other departments within the larger organization. So it's sort of a decentralized world where data is owned by the teams that know the most about that data, but there's some central organizing principles on how that data is accessed and governed, right?

Now, outside of each data domain, which might be a specific team that owns a specific set of data products, there might be use cases around the organization that need to tap into that data. So maybe there's someone outside of that specific team that needs to do an analysis that involves data products from multiple data domains. That is a use case that might connect within multiple data domains to the data products that are offered by those domains. So people with a use case for a data analysis problem don't need to go and hit the raw data for everything; they can just hit these data products that are exposed by individual data domains. This is sometimes called domain-based data management.

And it's not too surprising that this has caught on and got some traction within Amazon because, well, this is how Amazon itself has been structured internally for as long as I remember. They've always had this decentralized approach where they have a set of governing principles, but individual teams own their own stuff for the most part.

However, you do need to have federated governance with central standards. So each one of these domains is responsible for maintaining the integrity and security of that data and making sure that access controls are in place as appropriate by applying central standards to it. And also Amazon, of course, loves this concept because AWS is a good solution for implementing a data mesh, right?

So you can imagine using something like Lake Formation to organize how this data is actually managed and stored and accessed, and also to manage the permissions to that different data from different groups, right? And maybe AWS Glue is used as a centralized data catalog so people can see what data products are out there.

It also calls for self-service tooling and infrastructure. So it assumes that these different domains are not left to fend for themselves entirely, that there is some sort of infrastructure in place for them to build these data products on top of. And again, that's where AWS might come in. Data lakes, data warehouses, S3, Glue, Lake Formation, whatever it might be, could be a part of a data mesh, but a data mesh is more about the data management paradigm, you know, the higher level concerns about who owns the data, who maintains it, how is that accessed, how is that distributed.

It does not prescribe a specific technology or architecture, but AWS could be one of them.


### Managing and Orchestrating ETL Pipelines

We talked about ETL in the context of data warehouses and extracting, transforming, and loading data into a data warehouse. Let's just dive more into what each one of those letters really means. So, again, ETL: extract, transform, and load. For a data lake, it would be ELT, just switching those last two around, but the letters mean the same thing in either case.

**Extract:**

E is for Extract: That's retrieving your raw data from wherever it comes from. We'll talk more about data sources in a moment, but that might be an external database, a customer relationship management system like Salesforce, flat files like a log file, or APIs from some other system entirely. Your data is probably coming from many places. You need to know how to interface with that information and extract it into your data warehouse. You also need to ensure data integrity while extracting. What happens if one of your API calls fails? Do you retry it? How often? What are your policies? It can get complex. You need to consider the velocity of that data too. Will you extract this data in real time, near real time, or in batches? This depends on your requirements and how the data is generated and queried.


  * Retrieve raw data from source systems, which can be databases, CRMs, flat files, APIs, or other data repositories.
  * Ensure data integrity during the extraction phase.
  * Can be done in real-time or in batches, depending on requirements.

**ETL Pipelines: Transform**

T is for Transform: This is where we convert the extracted raw data into a format suitable for our target data warehouse. With a data warehouse, this transformation is done upfront, while with a data lake, it might be done on demand later. Transforming data can involve several steps: cleansing data (removing errors, duplicates, or imputing missing values), enriching data (adding data from other sources), and changing formats (like converting date strings to date-time formats). You might also perform aggregations or computations, such as calculating totals or averages, or encode/decode data, possibly decrypting or decompressing it. Handling missing data is another common task—deciding whether to drop rows with missing values, impute them, or generate a report flagging the issues. All these decisions depend on the specific requirements.


* Convert the extracted data into a format suitable for the target data warehouse.
* Can involve various operations such as:
  * Data cleansing (e.g., removing duplicates, fixing errors)
  * Data enrichment (e.g., adding additional data from other sources)
  * Format changes (e.g., date formatting, string manipulation)
  * Aggregations or computations (e.g., calculating totals or averages)
  * Encoding or decoding data
  * Handling missing values


**ETL Pipelines: Load**

L is for Load: This involves moving your transformed data into the target data warehouse or repository. Loading can be done in batches or streamed in real time, depending on the volume and velocity of the data and the requirements for analysis. Just as with extraction, you need to ensure data integrity during loading. Disk writes can fail, or systems can become backed up; you need to account for these possibilities to avoid losing data during the load process.

* Move the transformed data into the target data warehouse or another data repository.
* Can be done in batches (all at once) or in a streaming manner (as data becomes available).
* Ensure that data maintains its integrity during the loading phase.


**Managing ETL Pipelines**

We need to manage these pipelines to ensure all steps occur in the right order and on the correct schedule. Automation of these pipelines is critical for reliability. AWS offers various services to support this, such as AWS Glue for automated ETL/ELT in response to data events, and orchestration services like EventBridge, Amazon Managed Workflows for Apache Airflow, AWS Step Functions, Lambda, and Glue workflows. We will explore these services in greater depth later in the course to understand their roles in the broader context of ETL pipelines.

* This process must be automated in some reliable way
* AWS Glue
* Orchestration services
  * EventBridge
  * Amazon Managed Workflows for Apache Airflow [Amazon MWAA]
  * AWS Step Functions
  * Lambda
  * Glue Workflows
* We’ll get into specific architectures later.


### Common Data Source and Data Formats

The sources of data and the formats that it might come in, this is a little bit less high-level, so I would expect the exam to expect you to know this stuff, even though it's not necessarily AWS-specific.

#### Data Sources

* JDBC
  * Java Database Connectivity
  * Platform-independent
  * Language-dependent
* ODBC
  * Open Database Connectivity
  * Platform-dependent (thx to drivers)
  * Language-independent
* Raw logs
* API’s
* Streams

One data source might be a `JDBC connection`, so maybe you have some external database out there that you need to extract data from. JDBC might be one common interface for accessing and querying that data as you need. It stands for Java Database Connectivity. The nice thing about JDBC is that it's platform-independent because it is based on Java, right? It's also language-dependent because, well, it's based on Java. So the good thing about Java is that you can run it on pretty much any device you can think of. The bad part is that it does require you to be using the Java language to access that data. So if you're building an extraction tool in Java, then JDBC makes a whole lot of sense. However, if you don't, if you're not in Java, you'll need some sort of intermediary layer instead. 

And that might be `ODBC, Open Database Connectivity`. It is platform-dependent because you need specific drivers to access your databases with ODBC. But it is language-independent because it is not just built exclusively on Java. Now, you'll find that in practice, most data extraction tools will support both JDBC and ODBC interfaces. It's just that that support will be at a layer that's a little bit lower and hidden from you, right? So we just need to know what those stand for and what their differences are.

You might also just be sucking in data from raw log files. You know, maybe you're just writing out logs to S3 and you need to ingest that as-is. That's okay. You might have some sort of an API to an external system. So maybe instead of JDBC or ODBC, there's some other interface, some application programming interface, that you can hit from your extraction application to access the information that you need.

Also, you might have streams of data coming in. So there's a lot of stuff that you can deal with streaming information. Kafka, Kinesis, we'll talk about that in more depth. But some data sources will provide you streams of data in real time as it's received, and you just need to have the right software to ingest those streams and do stuff with it.

#### Common Data Formats

**CSV (Comma-Separated Values)**

![](/img/01/05.png)

Some common data formats. This is important stuff, folks. CSV, you probably already know what that one is, a comma-separated value file. Basically, it's a text-based format. It's human-readable. That's the key here. Where data is in a tabular form, where every line of data corresponds to a row of data, and the values within are separated by commas. Now, it doesn't necessarily have to be a comma. Your delimiter can be pretty much anything. Using commas can get complicated if you have commas within the data itself, right? So then you have to figure out, oh, what are my rules for escaping this data and quoting it and whatnot? So you sometimes see CSV files where the delimiter is not a comma. It might be a pipe or a tab or something else, right? It's okay. You know, you might call it a TSV file if it's separated by a tab, but same idea.

When do you use this stuff? Mostly appropriate for small-to-medium-sized data sets because it is not encoded. It's human-readable, right? So it's not the most efficient way to store information, but it is easy to read. It's also a pretty common denominator for different systems, so pretty much anything can handle a CSV file. If you need to interchange your data between different systems, a CSV is a pretty good choice for a lingua franca, if you will, a way of transforming that data between anything. And if you do want it to be human-readable and editable, CSV has a huge advantage there, right? So it's very easy to just open up a CSV file in a text editor, read it, understand what's in there, and maybe even modify it directly without having to decode and encode it along the way. It's also useful for importing and exporting data from databases or spreadsheets. Again, the implication of a spreadsheet is it's probably not that much data, so a CSV is fine. Typically, you see this use in databases that are SQL-based, if you're ingesting data into a SQL database. Microsoft Excel makes it very easy to import CSVs and export CSVs. Pandas, if you're using a Python notebook, can handle CSVs quite easily. R and many ETL tools as well.


**JSON (JavaScript Object Notation)**

![](/img/01/06.png)

There's also JSON, JavaScript Object Notation. You see this a lot. More often, you see this in the world of application development for passing data between back-end services and front-end websites. But more generally, it's a lightweight text-based and human-readable data interchange format that represents structured or semi-structured data based on key-value pairs. So the main difference here is that it can be semi-structured. It's still human-readable, just like a CSV file, but you can have different kinds of information on different rows, right? So because everything is tagged into key-value pairs, you can have different key-value pairs within each row, and that's okay with JSON.

So this is, like I said, more commonly used between a web server and a web client as a way of exchanging information. It's kind of the default standard for that. Also, for configurations and settings files for software applications, you'll see that in JSON format quite a bit. And also just for use cases where you need a flexible schema or maybe nested data structures. That's not something you can do easily with CSV. But with JSON, you can have more layers of structure within each row, which is kind of a cool thing. You see this in web browsers. It's supported widely in many programming languages, JavaScript, Python, Java, RESTful APIs, and also NoSQL databases. So if you need to store semi-structured data, maybe this is the format that you're storing that semi-structured in your underlying database. Like MongoDB is a good fit for this sort of data.

**Avro**

* Description: Binary format that stores both the data and its schema, allowing it to be processed later with different systems without needing the original system's context.
* When to Use:
  * With big data and real-time processing systems.
  * When schema evolution (changes in data structure) is needed.
  * Efficient serialization for data transport between systems.
* Systems: Apache Kafka, Apache Spark, Apache Flink, Hadoop ecosystem.

Let's also talk about Avro. So we're getting out of the world of human-readable information here. Avro is a binary format. And this stores both the data and its schema, allowing it to be processed later with different systems without needing the original system's context. So it's a little package of both the data and its schema that's in binary format to make it nice and compact.

Appropriate for big data and real-time processing systems, its binary nature means that it's making more efficient use of the data of the bits that it has. If your schema is going to change, if your data structure might change over time, that might be a good reason for encoding the schema itself along with the data. If your schema does not change, then you're kind of wasting some space by encoding that schema alongside the data every time, right? So the real use case here is if that schema might change and you need to tie that schema closely to the data that it's attached to.

It is also good for efficient serialization for data transport between systems. So if you need to push data from point A to point B, encoding it and decoding it from Avro might be an efficient way to do so. Again, because it is binary in nature and because that information on how to decode it is baked into the format itself. Apache Kafka, Apache Spark, Apache Flink, and Hadoop are examples of systems that deal with Avro format quite easily. And we'll talk about all of those systems in depth later on.

**Parquet**

* Description: Columnar storage format optimized for analytics. Allows for efficient compression and encoding schemes.
* When to Use:
  * Analyzing large datasets with analytics engines.
  * Use cases where reading specific columns instead of entire records is beneficial.
  * Storing data on distributed systems where I/O operations and storage need optimization.
* Systems: Hadoop ecosystem, Apache Spark, Apache Hive, Apache Impala, Amazon Redshift Spectrum.

 this one comes up a lot. This is what we call a columnar storage format. So, because it is storing data by columns instead of rows, it's optimized for analytics, where your queries might only be looking at specific columns of information and not everything that's in a row, right? So, we're kind of flipping things on its head there. We're storing the information by its columns because I know that when I'm querying this data, I'm probably only going to be interested in specific columns and not the entire row.

So, that's an optimization for analytics, which is why RK is a very important part of the data analytics platform. This allows for efficient compression and encoding, again, because columns tend to be the same format, right? So, that allows us to compress it even better. This is useful, again, for analyzing large datasets within analytics engines.

The use case is where reading specific columns instead of entire records is beneficial. So, again, think about situations where you might have a lot of features, a lot of columns on each row, but a specific use case, a specific analysis, only needs a very small subset of those columns. That's where RK becomes very powerful.

It's also useful for storing data on distributed systems because I can split things out based on the columns that are being queried. So, maybe I store some columns on different servers, right? This can be a very important optimization where I'm trying to minimize IO overhead or storage overhead based on what I'm trying to do.

Examples that use Parquet: Hadoop again, Apache Spark again, Apache Hive, Apache Impala, and Redshift Spectrum, which is a very important piece of the data engineering and data analytics world on AWS. So, Parquet, remember that one? If you have a use case where you have lots of columns but maybe don't need to analyze them all, Parquet is a very efficient way of storing that information, querying that information, and also distributing the processing of that information based on the columns.


### Quick Review of Data Modeling, Data Linage, Schema Evolution

**A Very (intentionally) Incomplete Overview of Data Modeling**

![](/img/01/07.png)

Data Modeling next. Let's do a very quick and very intentionally incomplete overview of Data Modeling. The exam guide says, well, you need to know what data modeling is, but it doesn't talk about specific data models, so it doesn't really get into star schemas and snowflake schemas and stuff in that regard. So, not going to go into a ton of depth here, but just a brief review.

So, well, here's a star schema. Let's imagine that we have a database that's keeping track of information on some e-learning platform, like Udemy or something, right? Maybe you have a fact table at the center here that contains a course ID, a student ID, a payment ID, and the date/time that order was placed for every time a student enrolls in a given course.

Now, that fact table might then spread out to dimension tables, right? So, since we have these dimension tables hanging off that fact table, it has a bit of a star shape, which is why we call it a star schema. The course ID might be used to tie into a course dimension table that has the same course ID, and for a given course ID, we can look up more information about that course—such as the name, the description, the instructor, and the price of that course.

Maybe the student ID ties the fact table to a student dimension table, so given a student ID, you can look up in dim_student what the name of that student is, the address, the email, maybe their saved credit card. Finally, the payment ID might tie off to a payment dimension that contains the payment type, amount, tax collected, authorization codes from the payment processor, etc.

The idea is that we have these primary and foreign keys tying these dimension tables back to the fact tables, minimizing the amount of information we need to store with each enrollment and just storing the specifics in these external dimension tables. So, Data Modeling 101: we have fact tables and dimensions tied together by primary and foreign keys. We call this kind of diagram an entity relationship diagram (ERD), which might be something you want to remember.

**Data Lineage**

![](/img/01/08.png)

Let's also talk about the concept of data lineage. That might also show up on the exam. So, what do we mean by data lineage? It is a visual representation that traces the flow and transformation of data through its lifecycle, from its source to its final destination.

As a data engineer, you're going to be extracting data, transforming it, loading it into different places, and moving it all around while transforming it in various ways. The idea of data lineage is having some sort of record of what you did along the way. Why would you need that? Well, if there's an error along the way or something is transformed incorrectly, this record can help you trace that error back to its source and troubleshoot your pipeline.

It might also be required for compliance with regulations. If you're dealing with sensitive or secure data, laws might require you to keep detailed records of how it was transformed. More generally, data lineage provides a clear understanding of how data was moved, transformed, and consumed within systems. Documentation is a good thing, right? If someone new joins your team, this is an easy way for them to understand how your data is transformed and moved through your pipeline.

![](/img/01/09.png)

Here's an example of data lineage in the context of AWS. This example is from a blog article on aws.amazon.com, and I mention it because the exams often include questions based on obscure blog articles. In this example, they illustrate the creation of data lineage using a tool called spline agent, which is an Apache Spark tool attached to AWS Glue.

In this case, we're ingesting our data using AWS Glue from S3. Glue is cataloging the S3 raw data in the Glue data catalog and transforming it as part of an ETL pipeline. The spline agent is attached to Glue to capture what it's doing along the way. It has what's called a lineage API, which we can consume directly using spline's tools to generate visualizations.

Later on, we'll see another tool called SageMaker Lineage that does something similar. In this example, we're using this third-party tool, spline agent. Maybe you want to take that data and store it in your systems as well. In this case, we're using AWS Lambda to consume that information from spline and writing it into Amazon Neptune, which is a graph database. As data lineages often have a graph structure, a graph database is a logical place to store that information. You can then query Amazon Neptune using whatever external tools you prefer. This is one example of handling data lineage within AWS.


**Schema Evolution**

![](/img/01/10.png)

What do we mean by that? It's a term mentioned in the exam guide. Schema evolution refers to the ability to adapt and change the schema of a dataset over time without disrupting existing processes or systems.

We talked about this before as a strength of data lakes, where the format isn't baked in upfront. This ensures that data systems can adapt to changing business requirements, which happens a lot. You don't want to deal with data migrations every time a new requirement arises, and schema evolution allows you to add, remove, or modify columns and fields within a dataset as needed without disrupting existing systems.

This way, you maintain backward compatibility with older data records while introducing new data, fields, or modified fields over time without breaking the old stuff. That's what we mean by schema evolution—being able to change your schema over time without causing disruptions.

One example of this is the Glue Schema Registry. This tool handles schema discovery, compatibility, validation, and registration. Glue can manage different versions of your schema over time and ensure backward compatibility with older versions of your data, if necessary.

### Database Performance Optimization

![](/img/01/11.png)

Let's also do a quick review of database performance optimization techniques, some basic tools for making your databases perform better.

The number one tool is having the right indexes, right? You never want to have a full table scan if you can avoid it. If you need to search through every single row of your data looking for what you need, you're doing it wrong. That means you need an index and you need to look at what your queries are and make sure you understand what indexes you need to build to quickly access the data you need.

Indexing can also be a useful tool for enforcing data uniqueness and integrity. If you have conflicts when you're trying to index your data, that might alert you to problems that you really need to deal with that you might not have known about otherwise.

Another optimization technique is partitioning. If you have a large amount of data, you can reduce the amount of data scanned by partitioning it based on different things that you might be querying by. For example, you might have partitions by date. If you typically search for information from just the past month, partitioning your data by month can reduce the amount of information scanned if you need to do a full table scan for an entire month's worth of data.

Partitioning can also help with data lifecycle management. If you partition based on time, it's easy to move older partitions to less expensive storage solutions or delete them over time to save on storage costs, which might also help with compliance requirements. Partitioning can also enable parallel processing. If your data is partitioned logically, you might be able to process each partition in parallel, which can significantly improve performance.

Compression is another important optimization technique. Compression can speed up data transfer and reduce storage and disk reads. Often, databases are limited by disk I/O, and if that's the case, you want to make sure your data is stored in a compressed format. Common compression formats are GZIP, LZOP, BZIP2, and ZStandard. These formats are supported by Amazon Redshift and offer various trade-offs between compression efficiency and speed. You want to avoid shifting from being I/O-bound to CPU-bound because your compression format is too complex. There are compromises to consider.

There's also columnar compression. We talked about Parquet earlier and storing data in a columnar format. Compressing data in a columnar format can be more efficient because columns are in the same data format and may have similar structures. Compressing columnar data can be more efficient than compressing a row that contains various types of data.

So again, a quick review of database performance optimization: indexing, partitioning, and compression are your main tools to improve your database query performance.

### Data Sampling Techniques

![](/img/01/12.png)

This AM guide makes mention of data sampling, so let's do a quick review. There are two main types to know about. One is random sampling. The point of sampling, right, is to take a large dataset and create a smaller one from it. Maybe you just want to experiment or avoid the expense of dealing with a larger dataset and prefer a smaller, yet representative, sample dataset for analysis.

One way to do this is through random sampling. With random sampling, every item has an equal chance of being selected for your sample. You're essentially rolling the dice and selecting at random. This method works well if your data doesn't have distinct categories or classifications that you need to consider.

However, sometimes random sampling is not ideal, and that's where stratified sampling comes in. Imagine you have a large dataset with different classifications or categories that you care about. You want your sample dataset to capture examples from each of these different categories. For example, if you have white things, black things, and gray things in your dataset, random sampling might result in a sample that contains only white or gray things, but not enough black things. This is what the example on the right is illustrating.

The idea behind stratified sampling is to divide your population into homogenous subgroups, or strata. For instance, you look at the white, black, and gray items and then randomly sample from within each stratum. This ensures good representation from each subgroup, reducing the risk of missing any category from your sample. Stratified sampling ensures representation from each subgroup, which is crucial for many analyses today.

Other types of sampling techniques include systemic, cluster, convenience, and judgmental sampling. Systemic sampling is likely the only additional method you'll need to know for the exam. Let's look at some examples in more depth:

**Random Sampling**

Random sampling: Suppose we have a collection of purchase information represented by little wallets. To create our sample, we randomly pick three from this pile of data. These selected ones are colored red in our example. We chose them entirely at random, with no pattern or specific reasoning. That's random sampling.

![](/img/01/13.png)

**Stratified Sampling**

Stratified sampling: Now, imagine we want to focus on purchases of specific types of items. We have people who bought books, music (like CDs or digital downloads), home and garden items, and apparel. To ensure even representation of each category in our sample dataset, we might decide to select two from books, two from music, two from home and garden, and two from apparel. This selection doesn't necessarily represent the actual ratio of these categories in the full dataset, but it does ensure sufficient coverage of each category in our sample.

![](/img/01/14.png)

**Systemic Sampling**

Systemic sampling: The key difference with systemic sampling is that, instead of sampling randomly, you use a specific rule, such as picking every third item. In this example, we're selecting every third order: 1, 2, 3, then select; 1, 2, 3, then select; and so on. Every third item is picked, resulting in a systematic sample.

![](/img/01/15.png)

So, that's data sampling in a nutshell. It's a straightforward concept, but important to review for the exam.


### Data Skew Mechanisms

![](/img/01/16.png)

All right, all right, all right. I know you're itching to get into the AWS services. I'm trying to get you there as quickly as I can, but the exam guide does talk about data skew. You've got to know what that is, too, and that's not an AWS-specific thing.

What do we mean by data skew? Well, it can mean a few things, but usually it refers to unequal distribution across partitions. We talked about partitioning as a database optimization earlier, where we're storing different segments of our data in different physical storage or at different tables under the hood to make sure that we can process them more efficiently and in parallel. Skew is what happens when that partitioning is imbalanced.

So, we have an imbalance of data across the nodes or partitions in a computing system that's distributed somehow. Sometimes this is called the celebrity problem. Partitioning doesn't work if your traffic is uneven. You can't evenly partition data if your incoming traffic is uneven.

Let's take an example. Imagine that I'm IMDb, a database focused on actors and movies. I might take the actor ID to store information about each actor, hash it, and use that to distribute data across different partitions under the hood. That sounds reasonable, but some actors receive much more traffic than others. For example, Brad Pitt is likely to get more traffic than a lesser-known actor from a film made 30 years ago.

In this example, the celebrity problem arises because a celebrity like Brad Pitt can overload the partition he maps to, resulting in more traffic to that partition compared to others. The fundamental issue here is that traffic is uneven compared to the data distribution.

This can be caused by a non-uniform distribution of data. If we partition evenly based on a hash of some ID, it might not align with how that data is accessed when being read. The partitioning strategy might be inadequate, or there could be temporal skew.

Temporal skew occurs if data grows quickly over time and is partitioned by month, year, or another time frame. Data from five years ago might be much smaller than more recent data, resulting in more recent partitions being much larger than older ones. This is a common occurrence.

It's important to monitor your data's distribution and set up alerts when skew arises. If skew starts to become an issue, you need to monitor, be aware of it, and set alarms—CloudWatch and all that, right? We'll get into that later on in the course. But the core problem with data skew is that your data is accessed unevenly, contrary to the assumptions made during partitioning.

**Addressing Data Skew**

How do you fix it? There are a few ways. Now, I wouldn't worry about memorizing these different solutions for the exam, as they aren't directly from AWS documentation. But just so you're aware, here are some potential solutions:

**1.Adaptive Partitioning**: Dynamically adjust partitions based on observed distribution over time. Repartitioning in real time based on actual observations is possible. Load balancers often work this way.
**2.Salting**: Introduce a random factor or "salt" to the data to distribute it more uniformly.  
**3.Repartitioning**: This involves periodically redistributing data across partitions, but it can be very disruptive. Shuffling data around while trying to read it can get messy, so it's best to avoid this if possible.  
**4.Sampling**: Sample the data to determine the actual distribution and adjust processing accordingly. This could be part of your adaptive partitioning strategy.
**5.Custom Partitioning**: Define custom rules or functions for partitioning data based on domain knowledge. For example, knowing Brad Pitt will get a lot of traffic, you could assign him his own partition. It's a bit of a hack, but it's a solution.  

### Data Validation and Profiling

![](/img/01/17.png)

Let's also discuss some key concepts around data validation and data profiling to ensure that your data is good. This, again, is something that the exam guide calls out. There are a few different dimensions to consider for data validation.

One is data `completeness`. Do we have missing data? And if so, how are we dealing with it? Data completeness means checking whether all the expected data is present. Are any essential parts of our data missing? This is straightforward to check—you can easily compute how many fields are missing in a given column, what your null count is, and what percentage of your data that represents. Does this fall within your accepted requirements?

Data completeness is crucial. For example, imagine a dataset of people's salaries where you want to compute the average salary for a subset of individuals. If there are many zeros because people didn't report their salaries, that will significantly lower your average. If you're unaware of or not handling missing data, it can lead to inaccurate analyses and incorrect conclusions. You need to decide how to handle missing data: Should you drop rows with missing data, or should you impute the data with placeholder values? Think about this upfront: Is your data complete? If not, what will you do about it?

Next is data `consistency`. Let's say you are joining data from different sources or tables. Is the data represented in a consistent format across these sources? You can check for this with cross-field validation, comparing the data from different sources to ensure consistency.

Data consistency is essential. For example, consider movie ratings: If one table uses a 1 to 5-star rating system and another uses a 1 to 10-star system, those are not consistent rating scales. Trying to combine these tables could result in confusion and incorrect conclusions. A 5-star rating in a 5-star system could be misinterpreted as a low rating when merged with a table using a 1 to 10-star system. This is an example where inconsistency could lead to significant errors.

Also, there's data `accuracy`. Is your data correct? Is it reliable? Does it accurately represent what it's supposed to? Data accuracy is more challenging to test. If you have a source of ground truth, you can compare your data against it, but otherwise, you might need to perform a sanity check. Does the overall nature and distribution of the data match what you'd expect from the real world? Accurate data is vital because inaccurate data leads to incorrect insights, results, and decision-making.

Finally, consider data `integrity`. Is your data maintaining its validity over time? This often involves relationships between different data tables. That's what foreign key checks are for: validating the relationships between tables. These relationships can get out of sync as data changes over time. It's crucial to ensure that relationships between data elements are preserved so your data remains trustworthy over time.


### SQL Review

#### Aggregation

```sql
# COUNT
SELECT COUNT(*) AS total_rows FROM employees;
# SUM
SELECT SUM(salary) AS total_salary FROM employees;
# AVG
SELECT AVG(salary) AS average_salary FROM employees;
# MAX / MIN
SELECT MAX(salary) AS highest_salary FROM employees;
```

**Aggregate with CASE**

`WHERE` clauses are specified after aggregation, so you can only filter on one thing at a time

```sql
SELECT COUNT(*) AS high_salary_count
FROM employees
WHERE salary > 70000;
```

One way to apply multiple filters to what you’re aggregating

```sql
SELECT
    COUNT(CASE WHEN salary > 70000 THEN 1 END) AS high_salary_count,
    COUNT(CASE WHEN salary BETWEEN 50000 AND 70000 THEN 1 END) AS medium_salary_count,
    COUNT(CASE WHEN salary < 50000 THEN 1 END) AS low_salary_count
FROM employees;
```

#### Grouping

```sql
SELECT department_id, COUNT(*) AS number_of_employees
FROM employees
WHERE join_date > '2020-02-01'
GROUP BY deparment_id;
```

#### Nested grouping, sorting

```sql
SELECT YEAR(sale_date) AS sale_year, product_id, SUM(amount) AS total_sales
FROM sales
WHERE sales_year, product_id
ORDER BY sale_year, total_sales DESC,
```

#### Pivoting

Pivoting is another useful concept. While often associated with Excel spreadsheets, you can pivot in SQL as well. The syntax for pivoting varies between databases, so the exam may not quiz you on specific syntax. The key takeaway is understanding what pivoting is: turning row-level data into columnar data—essentially flipping a table on its head.

* Pivoting is the act of turning row-level data into columnar data.
* How this works is very database-specific. Some have a PIVOT command.
* For example, let’s imagine we have a sales table that contains sales amounts and the salesperson in each row, but we want a report by salesperson:

```sql
SELECT salesperson, [Jan] AS Jan_sales, [Feb] AS Feb_sales
FROM
(SELECT salesperson, month, sales FROM sales) AS sourceTable
PIVOT
(
  SUM(sales)
  FOR month IN ([Jan], [Feb])
) AS PivotTable;
```

* The same thing could be achieved with conditional aggregation, without requiring a specific PIVOT operation:

```sql
SELECT
    salesperson,
    SUM(CASE WHEN month = 'Jan' THEN sales ELSE 0 END) AS Jan_sales,
    SUM(CASE WHEN month = 'Feb' THEN sales ELSE 0 END) AS Feb_sales
FROM sales
GROUP BY salesperson;
```

The main takeaway is that pivoting allows you to convert row-level information into columns, flipping the data structure to better fit your analysis needs.


### JOIN´s

Let's assume we have two tables: employees and departments.

```sql
-- Table: employees
CREATE TABLE employees (
  employee_id INT,
  name VARCHAR(50),
  department_id INT
);

INSERT INTO employees (employee_id, name, department_id) VALUES
(1, 'Alice', 1),
(2, 'Bob', 2),
(3, 'Charlie', NULL),
(4, 'David', 3);

-- Table: departments
CREATE TABLE departments (
  department_id INT,
  department_name VARCHAR(50)
);

INSERT INTO departments (department_id, department_name) VALUES
(1, 'HR'),
(2, 'Engineering'),
(4, 'Marketing');

```

**INNER JOIN**
returns only the rows where there is a match in both tables.

```sql
SELECT employees.name, departments.department_name
FROM employees
INNER JOIN departments ON employees.department_id = departments.department_id;
```

| name    | department_name |
|---------|-----------------|
| Alice   | HR              |
| Bob     | Engineering     |

**LEFT OUTER JOIN**

returns all rows from the left table (employees), and the matching rows from the right table (departments). Rows from the left table with no match will show NULL for the columns of the right table.

```sql
SELECT employees.name, departments.department_name
FROM employees
LEFT OUTER JOIN departments ON employees.department_id = departments.department_id;
```

| name     | department_name |
|----------|-----------------|
| Alice    | HR              |
| Bob      | Engineering     |
| Charlie  | NULL            |
| David    | NULL            |

**RIGHT OUTER JOIN**

returns all rows from the right table (departments), and the matching rows from the left table (employees). Rows from the right table with no match will show NULL for the columns of the left table.

```sql
SELECT employees.name, departments.department_name
FROM employees
RIGHT OUTER JOIN departments ON employees.department_id = departments.department_id;
```
| name   | department_name |
|--------|-----------------|
| Alice  | HR              |
| Bob    | Engineering     |
| NULL   | Marketing       |

**FULL OUTER JOIN**

returns all rows when there is a match in either of the tables. If there is no match, the rows will still be included, with NULL in place of missing columns.

```sql
SELECT employees.name, departments.department_name
FROM employees
FULL OUTER JOIN departments ON employees.department_id = departments.department_id;
```

| name     | department_name |
|----------|-----------------|
| Alice    | HR              |
| Bob      | Engineering     |
| Charlie  | NULL            |
| David    | NULL            |
| NULL     | Marketing       |


**CROSS OUTER JOIN**

returns the Cartesian product of both tables, i.e., all possible combinations of rows between the two tables.

```sql
SELECT employees.name, departments.department_name
FROM employees
CROSS JOIN departments;
```

| name    | department_name |
|---------|-----------------|
| Alice   | HR              |
| Alice   | Engineering     |
| Alice   | Marketing       |
| Bob     | HR              |
| Bob     | Engineering     |
| Bob     | Marketing       |
| Charlie | HR              |
| Charlie | Engineering     |
| Charlie | Marketing       |
| David   | HR              |
| David   | Engineering     |
| David   | Marketing       |

### SQL Regular Expressions (a quick intro)

* Pattern matching
  * Think a much more powerful “LIKE”
* `~` is the regular expression operator
* `~*` is case-insensitive
* `!~*` would mean “not match expression, case insensitive”
* Regular expression 101
  * `^` - match a pattern at the start of a string
  * `\$` - match a pattern at the end of a string (boo$ would match boo but not book)
  * `|` - alternate characters (sit|sat matches both sit and sat)
  * Ranges (`[a-z]` matches any lower case letter)
  * Repeats (`[a-z]{4}` matches any four-letter lowercase word)
  * Special metacharacters
    * `\d` – any digit; 
    * `\w` – any letter, digit, or underscore, 
    * `\s` – whitespace, 
    * `\t` – tab
* Example:
  * `SELECT * FROM name WHERE name ~ * ‘^(fire|ice)’;`
  * Selects any rows where the name starts with “fire” or “ice” (case insensitive)


