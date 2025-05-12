 
## Mongo DB Background

### Origins of MongoDB

MongoDB was created by 10gen (now MongoDB Inc.) in 2007 as part of a larger platform-as-a-service (PaaS) product. The company initially aimed to develop a comprehensive cloud-based infrastructure that included a database component. However, they soon realized that the database technology they were developing had broader potential beyond their initial PaaS vision. As a result, they pivoted to focus entirely on MongoDB, refining it as a standalone database solution.
Open-Source Release and Adoption

In 2009, MongoDB was officially released as an open-source project, allowing developers worldwide to leverage its features for free. The database quickly gained popularity due to its document-oriented design, scalability, and flexibility. Unlike traditional SQL databases, MongoDB provided a schema-less structure, making it particularly well-suited for rapidly evolving applications and big data workloads.
#### Why Was MongoDB Created?

At the time, most databases were relational (SQL-based), but they struggled with several challenges:

•	Scalability – Traditional SQL databases were difficult to scale horizontally (i.e., distributing data across multiple servers efficiently).

•	Schema Rigidity – SQL databases required predefined schemas, making it difficult to accommodate changing application requirements.

•	Big Data Challenges – The rise of web applications, mobile apps, and IoT led to an explosion of unstructured data, which relational databases were not well-equipped to handle.

MongoDB was designed to address these issues by offering:

•	A flexible, schema-less architecture, allowing for dynamic and evolving data models.

•	Horizontal scalability through sharding, enabling efficient data distribution across multiple servers.

•	High performance with in-memory processing, optimizing read and write operations for modern applications.

#### Key Features That Set MongoDB Apart

MongoDB introduced several innovative features that distinguished it from traditional databases:

•	Document-Oriented Storage – Data is stored in BSON (binary JSON) format, allowing for complex, nested structures.

•	Indexing for Speed – Supports various types of indexes to enhance query performance.

•	Replication & High Availability – Uses replica sets to ensure data redundancy and fault tolerance.

•	Automatic Sharding – Distributes large datasets across multiple servers to improve scalability.

•	Aggregation Framework – Enables powerful data transformations and computations.

•	Multi-Document Transactions – Added in 2018, allowing for ACID-compliant operations across multiple documents.

#### MongoDB Timeline & Growth

•	2007 – 10gen begins developing MongoDB.

•	2009 – MongoDB is released as an open-source project.

•	2013 – 10gen rebrands as MongoDB Inc., focusing entirely on database technology.

•	2017 – MongoDB goes public with an IPO on NASDAQ.

•	2020s – Becomes one of the leading databases for modern applications, used by major companies like Google, Facebook, Uber, and others.

•	Present Day – Continues to evolve with features like MongoDB Atlas (a fully managed cloud database) and serverless offerings.

MongoDB remains at the forefront of NoSQL databases, providing a scalable and flexible solution for developers building modern applications.

#### Sources:
-	https://www.geeksforgeeks.org/mongodb-architecture/
-	https://www.techtarget.com/searchdatamanagement/definition/MongoDB
-	https://www.mongodb.com/resources/products/capabilities/features




	<br/>	<br/>
	### Info:	<br/>
MongoDB was initially built with the cloud in mind
 	<br/>	<br/>
MongoDB documents are formatted in BSON (an extended Binary form of JSON) 
	<br/>	<br/>
 High availability and easy scalability through distributed workloads

		
	<br/>	<br/>
###	What is a cluster ? 	<br/>
	<br/>
=> clusters can refer to two different architectures	<br/>
				<br/>
1. Replica Set: group of one or more servers containing the exact copy of the data.	<br/>
					-> High availability and redundancy	<br/>
	<br/>
	<br/>
ensures replication is enforced by constantly copying Operations log (Oplog) of primary server	<br/>
	<br/>
possible to have one or two, recommended minimum is 3	<br/>
				<br/>
2. Sharded Cluster:  distributes large datasets across multiple shards (servers) to improve performance for both read and write operations	<br/>
					-> horizontal Scaling	<br/>
	<br/>
=> each shard contains own replica sets ( more than one router or configuration server recommended for high availability)	<br/>
						<br/>
	
<br/>
			
When a read or write operation is performed on a collection, the client sends the request to a router (mongos). 	<br/>
The router will then validate which shard the data is stored in via the configuration server and send the requests to the specific cluster.	<br/>
				
<br/>
in Distributed applications, due to data locality, a replica set on a different server, next to fault tolerance,
				 can handle read operations and thereby increase availability and durability
	<br/>	<br/>	<br/>
Primary Node: Handles all write operations	<br/>
Secondary Nodes: continuously replicate primary node.	<br/>
	<br/>

### What is the Oplog ?
	
 <br/>
 => is a special capped collection that keeps a rolling record of all operations that modify the data for a specified amount of hours<br/>
 	<br/>
 unlike other capped collections, can dynamically resize configured size limit, to avoid deleting commits up to the maximum configured size	<br/>
	<br/>
 => By default, tries to delete oldest and always keep max size
			
<br/>	
<br/>Default Oplog size: 5% of physical memory when self managed deployment, otherwise 5% free disk space
<br/>	MacOS: 192MB

<br/>	<br/>
MongoDB applies DB operations on primary and then records operations on primary ’s oplog.  	<br/>
secondary members copy and apply these operations (asynchronously). 	<br/>
All secondary members contain a copy of the oplog, which allows them to maintain current state of DB. 	<br/>
<br/>	<br/>

### What is an Election Process ?	<br/>
=> process where members of a replica set select a primary on startup and in the event of a failure.	<br/>
 	<br/>
		When primary node becomes unavailable, replica set initiates election to select new primary from among secondary’s.	<br/>
			Election can be triggered by:	<br/>
		 &nbsp;&nbsp;		- Primary node failure	<br/>
			&nbsp;&nbsp; 	- timeout for >10 seconds (default, can be configured ) 	<br/>
			&nbsp;&nbsp; 	- Addition of new nodes to replica set (research needed )	<br/>
			&nbsp;&nbsp; 	- Initial replica set configuration	<br/>
			&nbsp;&nbsp; 	- planned maintenance operations 	<br/>
				
		





