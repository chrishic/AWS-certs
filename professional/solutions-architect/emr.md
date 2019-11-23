# AWS Elastic MapReduce (EMR)

## EMR

* Managed Hadoop framework for processing huge amounts of data quickly, and cost-effectively at scale
	- Uses open source tools such as: Apache Spark, Apache Hive, Apache HBase, Apache Flink, and Presto
* Most commonly used for log analysis, financial analysis or extract, ETL
* Components
	- Step
		- Programmatic task for performing some process on the data (i.e. count words)
	- Cluster
		- Collection of EC2s provisioned by EMR to run your Steps
		- Three types of cluster nodes
			- Master node
			- Core node
				- Persists data using HDFS
			- Task node
				- Ephemeral


## Hadoop Framework

* Collection of various open source tools and services, including:
	- Hadoop HDFS
		- Hadoop Distributed File System
			- persistent data store
	- Hadoop MapReduce
		- distributed processing
	- ZooKeeper
		- resource coordination
	- Flume
		- log collection
	- Sqoop
		- Data transfer
	- Pig
		- Scripting
	- Hive
		- SQL
	- Mahout
		- Machine learning
	- Oozie
		- Workflow
	- Apache HBase
		- Columnar datastore
	- Ambari
		- Management and monitoring


## Links

* [Amazon EMR](https://aws.amazon.com/emr/)
* [Understanding Master, Core, and Task Nodes](https://docs.aws.amazon.com/emr/latest/ManagementGuide/emr-master-core-task-nodes.html)
