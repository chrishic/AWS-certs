## DynamoDB

* Managed, multi-AZ NoSQL data store with cross-region replication option
* Defaults to eventual consistency reads but can request strongly consistent read via SDK parameter
* Priced on throughput, rather than compute
* Provision read and write capacity in anticipation of need
* Autoscale capacity adjusts per configured min/max levels
	- watches table for elevated requests (read/write), which triggers CW alarm
- On-demand capacity for flexible capacity at small premium cost
- Achieve ACID compliance with DynamoDB Transactions
* Terminology
	- Attribute: Name/value pair
	- Item: the document (collection of attributes)
	- Table: collection of items
* Primary key
	- Also known as partition key
	- MUST be unique
	- Can be either:
		- partition key
		- partition key + sort key
* Secondary indexes
	- Global secondary index (GSI)
		- partition key and sort key can be different than from those on the table
		- use when you want fast query of attributes outside of primary key
	- Local secondary index (LSI)
		- same partition key as the table but different sort key
		- use when you know the partition key and want to quickly query on some other attribute
	- max of 5 LSI and 5 GSI
	- max 20 attributes across all indexes
	- indexes take up storage space
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
	- (Total RCU / 3000) + (Total WCU / 1000)
By Size
	- Total Size / 10 GB
Total Partitions
	- Round up for the MAX(By Capacity, By Size)

Example:
10GB table, 2000 RCU, 2000 WCU
- by capacity = 2000/3000 + 2000/1000 = 2.66
- by size = 10 / 10 = 1
- total = ceil(max(2.66, 1)) = 3 partitions

Auto Scaling for DynamoDB
- Uses Target Tracking method to try to stay close to target utilization
- Currently  does not scale down if table's consumption drops to zero
	- Workarounds:
		- Send requests to the table until it auto scales down
		- Manually reduce the max capacity to be the same as minimum capacity
- Also supports Global Secondary Indexes
	- Makes sense since GSI are essentially copies of the table

On-Demand Scaling for DynamoDB
- Alternative to Auto Scaling
- Useful if you can't deal with scaling lag or truly have no idea of the anticipated capacity requirements
- Instantly allocates capacity as needed with no concept of provisioned capacity
- Costs more than traditional provisioning and auto scaling

DynamoDB Accelerator (DAX)
- in-memory cache that sits in front of DynamoDB table
- good use cases
	- require fastest possible reads such as live auctions
	- read intense scenarios where you want to offload reads from DynamoDB
	- repeated reads against a large set of data
- bad uses of DAX
	- write-intensive apps that don't have many reads
	- apps where you use client caching methods




Use Amazon DynamoDB Local More Easily with the New Docker Image
https://hub.docker.com/r/amazon/dynamodb-local/
https://aws.amazon.com/about-aws/whats-new/2018/08/use-amazon-dynamodb-local-more-easily-with-the-new-docker-image
https://aws.amazon.com/dynamodb/

Setting Up DynamoDB Local (Downloadable Version)
https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/DynamoDBLocal.html






Best Practices for Handling Time-Series Data in DynamoDB
https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/bp-time-series.html


General design principles in DynamoDB recommend that you keep the number of tables you use to a minimum. For most applications, a single table is all you need. However, for time-series data, you can often best handle it by using one table per application per period.


The following design pattern often handles this kind of scenario effectively:

Create one table per period, provisioned with the required read and write capacity and the required indexes.

Before the end of each period, prebuild the table for the next period. Just as the current period ends, direct event traffic to the new table. You can assign names to these tables that specify the periods they have recorded.

As soon as a table is no longer being written to, reduce its provisioned write capacity to a lower value (for example, 1 WCU) and provision whatever read capacity is appropriate. Reduce the provisioned read capacity of earlier tables as they age. You may choose to archive or delete the tables whose contents will rarely or never be needed.

The idea is to allocate the required resources for the current period that will experience the highest volume of traffic and scale down provisioning for older tables that are not used actively, therefore saving costs.


Managing Throughput Settings on Provisioned Tables
https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/ProvisionedThroughput.html
