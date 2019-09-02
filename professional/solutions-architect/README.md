# AWS Certified Solutions Architect - Professional
Study guide for [AWS Certified Solutions Architect - Professional](https://aws.amazon.com/certification/certified-solutions-architect-professional/).

## Table of Contents

<!-- MarkdownTOC depth=4 -->

- [Content Outline](./content-outline.md)
- [Preparation Steps](#preparation-steps)
- [General Strategies](#general-strategies)
- [Domains](#domains)
	- [Design for Organizational Complexity](#design-for-organizational-complexity)
	- [Design for New Solutions](#design-for-new-solutions)
- [Whitepapers](#whitepapers)
- [re:Invent Videos](#reinvent-videos)
- [Other Resources](#other-resources)
- [TO DO](./to-do.md)

<!-- /MarkdownTOC -->

---

## Preparation Steps
  1. Review the [Exam Guide](https://d1.awsstatic.com/training-and-certification/docs-sa-pro/AWS_Certified_Solutions_Architect_Professional-Exam_Guide_EN_1.2.pdf) and [Sample Questions](https://d1.awsstatic.com/Train%20%26%20Cert/docs/AWS_certified_solutions_architect_professional_examsample.pdf)
  2. [AWS Training course - Exam Readiness: AWS Certified Solutions Architect – Professional](https://www.aws.training/Details/eLearning?id=34737)
  3. [Cloud Guru preparation course](https://acloud.guru/learn/aws-certified-solutions-architect-professional-2019)
  4. [Linux Academy preparation course](https://linuxacademy.com/course/aws-certified-solutions-architect-professional-2018/)
  5. Review the [Well Architected Framework deep dive](../../deep-dives/well-architected-framework/)
  6. Review the [AWS Encryption deep dive](../../deep-dives/encryption/)
  7. Review [whitepapers](#whitepapers)
  8. Review FAQs


## General Strategies
* Manage your time effectively
	- You have 170 minutes to complete exam
	- Flag questions you are unsure about to come back to later
* Exam relies heavily on reading comprehension
	- Read both the question and the answer in full at least once
* Identify text in the question that implies certain AWS features
	- e.g. "data retrieval times"
* Identify features mentioned in the answers
* Pay attention to qualifying clauses
	- e.g. "most cost effective" or "will best fulfill"
* Eliminate obviously wrong answers to narrow the selection of possible right answers.


## Domains

### Design for Organizational Complexity

* 1.1. Cross-account authentication and access strategies
	- Know IAM inside-and-out
		- User-based access controls
			- Users, groups, roles
		- Resource-based access controls
		- IAM policies
	- Cross account delegation
		- Via Security Token Service (STS) to generate temporary access credentials
	- User federation
		- Identity store is outside AWS
		- Roles for federated users
			- Give federated users access to AWS resources for limited amount of time
		- AWS Directory Service
			- Simple AD
				- Does *not* support MFA
			- AD Connector
			- AWS Managed Microsoft AD
		- AWS SSO
* 1.2. Networks
	- Hybrid VPN connections
		- AWS Managed VPN
			- hardware VPN connection between your on-premise network and your AWS VPC
		- AWS VPN CloudHub
			- hub-and-spoke model of connecting multiple branch locations to your VPC
		- Software VPN
			- user managed software appliance running in your VPC
	- Direct Connect (DX)
		- Customer updates router to connect to DX partner
		- DX partner has direct connection to AWS
		- Understand Direct Connect Gateways
		- Understand how to setup public, private virtual interfaces (VIFs)
	- Storage gateways
		- File Gateway
			- NFS/CIFS access to S3/Glacier
		- Tape Gateway
			- iSCSI VTL interface, stored in S3 and archived to Glacier
		- Stored-volume Gateway
			- iSCSI, up to 16TB in size, full data is kept on-premise, backups made to S3 as EBS snapshots
		- Cached-volume Gateway
			- iSCSI, up to 32TB in size, only cache is kept on-premise, full data in S3, uses EBS snapshots
	- VPC endpoints
		- By default, IAM users do *not* have permission to work with endpoints
			- You must create an IAM user policy that grants users the permissions to create/modify/describe/delete endpoints
		- Interface endpoints
			- ENI with private IP that serves as entry point for traffic to services powered by AWS PrivateLink
		- Gateway endpoints
			- Target for a specified route in your route table
				- Used by supported AWS services, such as S3 or DynamoDB
* 1.3. Multi-account AWS environments
	- Multi-account AWS environments
		- Enables resource and billing isolation
		- Also provides security benefits
		- E.g. Separate accounts for dev, test, production
	- Billing strategies for multiple accounts
		- Send notifications to group aliases
		- Use AWS tagging standards across accounts
		- Automate baseline configuration with APIs and scripts
	- AWS Organizations
		- Policy-based central management for multiple AWS accounts
		- Manage policies across accounts and filter out unwanted access
		- Automate creation of new accounts through APIs
		- Organize accounts into organizational units (OUs)
		- Offers consolidated billing
	- Make sure you know:
		- Service control policies (SCPs)


### Design for New Solutions

* Core architecture services
	- EC2 Auto Scaling
	- Elastic Load Balancing
	- SQS
		- Standard queues
			- Unordered, at least once delivery
		- FIFO queues
	- SNS
	- ElastiCache
		- Redis
			- persistent, auto failover with multi-AZ, can scale up but not down, scale with read replicas
		- Memcached
			- in-memory only
	- Kinesis
		- Kinesis Streams
			- Ordered within shard, async, "at least once" semantics
			- Custom processing per incoming record
			- Sub-second processing latency
		- Kinesis Firehose
			- Zero admin
			- Processing latency of at least 60 seconds
			- Ability to use existing analytics tools such as S3, Redshift
		- Know difference between Data Streams and Data Firehose
	- CloudFormation
		- Make sure you understand:
			- Mappings
			- Conditions
	- CloudWatch
	- Cognito
		- Fully managed solution providing access control and authentication for web/mobile apps
		- Features
			- Supports MFA
			- Data at-rest and in-transit encryption
			- Log in via social identity providers
			- Support for SAML	
		- Core concepts
			- User pools
				- Directory profile for all users
				- Supports user federation through a third-party IdP
				- Signed users receive authentication tokens
				- Tokens can be exchanged for AWS access via Amazon Cognito identity pools
			- Identity pools
				- Auth users with web IdP, including Amazon Cognito user pools
				- Assign temporary AWS credentials via STS
				- Supports anonymous guest users
* Other important services
	- RDS
		- Scaling
			- Change instance size (CPU)
			- Change storage size (disk)
		- Read replicas
			- Determine async repl lag with CloudWatch metric, "ReplicaLag"
	- DynamoDB
		- DynamoDB Accelerator (DAX)
			- read-through cache
			- write-through cache
		- Auto Scaling (via integration with CloudWatch)
		- Global Tables
			- multi-region, multi-master
	- EBS
		- Volume size can be increased while attached to instance
		- Volume type and thruput can be changed while attached to instance
		- EC2 instances have max EBS throughput rate
		- Consider OS-based RAID sets
	- CloudFront
		- Reduce traffic costs, increase performance
		- Origin can be from AWS or on-premise
		- Access to S3 buckets can be limited to Origin Access Identities (OAI)
	- Route53


## Whitepapers
* [AWS Security Best Practices whitepaper, August 2016](https://d1.awsstatic.com/whitepapers/Security/AWS_Security_Best_Practices.pdf)
* [Architecting for the Cloud AWS Best Practices whitepaper, February 2016](https://aws.amazon.com/whitepapers/architecting-for-the-aws-cloud-best-practices/)
* [Microservices on AWS](https://docs.aws.amazon.com/whitepapers/latest/microservices-on-aws/introduction.html)
* [Amazon Web Services: Overview of Security Processes whitepaper, May 2017](https://d1.awsstatic.com/whitepapers/aws-security-whitepaper.pdf)
* Practicing Continuous Integration and Continuous Delivery on AWS Accelerating Software Delivery with DevOps whitepaper, June 2017
* Using Amazon Web Services for Disaster Recovery whitepaper, October 2014


## re:Invent Videos
* [AWS re:Invent 2016: Amazon Global Network Overview with James Hamilton](https://www.youtube.com/watch?v=uj7Ting6Ckk)
* [AWS re:Invent 2017: Networking Many VPCs: Transit and Shared Architectures (NET404)](https://www.youtube.com/watch?v=KGKrVO9xlqI)


## Other Resources
* [AWS Documentation for services](https://docs.aws.amazon.com/index.html)
* [AWS Architecture Center](https://aws.amazon.com/architecture/)
