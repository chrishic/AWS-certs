# AWS Data Stores

## General

* Types of data stores
	- Persistent
		- e.g. RDS, Glacier
	- Transient
		- e.g. SQS, SNS
	- Ephemeral
		- Memcached, EC2 Instance Store
* IOPS vs Throughput
	- IOPS: how fast can you read/write to/from the device
		- i.e. I/O operations per second
	- Throughput: how much data can be moved
* Consistency Models
	- ACID
		- Atomicity
		- Consistency
		- Isolation
		- Durability
	- BASE
		- Basically available: the system guarantees availability, in terms of the CAP theorem
		- Soft state: the state of the system may change over time, even without input (due to eventual consistency model)
		- Eventual consistency: the system will become consistent over time
	- CAP theorem
		- States that a distributed system cannot guarantee the following three properties at the same time:
			- Consistency
			- Availability
			- Partition tolerance
		- BASE system is (Availability, Partition tolerance)
		- ACID system is (Consistency, Availability)


## [Amazon Simple Storage Service (S3)](../../deep-dives/s3/)

## Glacier

* Glacier Vault
	- like a bucket
* Glacier Archive
	- like an object
	- file, zip, tar, etc
	- max size 40TB
	- immutable
* Glacier Vault Lock
	- different than vault access (IAM) policy
	- enforce rules like no deletes or MFA
	- immutable
	- two step process:
		- 1. initiate vault lock
		- 2. 24 hr period in which to either abort or complete
* Access policies
	- Via IAM


## EBS

* Tied to a single AZ
* EBS snapshots
	- cost-effective and easy backup strategy
	- share data sets with other users or accounts
	- migrate system to new AZ or region
	- convert unencrypted to encrypted volume
		- take snapshot of unencrypted volume
		- restore snapshot to new volume, chooisng to enable encryption
* Data Lifecycle Manager
	- schedule snapshots for volumes or instances every X hours
	- provides retention rules to remove stale snapshots
* EC2 instance store
	- when you stop/terminate instance, it goes away
	- direct attached storage
	- temporary, ideal for caches/buffers


## Elastic File Service (EFS)

* Implementation of NFS 
* Elastic storage capacity and pay for only what you use (in contrast to EBS)
* Multi-AZ metadata and data storage
* Configure mount points in 1 or many AZs
* Can be mounted from on-prem systems
	- careful: NFS not secure protocol
		- you should create VPN tunnel to ensure encryption when going over internet
	- alternatively use Amazon DataSync
* EFS 3x more expensive than EBS, 20x more expensive than S3


## [Storage Gateway](../../deep-dives/storage-gateway/)


## RDS

* Supports the following relational databases:
	- Mysql
		- non-transactional storage engines like MyISAM don't support replication; you must use InnoDB (or XtraDB on Maria)
	- MariaDB
		- open source fork of MySQL
	- Postgres
	- MS SQL Server
	- Oracle
	- Aurora
* Replication
	- Multi-AZ
		- replication between master and standby is sync
	- Read replicas
		- Replication between master and read replica is async
		- NOTE: Read replicas service regional users
* In Multi-AZ
	- if master dies, then one of the standbys is promoted to new master
	- if entire region dies, you can use read replica
		- 1. promote read-replica to stand-alone (single-AZ)
		- 2. single_AZ reconfigured to multi-AZ


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


## Redshift

* Fully managed clustered peta-byte scale data warehouse
* Very cost-effective compared to other data warehouse platforms
* Postgres compatible with JDBC/ODBC drivers
	- compatible with most BI tools out of the box
* Parallel processing and columnar data stores which are optimized for complex queries
* Option to query directly from data files on S3 via Redshift Spectrum
* Data Lakes
	- Store data in S3
	- Use Redshift Spectrum (or Athena) to query
	- Use QuickSight for visualization


## Elasticache

* Fully managed implementations of Redis and Memcached
* Push button scalability for memory, writes and reads
* In memory key/value store - not persistent in traditional sense
* Billed by node size and hours of use
* Use Cases
	- web session store
		- store session data in Redis
	- database caching
	- leaderboards
	- streaming data dashboards
* Two types
	- Memcached
		- simple, no frills
		- can scale out and scale in as demand changes
		- need to run multiple CPU cores and threads
		- you need to cache objects (like db queries)
	- Redis
		- you need encryption
		- HIPAA compliance
		- support for clustering
		- complex data types
		- HA (replication)
		- pub/sub capability
		- geospatial indexing
		- backup and restore


## Other Database Options

* Athena
	- SQL engine overlaid on S3 using Presto
	- query raw data objects as they sit in S3 bucket
	- can convert data to Parquet format for big perf jump
	- similar in concept to Redshift Spectrum but...
		- use Athena: data lives mostly on S3 w/o need for joins with other sources
		- use Redshift Spectrum: want to join S3 data with existing Redshift tables or create union products
* Quantum Ledger Database
	- based on blockchain
	- provides immutable and transparent journal as a service 
	- centralized design (as opposed to decentralized consensus-based design for common blockchain frameworks) - allows for high perf and scalabillity
	- append only concept where each record contributes to the integrity of the chain
		- when you write a record to the ledger, you use the hash of the previous record to compute hash of new record
		- that's why you cannot delete/change any records - it will invalidate the chain
* Amazon Managed Blockchain
	- fully managed blockchain framework
		- supports either Hyperledger Fabric or Ethereum
	- distributed consensus-based concept consisting of network, members (other AWS accounts), nodes (instances) and potentially apps
	- uses QLDB ordering service to main complete history of all transactions
* Amazon Timestream Database
	- fully managed db for storing/analyzing time-series data
	- alternative to DynamoDB or Redshift and includes some built in analytics like interpolation and smoothing
	- use cases:
		- sensor networks
		- equipment telemetry
* DocumentDB
	- MongoDB-compatible
	- multi-AZ HA, scalable, integrated with KMS, backed up to S3
* ElasticSearch
	- Mostly a search  engine but also a document store
	- Aka ELK stack
		- Kibana (analytics)
		- Logstash (intake)
		- Elasticsearch (search and storage)
* Amazon WorkDocs
	- Amazon's version of Dropbox
		- can integrate with AD for SSO
		- web, mobile and native clients (no Linux client)
