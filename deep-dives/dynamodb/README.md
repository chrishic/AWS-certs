## DynamoDB

* Fully managed, multi-AZ NoSQL data store with cross-region replication option
* Defaults to eventual consistency reads but can request strongly consistent read via API parameter
* Priced on throughput, rather than compute
* Achieve ACID compliance with DynamoDB Transactions
* Terminology
	- Attribute: Name/value pair
	- Item: the document (collection of attributes)
	- Table: collection of items
		- Table names must be unique within each region
			- Just like S3, Dynamo has public namespace
* Primary key
	- MUST be unique
	- Can be either:
		- partition key
		- partition key + sort key (known as composite primary key)
* Secondary indexes
	- Global secondary index (GSI)
		- partition key and sort key can be different than from those on the table
		- use when you want fast query of attributes outside of primary key
		- you can create GSIs at any time (at time of base table creation or anytime thereafter)
		- only supports eventual consistency reads
			- replication is asynchronous
			  (cannot provide strong consistency)
		- has its own capacity settings
			- does not share with base table
		- default limit of 20 GSI per table
			- Can be increased upon request
	- Local secondary index (LSI)
		- same partition key as the table but different sort key
		- use when you know the partition key and want to quickly query on some other attribute
		- you can only create LSIs when base table is created
		- supports strongly or eventual consistency reads
			- synchronous replication
		- shares capacity units with base table
		- limited to max of 5 LSI per table
	- maximum limit of 100 attributes across all indexes
		- Same attribute in multiple indexes counts for each occurrence
	- Keep in mind: indexes take up storage space!
		- You are duplicating data
* Use cases for indexes
	- Access just a few attributes the fastest way possible
		- project those few attributes in global secondary index
	- Frequently access some non-key attributes
		- project those attributes in global secondary index
	- Frequently access most non-key attributes
		- project those attributes or entire table in global secondary index
* Sparse indexes
	- space is only taken up by items that contain that attribute
* You can create table replicas via GSI
	- Use case: better QOS for paying customers
		- primary table for premium customers
			- higher RCU/WCU limits
		- GSI for free-tier customers
			- lower RCU/WCU limits
	- Use case: split RCU and WCU
		- primary table for writes only - set high WCU
		- GSI for reads only - set high RCU
* Two types of read/write capacity modes
	- Provisioned
		- You provision read and write capacity in anticipation of need
			- Good for when workload is predictable
		- Read capacity units (RCU)
			- (Strongly consistent) 1 read/sec for item up to 4KB
			- (Eventually consistent) 2 read/sec for item up to 4KB
			- Transactional reads require 2 RCU for 1 read/sec for each 4KB
		- Write capacity units (WCU)
			- 1 write/sec for item up to 1KB
			- Transactional writes require 2 WCU for 1 write/sec for each 1KB
		- Auto Scaling for DynamoDB
			- Autoscale capacity adjusts per configured min/max levels
				- Watches table for elevated requests (read/write), which triggers CloudWatch alarm
			- Uses Target Tracking method to try to stay close to target utilization
			- Currently does not scale down if table's consumption drops to zero
				- Workarounds:
					- Send requests to the table until it auto scales down
					- Manually reduce the max capacity to be the same as minimum capacity
			- Also supports Global Secondary Indexes
				- Makes sense since GSI are essentially copies of the table
	- On-Demand
		- For flexible capacity at small premium cost
		- Alternative to Auto Scaling
		- Useful if you can't deal with scaling lag or truly have no idea of the anticipated capacity requirements
		- Instantly allocates capacity as needed with no concept of provisioned capacity
		- Costs more than traditional provisioning and auto scaling
	- NOTE: if you go beyond provisioned capacity, you'll get an exception
		- `ProvisionedThroughputExceededException`
* Reading data
	- Query
		- Very efficient, uses indexes
		- must provide partition key
		- optionally can provide sort key
	- Scan
		- Reads entire table, with option to filter out results before sending
		- Very expensive, slow
