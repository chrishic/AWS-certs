# AWS Certified Solutions Architect - Professional
Study guide for [AWS Certified Solutions Architect - Professional](https://aws.amazon.com/certification/certified-solutions-architect-professional/).


## Table of Contents

<!-- MarkdownTOC depth=4 -->

- [Content Outline](./content-outline.md)
- [Preparation Steps](#preparation-steps)
- [General Strategies](#general-strategies)
- [Important Concepts](#important-concepts)
- [Domains](#domains)
	- [Design for Organizational Complexity](#design-for-organizational-complexity)
	- [Design for New Solutions](#design-for-new-solutions)
	- [Migration Planning](#migration-planning)
	- [Cost Control](#cost-control)
	- [Improving Existing Architectures](#improving-existing-architectures)
- [Test Preparation Services](#test-preparation-services)
- [Whitepapers](#whitepapers)
- [AWS Training Courses](#aws-training-courses)
- [re:Invent Videos](#reinvent-videos)
- [Other Resources](#other-resources)
- [TO DO](./to-do.md)

<!-- /MarkdownTOC -->

---

## Preparation Steps
* Review the [Exam Guide](https://d1.awsstatic.com/training-and-certification/docs-sa-pro/AWS_Certified_Solutions_Architect_Professional-Exam_Guide_EN_1.2.pdf) and [Sample Questions](https://d1.awsstatic.com/Train%20%26%20Cert/docs/AWS_certified_solutions_architect_professional_examsample.pdf)
* Take the [AWS training courses](#aws-training-courses) listed below 
* [A Cloud Guru preparation course](https://acloud.guru/learn/aws-certified-solutions-architect-professional-2019)
* [Linux Academy preparation course](https://linuxacademy.com/course/aws-certified-solutions-architect-professional-2018/)
* Review the following deep dives
	- [Well Architected Framework deep dive](../../deep-dives/well-architected-framework/)
	- [AWS Encryption deep dive](../../deep-dives/encryption/)
	- [Amazon Simple Storage Service (S3) Deep Dive](../../deep-dives/s3/)
* Review [whitepapers](#whitepapers)
	- With "Storage Options in the AWS Cloud, October 2013" whitepaper, pay attention to the "Anti-patterns" sections for each option
* Review FAQs
* Take the "AWS Certified Solutions Architect – Professional" practice exam
	- If you get a passing score, you are ready for the actual exam


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


## Important Concepts

* Disaster recovery
	- Understand RTO, RPO
	- Understand the difference between each type and what each offers with respect to RTO/RPO
		- Pilot light
		- Warm standby
		- Understand what each offers in terms of RTO, RPO
	- Understand difference between HA and fault tolerant
* Cloud migration
	- Make sure you understand the difference between:
		- re-host
		- re-platform
		- re-architect


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
		- CloudWatch alarms trigger against a metric threshold
			- State changes do *not* trigger CloudWatch alarms
		- CloudWatch Events rules can trigger state changes
	- Cognito
		- Fully managed solution providing access control and authentication for web/mobile apps
		- Features
			- Supports MFA
			- Data at-rest and in-transit encryption
			- Log in via social identity providers
				- Facebook, Google, AWS
			- Support for SAML	
		- Core concepts
			- User pools
				- Provides directory profile for all users which you access thru SDK
				- Supports user federation through 3rd party identity provider
				- Signed users receive authentication tokens
				- Tokens can be exchanged for AWS access via Cognito identity pools
			- Identity pools
				- Authenticates users with web identity providers, including Cognito user pools
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
* Security requirements and controls
	- IAM Users and Groups
		- policies and roles
		- AWS STS
	- AWS Service Roles
		- for AWS services interacting with the account
			- i.e. Lambda needs a service role
	- Identity providers
		- SAML 2.0, SSO, OpenID Connect
		- Cognito, AWS Directory Service
	- Understand how to assume a role:
		- cross account
		- in same account
	- Security logging
		- segregated bucket
		- dedicated account
* Best practice: security logging
	- Have a separate AWS account just for "security"
		- where logs are written to S3 bucket in the "security" account
		- only give "security" personnel access to security account
	- Centralize collecting of logs to a dedicated account, if possible
	- Send all CloudTrail logs to S3 for storage and log retention
		- Enable CloudTrail log file integrity validation
	- Enforce least privilege principle
	- Enforce MFA delete on S3 bucket


### Migration Planning

* Focus areas:
	- Existing workloads and processes for potential migration to the cloud
	- Migration tools or services for new and migrated solutions based on detailed AWS knowledge
	- Strategies for migrating existing on-premises workloads to the cloud
	- New cloud architectures for existing solutions
* The 6 R's of migration:
	1. Retain: keep as-is
	2. Re-host: lift and shift
	3. Refactor: re-architect
	4. Re-platform: life and shift with some tinkering
	5. Replace
	6. Retire
* Application migration process
	- Plan
		- Discover
			- Assessment and profiling
			- Data requirements and classification
			- Prioritization
			- Business logic and infrastructure dependencies
		- Design
			- Detailed migration plan
			- Estimate effort
			- Security and risk assessment
	- Build
		- Transform
			- Network topology
			- Migrate
			- Deploy
			- Validate
		- Transition
			- Pilot testing
			- Transition to support
			- Release management
			- Cutover and decommission
	- Run
		- Operate
			- Staff training
			- Monitoring
			- Incident management
			- Provisioning
		- Optimize
			- Monitoring-driven optimization
			- CI/CD
			- Well Architected framework
* Migration tools for assistance
	- Migration Hub:
		- Application Discovery Service
		- Database Migration Service
		- Server Migration Service
			- automates migration of your on-premises VM servers
				- VMware vSphere
					- Requires vCenter
				- Microsoft Hyper-V/SCVMM
				- Azure VMs
			- SMS incrementally replicates your server VMs as AMIs ready for deployment on Amazon EC2
			- Getting started:
				- You need to install the Server Migration Connector to your on-prem environment
					- the connector is what copies VM snapshots from on-prem to AWS cloud
	- [AWS Management Portal for vCenter Server](https://aws.amazon.com/ec2/vcenter-portal/)
		- Portal installs as a vCenter Server plug-in within your existing vCenter Server environment
		- It enables you to migrate VMware VMs to Amazon EC2 and manage AWS resources from within vCenter Server
		- IMPORTANT:  AWS recommends using AWS Server Migration Service to migrate VMs from a vCenter environment
			- SMS automates the migration by replicating on-premises VMs incrementally and converting them to AMIs
			- You can continue using your on-premises VMs while migration is in progress
			- You should only use AWS Management Portal for vCenter Server if you want to manage EC2 resources from within vSphere Client
	- VMWare Cloud on AWS
		- [VMware Cloud on AWS](https://cloud.vmware.com/vmc-aws)
		- [VMware Cloud on AWS - Overview](https://youtu.be/wibLL71pOBk)
* AWS Storage Portfolio
	- Block
		- EBS
		- Ephemeral storage
	- File
		- EFS
	- Object
		- S3
		- Glacier
	- Data transfer
		- Snowball
		- Storage Gateway
		- Direct Connect (DX)
		- Kinesis
* Data migration
	- Consider downtime/orchestration
		- The more downtime you are allowed, the simpler/cheaper the migration can be
	- Methods:
		- image backup/restore
		- file copy
		- replication
* Hybrid cloud architectures and connectivity
	- Internet
	- Direct Connect (DX)
	- VPN	


### Cost Control

* Focus areas:
	- Cost-effective pricing models
	- Designing and implementing controls to ensure cost optimization
	- Opportunities to reduce cost in an existing solution
* Change your way of thinking about resources...
	- Instead of "paying for what you *think* you need" you should frame solutions around "paying for what you *actually* need"
* You can only manage what you measure
	- what to measure
	- who/what can measure
	- who/what can see measurements
* Limit resource provisioning
* Tag everything!
	- Categories: name, group, costcenter, use, etc...
	- Types of tags:
		- Resource tags
			- provide ability to organize and search within and across resources
			- filterable and searchable
			- do *not* appear in deatiled billing report
		- Cost allocation tags
			- map AWS charges to organizational attributes for accounting purposes
			- information presented in the detailed billing report and Cost Explorer (must be explicitly selected)
			- only available on certain services or limited to components within a service
				- e.g. can be specified at S3 bucket level, but not at S3 object level
* Best practices:
	- Only allow specific groups/teams to deploy chosen AWS resources
	- Create policies for each environment
	- Require tags in order to instantiate resources
	- Monitor and send alerts or shut down instances that are improperly tagged
	- Use CloudWatch to send alerts when billing thresholds are met
	- Analyze spend using AWS or partner tools
		- Cost Explorer
		- Cost and Utilization Report
		- QuickSight integration for visualizing reports


### Improving Existing Architectures

* 5.1. Troubleshoot solution architectures
	- S3 Server Access Logs
		- contains details about data requests, such as:
			- request type
			- resources requested
			- timestamp of request
	- ELB Access Logs
		- information about each request made to ELB, including:
			- client IP
			- latencies
			- server response
	- CloudTrail
		- provides history of API calls to your account
	- VPC Flow Logs
		- capture information about IP traffic going into or out of your network interfaces and subnets
	- CloudWatch Logs
		- monitor, store, and access applications and systems using log data from EC2 instances and on-prem servers
			- *NOTE* you can import logs from on-prem servers into CloudWatch for viewing/analysis
	- AWS Config
		- provide an inventory of AWS resources and records changes to the configuration of those resources
* 5.2. Improve an existing solution for operational excellence
	- See: "Operational Excellence" pillar of Well Architected framework
	- Understand business and customer needs
	- Make frequent, small and reversible changes
		- You want "two-way doors", *not* one-way doors
	- Create and use procedures to respond to operational events
	- Continuously improve supporting processes and procedures
	- Key tools:
		- Trusted Advisor
			- define operational priorities
			- looks at ~50 key operational elements spread over the following categories:
				- Cost optimization
				- Performance
				- Security
				- Fault tolerance
				- Service limits
		- CloudFormation
			- design for operations
		- Systems Manager
			- operational readiness
* 5.3. Improve the reliability of an existing solution
	- Operational continuum
		- High Availability spectrum
			- Manual recovery -> High availability -> Fault tolerant
		- Disaster Recovery spectrum
			- Backup/recover -> Pilot light -> Warm standby -> Multi-site
	- Architect for AZ failure
		- Use multi-AZ services
			- e.g. S3, DynamoDB
		- Plan for capacity constraints
		- Use Reserved Instances for critical systems
		- Identify all AZ-specific services, noting which are regional/global
		- Use EBS snapshots to help minimize the RPO
* 5.4. Improve the performance of an existing solution
	- Ways of improving performance
		- Shorten response times
		- Increase throughput
		- Lower the utilization of resources (efficiency)
		- Increase scalability for workloads that burst
	- Tools to use to increase performance:
		- S3
			- Move static content to S3
			- Use IA for infrequently accessed data
			- Larger objects reduce PUT/GET requests
		- EBS
			- GP2 for system disks
			- SC1 for cold storage
			- PIOPS for high performance random I/O
			- ST1 for high performance sequential I/O
		- RDS
			- Scale up instance size
			- Increase storage size online
			- Employ caching layers
			- RDS read replicas
				- async
				- apps must be updated to use replica endpoints
				- cross-region option (for some RDS flavors)
		- ElastiCache
			- Be aware of cache timeouts/TTLs
			- Redis replication groups for HA
			- Employ write-through cache for write spikes
		- DynamoDB
			- Alter read/write capacity units
			- Use global or local secondary indexes
			- Use SQS for write spikes
			- Write data in quiet periods
		- Loosely coupled architecture
			- ELB, SQS, SNS, Kinesis, Auto Scaling
* 5.5. Improve the security of an existing solution
	- Restrict access to resources
		- IAM
			- User-based policies
			- Resource-based policies
			- Policy conditions
	- Data encryption at rest
		- S3
			- SSE-C, SSE-KMS, SSE-S3
		- EBS
			- KMS
		- DynamoDB
			- Encryption client, SSE-KMS
		- RDS
			- KMS, TDE (transparent data encryption)
		- Redshift
			- KMS, CloudHSM
		- SQS/SNS
			- KMS
	- Protect data in-transit
		- SSL termination at the load balancer
			- Certificates are stored in IAM
			- Single certificate per load balancer
			- Offload decryption work to load balancer
			- Re-encryption between load balancer and instances
			- ALB vs classic load balancer
		- SSL termination in CloudFront
			- Using both SNI or non-SNI certificates
			- SSL connections to ELB
	- Improve network protection
		- Network perimeter controls
			- Security groups
				- Per-ENI granularity
				- Stateful
				- Inter-service communication
			- Network ACLs
				- Subnet boundaries only
				- ALLOW and DENY rules
				- IP ranges only
			- Host firewalls
				- Central or distributed control
				- IDS, IPS
* 5.6. Improve the deployment of an existing solution
	- CloudFormation
		- Setup, Configure
			- Stack
			- AWS::CloudFormation::Init
			- CreationPolicy/WaitCondition
		- Deploy
			- UserData
			- Stack updates
			- Update change sets
		- Undeploy, Shutdown
			- Delete stack
			- Consider DeletionPolicy attribute
	- CodeDeploy
		- Deploy
			- Applications and revisions
			- Deployment groups
			- Lifecycle hooks
	- BeanStalk
		- Setup, Configure
			- EC2, Auto Scaling, ELB
			- .ebextensions: configuration/customization
			- leader_only: single operation updates
		- Deploy
			- multiple app versions
			- deployment to the fleet
			- CNAME swaps/zero downtime
		- Undeploy, Shutdown
			- remove an environment
	- OpsWorks
		- Hosted Chef


## Test Preparation Services
* A Cloud Guru
	- $29/month
	- [AWS Certified Solutions Architect – Professional](https://acloud.guru/learn/aws-certified-solutions-architect-professional-2019)
* Linux Academy
	- $49/month
	- [AWS Certified Solutions Architect – Professional](https://linuxacademy.com/course/aws-certified-solutions-architect-professional-2018/)
* Whizlabs
	- Practice exams, 400 questions, $30
	- [AWS Certified Solutions Architect Professional](https://www.whizlabs.com/aws-solutions-architect-professional/)


## Whitepapers
* [Architecting for the Cloud - AWS Best Practices, October 2018](https://d1.awsstatic.com/whitepapers/AWS_Cloud_Best_Practices.pdf)
* [Amazon Web Services: Overview of Security Processes whitepaper, May 2017](https://d1.awsstatic.com/whitepapers/Security/AWS_Security_Whitepaper.pdf)
* [AWS Security Best Practices whitepaper, August 2016](https://d1.awsstatic.com/whitepapers/Security/AWS_Security_Best_Practices.pdf)
* [Microservices on AWS](https://docs.aws.amazon.com/whitepapers/latest/microservices-on-aws/introduction.html)
* [Using Amazon Web Services for Disaster Recovery, October 2014](https://d1.awsstatic.com/whitepapers/aws-disaster-recovery.pdf)
* [An Overview of AWS Cloud Data Migration Services, May 2016](https://d1.awsstatic.com/whitepapers/Storage/An_Overview_of_AWS_Cloud_Data_Migration_Services.pdf)
* [Storage Options in the AWS Cloud, October 2013](https://d1.awsstatic.com/whitepapers/Storage/aws-storage-options.pdf)
	- Pay attention to the "Anti-patterns" sections for each option
* [Practicing Continuous Integration and Continuous Delivery on AWS, June 2017](https://d1.awsstatic.com/whitepapers/DevOps/practicing-continuous-integration-continuous-delivery-on-AWS.pdf)


## AWS Training Courses
* [Exam Readiness: AWS Certified Solutions Architect – Professional](https://www.aws.training/Details/eLearning?id=34737)
* [Deep Dive into Amazon Simple Storage Service - Amazon S3](https://www.aws.training/Details/Curriculum?id=26930)
* [Deep Dive into Amazon Glacier](https://www.aws.training/Details/Curriculum?id=19093)
* [Deep Dive into AWS Storage Gateway](https://www.aws.training/Details/Curriculum?id=19403)
* [Migrating and Tiering Storage to AWS](https://www.aws.training/Details/eLearning?id=16368)
* [Deep Dive into Amazon Elastic Block Store - EBS](https://www.aws.training/Details/Curriculum?id=26940)


## re:Invent Videos
* [AWS re:Invent 2016: Amazon Global Network Overview with James Hamilton](https://www.youtube.com/watch?v=uj7Ting6Ckk)
* [AWS re:Invent 2017: Networking Many VPCs: Transit and Shared Architectures (NET404)](https://www.youtube.com/watch?v=KGKrVO9xlqI)
* [AWS re:Invent 2017: Deep Dive: AWS Direct Connect and VPNs](https://youtu.be/eNxPhHTN8gY)
* [AWS re:Invent 2017: Deep Dive on AWS CloudFormation](https://youtu.be/01hy48R9Kr8)
* [AWS re:Invent 2018: Aurora Serverless: Scalable, Cost-Effective Application Deployment](https://www.youtube.com/watch?v=4DqNk7ZTYjA)
* [AWS re:Invent 2015: A Technical Introduction to Amazon EMR](https://youtu.be/WnFYoiRqEHw)

## Other Resources
* [Self Paced Labs](http://aws.amazon.com/training/self-paced-labs)
* [AWS Documentation for services](https://docs.aws.amazon.com/index.html)
* [AWS Architecture Center](https://aws.amazon.com/architecture/)
* [AWS Solutions](https://aws.amazon.com/solutions/)
* [On-Demand AWS Tech Talks](https://aws.amazon.com/about-aws/events/monthlywebinarseries/on-demand/)
* [Best Practices for Amazon RDS](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/CHAP_BestPractices.html)
* [Managed PostgreSQL Databases on AWS](https://youtu.be/hdQ-geGBsq4)
* [Solution Architect Pro - Learning Path](http://jayendrapatil.com/aws-certified-solution-architect-professional-exam-learning-path/)
