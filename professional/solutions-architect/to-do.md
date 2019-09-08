# To Do


## Action Items

* Create study guides for the following AWS services:
	- CloudWatch
		- CloudWatch Events rule
	- S3
		- Revisit S3 lifecycle policies and rules of when you can transition to Glacier
			- Can you transition from S3 standard to S3 IA after one day? Or is there a minimum of 30 days before transition?
		- Batch S3 calls to reduce costs
	- Storage Gateway
	- Systems Manager
		- SSM agent installation
		- Tools:
			- RunCommand
			- Session Manager
			- State Manager
			- Patch Manager
			- Parameter Store for storing/retrieving secrets
	- CloudFormation
		- Mappings
		- Conditions
		- How do you test change sets in CloudFormation?
	- VPC
		- VPC endpoints
		- Setup VPN between on-premises machines and AWS VPC
		- VPC peering
			- To have different VPCs talk with each other, do you just create the peering and then update the route tables on both VPCs?
		- Design a hybrid architecture using key AWS technologies
			- Direct Connect (DX)
				- Understand VIFs (both public and private)
			- VPN
			- Transit VPC
			- CloudHub
	- Route 53
		- routing options, in particular:
			- failover routing
* Play around with Secrets Manager
* Techniques to allow mobile users to make calls to DynamoDB
	- Cognito // User pools vs identity pools // Security Token Service // Web Identity Provider
* Architect a continuous integration and deployment process
* Blue/green deployments
* Understand cache population techniques
	- Write-through
	- Lazy loading	


## Items to Research

* AWS Batch
* CloudTrail 
	- log file integrity validation
* Elastic Beanstalk
	- Deployment types (immutable, rolling)
	- Does *not* support deleting application bundles after deploy
* CloudFront
	- CloudFront Trusted Signer
	- CloudFront-signed URL
* Code*
	- CodeCommit
	- CodeBuild
	- CodePipeline
	- CodeDeploy
		- To prevent instances from being terminated during updates (due to failed health checks), CodeDeploy can:
			- run pre-install tasks to remove an instance from an ELB load balancer
			- perform install
			- then reinstate instance to load balancer
* Migration tools
	- Application Discovery Service
	- Database Migration Service
	- Server Migration Service
* [Amazon Elasticsearch Service](https://aws.amazon.com/elasticsearch-service/)
	- Fully managed service for Elasticsearch
		- open-source Elasticsearch APIs
		- managed Kibana
		- integrations with Logstash and other AWS Services
	- Securely ingest data from any source and search, analyze, and visualize it in real time
	- ELK stack
		- Elasticsearch
		- Logstash
		- Kibana
			 - data visualization plugin for Elasticsearch
			 - create bar, line and scatter plots, or pie charts and maps on top of large volumes of data
* [Amazon Athena](https://aws.amazon.com/athena/)
	- interactive query service that makes it easy to analyze data in Amazon S3 using standard SQL
	- serverless: you pay only for the queries that you run
	- makes it easy for anyone with SQL skills to quickly analyze large-scale datasets
	- easy to use:
		- point to your data in S3
		- define the schema
		- start querying using standard SQL
* [Amazon QuickSight](https://aws.amazon.com/quicksight/)
	- BI tool
	- create and publish dashboards
	- pay-per-session pricing
* Understand how [Amazon EMR](https://aws.amazon.com/emr/) works
	- Big data platform allowing teams to process vast amounts of data quickly, and cost-effectively at scale
		- Uses open source tools such as: Apache Spark, Apache Hive, Apache HBase, Apache Flink, and Presto
	- [Understanding Master, Core, and Task Nodes](https://docs.aws.amazon.com/emr/latest/ManagementGuide/emr-master-core-task-nodes.html)