* Using DynamoDB Streams
	- Have stream trigger Lambda function
	- Lambda trigger can do any of the following:
		1. Update Elastic Search
		2. Send change notification (SNS message)
		3. Compute realtime aggregation and store results back in DynamoDB
		4. Send data to Kinesis Firehose->S3(Parquet)->Athena pipeline for adhoc analytics


# DynamoDB

## DynamoDB Scaling

Scaling in two dimensions:
1. Throughput
	- Write capacity units, read capacity units
2. Size
	- Max item size is 400KB

Partition
- physical space where DynamoDB data is stored
	- divided into 10GB chunks

Partition Key
- unique id for each record
	- sometimes called a hash key

Sort Key
- in combination with partition key, optional second part of composite key that defines storage order
	- sometimes called a range key

Partition Calculations

By Capacity
	- When you exceed RCUs (3000) or WCUs (1000) limits for a single partition
		- I.e. (Total RCU / 3000) + (Total WCU / 1000)
By Size
	- Every 10 GB of data
		- I.e. Total Size / 10 GB
Total Partitions
	- Round up for the MAX(By Capacity, By Size)

Example:
10GB table, 2000 RCU, 2000 WCU
- by capacity = 2000/3000 + 2000/1000 = 2.66
- by size = 10 / 10 = 1
- total = ceil(max(2.66, 1)) = 3 partitions

When DynamoDB detects hot partition, it will split that partition

DynamoDB Accelerator (DAX)
- in-memory cache that sits in front of DynamoDB table
- good use cases
	- require fastest possible reads such as live auctions
	- read intense scenarios where you want to offload reads from DynamoDB
	- repeated reads against a large set of data
- bad uses of DAX
	- write-intensive apps that don't have many reads
	- apps where you use client caching methods

Data modeling best practices
	- Typical apps
		- In general, keep number of tables to a minimum
		- For most apps, a single table is all you need
	- Apps with time-series data
		- Create one table per period, provisioned with required read/write capacity and indexes
		- Before end of each period, prebuild table for next period
			- As current period ends, direct traffic to new table
			- As soon as table no longer being written to, reduce its write capacity
			- Consider archiving or deleting tables as they age
		- Primary goal
			- Allocate required resources for current period that experiences highest volume of traffic
			- Scale down provisioning for older tables


## Links

* [AWS re:Invent 2019: [REPEAT 1] Amazon DynamoDB deep dive: Advanced design patterns (DAT403-R1)](https://www.youtube.com/watch?v=6yqfmXiZTlM)
* [AWS re:Invent 2019: Data modeling with Amazon DynamoDB](https://www.youtube.com/watch?v=DIQVJqiSUkE)
* [AWS re:Invent 2018: Amazon DynamoDB Deep Dive: Advanced Design Patterns for DynamoDB (DAT401)](https://www.youtube.com/watch?v=HaEPXoXVf2k)
* [Build On DynamoDB | S1 E2 – Intro to NoSQL Data Modeling with Amazon DynamoDB](https://www.youtube.com/watch?v=Rmf8mrJ3X2s)
* [How to model one-to-many relationships in DynamoDB](https://www.alexdebrie.com/posts/dynamodb-one-to-many/)
* [DynamoDB Local](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/DynamoDBLocal.html)
* [NoSQL Workbench for Amazon DynamoDB](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/workbench.html)
* [Best Practices for Handling Time-Series Data in DynamoDB](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/bp-time-series.html)
* [Managing Throughput Settings on Provisioned Tables](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/ProvisionedThroughput.html)
* [The Ten Rules for Data Modeling with DynamoDB](https://www.trek10.com/blog/the-ten-rules-for-data-modeling-with-dynamodb)
* [The What, Why, and When of Single-Table Design with DynamoDB](https://www.alexdebrie.com/posts/dynamodb-single-table/)
* [Take Advantage of Sparse Indexes](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/bp-indexes-general-sparse-indexes.html)
