# To Do

## Action Items

* Create study guides for the following AWS services:
	- CloudWatch
		- CloudWatch Events rule
	- CloudFormation
	- VPC
		- Virtual Private Gateway (AWS side)
		- Customer Gateway (customer side)
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
	- ElastiCache
		- ElastiCache for Redis at-rest encryption
			- optional feature that encryptson-disk data
			- when enabled on a replication group, it encrypts the following aspects:
				- Disk during sync, backup and swap operations
				- Backups stored in Amazon S3
			- By default, uses service managed encryption at rest (e.g. SSE-Redis)
			- You can also use your own CMKs (e.g. SSE-KMS)
		- Links:
			* [At-Rest Encryption in ElastiCache for Redis](https://docs.aws.amazon.com/AmazonElastiCache/latest/red-ug/at-rest-encryption.html)
			* [Amazon ElastiCache for Redis adds support for customer managed keys in AWS Key Management Service for encryption at rest](https://aws.amazon.com/about-aws/whats-new/2019/08/amazon-elasticache-for-redis-adds-support-for-customer-managed-keys-in-aws-key-management-service-for-encryption-at-rest/)
			* [ElastiCache for Redis In-Transit Encryption - TLS](https://docs.aws.amazon.com/AmazonElastiCache/latest/red-ug/in-transit-encryption.html)
* Play around with Secrets Manager
* Techniques to allow mobile users to make calls to DynamoDB
	- Cognito // User pools vs identity pools // Security Token Service // Web Identity Provider
* Architect a continuous integration and deployment process
* Understand cache population techniques
	- Write-through
	- Lazy loading	


## Items to Research

* API Gateway
	- Method Request/Response vs Integration Request/Response
	- See: [Handle Lambda Errors in API Gateway](https://docs.aws.amazon.com/apigateway/latest/developerguide/handle-errors-in-lambda-integration.html)
* Cognito
	- User Pools, Identity Pools, federation of users
	- See: [Getting Started with Amazon Cognito Identity Pools (Federated Identities)](https://docs.aws.amazon.com/cognito/latest/developerguide/getting-started-with-identity-pools.html)
* EC2
	- Need to understand EC2 Placement Groups
		- Spread, Cluster, Partition
* EBS
	- Data Lifecycle Manager
		- See: [Automating the Amazon EBS Snapshot Lifecycle](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/snapshot-lifecycle.html)
	- Minimum sizes, constraints on types that can be used for root volume
		- See: [Requesting Modifications to Your EBS Volumes](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/requesting-ebs-volume-modifications.html#elastic-volumes-limitations)
* AWS Batch
* CloudTrail 
	- log file integrity validation
* Elastic Beanstalk
	- Deployment types (immutable, rolling)
	- Does *not* support deleting application bundles after deploy
* CloudFront
	- CloudFront Trusted Signer
	- CloudFront-signed URL
	- CloudFront Signed URLs
	- CloudFront Signed Cookies
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
* AWS Organizations
	- Service Control Policies
		- How to prevent disabling CloudTrail logging across all accounts under the Organization
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
* AWS Glue
