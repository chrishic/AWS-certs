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
* Prepare for lots of studying
	- Some people that have passed the exam report 350+ hours of preparation over a period of 3 months
	- Make sure you have hands-on practice with labs and real-world experience
	- Spend at least 30-40 hours taking practice exams
* Review the [Exam Guide](https://d1.awsstatic.com/training-and-certification/docs-sa-pro/AWS_Certified_Solutions_Architect_Professional-Exam_Guide_EN_1.2.pdf) and [Sample Questions](https://d1.awsstatic.com/Train%20%26%20Cert/docs/AWS_certified_solutions_architect_professional_examsample.pdf)
* Take the [AWS training courses](#aws-training-courses) listed below 
* [A Cloud Guru preparation course](https://acloud.guru/learn/aws-certified-solutions-architect-professional)
* [SA-Pro 2020 by Stephane Maarek](https://www.udemy.com/course/aws-solutions-architect-professional/)
* [Linux Academy preparation course](https://linuxacademy.com/course/aws-certified-solutions-architect-professional-2018/)
* Review the following deep dives:
	- [Well Architected Framework deep dive](../../deep-dives/well-architected-framework/)
	- [AWS Encryption deep dive](../../deep-dives/encryption/)
	- [Amazon Simple Storage Service (S3) Deep Dive](../../deep-dives/s3/)
	- [VPC Networking Deep Dive](../../deep-dives/networking/)
	- [DynamoDB Deep Dive](../../deep-dives/dynamodb/)
* Review the [whitepapers](#whitepapers)
* Review FAQs
* Take practice exams with 3rd party providers
	- Exam simulators
		- A Cloud Guru exam simulator
		- [Tutorials Dojo / Jon Bonso exam simulator](https://portal.tutorialsdojo.com)
	- Other question banks
		- [Whiz Labs SA-Pro practice questions](https://www.whizlabs.com/aws-solutions-architect-professional/)
* Take the official "AWS Certified Solutions Architect – Professional" practice exam
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


## Domains

### Design for Organizational Complexity

* 1.1. Cross-account authentication and access strategies
	- Know IAM inside-and-out
		- User-based access controls
			- Users, groups, roles
		- Resource-based access controls
		- IAM policies
	- Cross account delegation
		- Via Secure Token Service (STS) to generate temporary access credentials
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
		- Fan out to SQS
	- ElastiCache
		- Redis
			- persistent, auto failover with multi-AZ, can scale up but not down, scale with read replicas
		- Memcached
			- in-memory only
			- can scale both up and out
			- scales out using multiple nodes
	- Kinesis
		- Kinesis Streams
			- Ordered within shard, async, "at least once" semantics
			- Single direction only
			- Custom processing per incoming record
			- Sub-second processing latency
		- Kinesis Firehose
			- Zero admin
			- Processing latency of at least 60 seconds
			- Ability to use existing analytics tools such as S3, Redshift
		- Know difference between Data Streams and Data Firehose
			- Streams
				- Sub-second processing
				- Custom processing per record
			- Firehose
				- Latency of 60 seconds or higher
				- Zero admin
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


### [Migration Planning](./migrations.md)


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
			- do *not* appear in detailed billing report
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
		- Understand RTO (recovery time objective), RPO (recovery point objective)
		- Disaster recovery options
			- Backup and Restore
				- least expensive
				- longest time to recover
				- use S3 to quickly restore workloads to EC2/EBS
			- Pilot Light
				- have minimal resources ready to go
				- can be quickly grown if need arises
				- replication is typically the only running component
			- Warm Standby
				- all services up and ready to go (scaled down version of full environment)
				- can be used as shadow environment
			- Multi-Site
				- ready all the time to take full production load
				- effectively a mirrored data center
				- fail over in seconds or less
				- Understand difference between HA and fault tolerant
		- Understand what each offers in terms of RTO, RPO
	- Architect for AZ failure
		- Use multi-AZ services
			- e.g. S3, DynamoDB
		- Be aware of single-AZ services
			- Redshift does not support multi-AZ deployments
				- For best HA, you must use multi-node cluster with replication
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
			- ST1 for high performance sequential I/O (thruput)
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
				- Memcached is *not* HA
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
	- Types of Deployments
		- Rolling
		- A/B deployments
		- Canary deployments
		- Blue/green deployments
			- emphasize immutable infrastructure
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
		- Three flavors
			- Hosted Chef
			- Hosted Puppet
			- Stacks (AWS-created using Chef Solo)


## Test Preparation Services
* A Cloud Guru
	- $49/month
	- [AWS Certified Solutions Architect – Professional](https://acloud.guru/learn/aws-certified-solutions-architect-professional)
* Linux Academy
	- $49/month
	- [AWS Certified Solutions Architect – Professional](https://linuxacademy.com/course/aws-certified-solutions-architect-professional-2018/)
* SA-Pro 2020 course by Stephane Maarek (Udemy course)
	- [Ultimate AWS Certified Solutions Architect Professional 2020 - Stephane Maarek - Udemy](https://www.udemy.com/course/aws-solutions-architect-professional/)
* Whizlabs
	- Practice exams, 400 questions, $30
	- [AWS Certified Solutions Architect Professional](https://www.whizlabs.com/aws-solutions-architect-professional/)
* Tutorials Dojo 
	- Practice exams, 300 questions, $15 (Udemy course)
	- [AWS Certified Solutions Architect Professional Practice Exam - Jon Bonso / Tutorials Dojo](https://www.udemy.com/course/aws-solutions-architect-professional-practice-exams-amazon/)


## Whitepapers
* [AWS Well-Architected Framework, July 2019](https://d1.awsstatic.com/whitepapers/architecture/AWS_Well-Architected_Framework.pdf)
* [Architecting for the Cloud - AWS Best Practices, October 2018](https://d1.awsstatic.com/whitepapers/AWS_Cloud_Best_Practices.pdf)
* [Microservices on AWS](https://docs.aws.amazon.com/whitepapers/latest/microservices-on-aws/introduction.html)
* [Amazon Web Services: Overview of Security Processes whitepaper, March 2020](https://d1.awsstatic.com/whitepapers/Security/AWS_Security_Whitepaper.pdf)
* [Using Amazon Web Services for Disaster Recovery, October 2014](https://d1.awsstatic.com/whitepapers/aws-disaster-recovery.pdf)
* [Storage Options in the Cloud](https://d1.awsstatic.com/whitepapers/Storage/AWS%20Storage%20Services%20Whitepaper-v9.pdf)
* [Backup and Recovery Approaches Using AWS](https://d1.awsstatic.com/whitepapers/Storage/Backup_and_Recovery_Approaches_Using_AWS.pdf)
* [AWS Security Best Practices whitepaper, August 2016](https://d1.awsstatic.com/whitepapers/Security/AWS_Security_Best_Practices.pdf)
* [Overview of AWS Security - Network Security](https://d1.awsstatic.com/whitepapers/Security/Networking_Security_Whitepaper.pdf)
* [An Overview of AWS Cloud Data Migration Services, May 2016](https://d1.awsstatic.com/whitepapers/Storage/An_Overview_of_AWS_Cloud_Data_Migration_Services.pdf)
* [Overview of Deployment Options on AWS](https://d1.awsstatic.com/whitepapers/overview-of-deployment-options-on-aws.pdf)
* [Practicing Continuous Integration and Continuous Delivery on AWS, June 2017](https://d1.awsstatic.com/whitepapers/DevOps/practicing-continuous-integration-continuous-delivery-on-AWS.pdf)
* [An Overview of the AWS Cloud Adoption Framework, Version 2 - February 2017](https://d1.awsstatic.com/whitepapers/aws_cloud_adoption_framework.pdf)
* [Amazon Virtual Private Cloud Connectivity Options](https://d0.awsstatic.com/whitepapers/aws-amazon-vpc-connectivity-options.pdf)
* [Getting Started with Amazon Aurora](https://d1.awsstatic.com/whitepapers/getting-started-with-amazon-aurora.pdf)
* [Performance at Scale with Amazon ElastiCache](https://d0.awsstatic.com/whitepapers/performance-at-scale-with-amazon-elasticache.pdf)


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
* [AWS re:Invent 2017: Another Day, Another Billion Flows](https://www.youtube.com/watch?v=8gc2DgBqo9U)
* [AWS re:Invent 2017: Deep Dive: AWS Direct Connect and VPNs](https://youtu.be/eNxPhHTN8gY)
* [AWS re:Invent 2017: Deep Dive on AWS CloudFormation](https://youtu.be/01hy48R9Kr8)
* [AWS re:Invent 2017: ElastiCache Deep Dive: Best Practices and Usage Patterns](https://www.youtube.com/watch?v=_YYBdsuUq2M)
* [AWS re:Invent 2017: Deep Dive: Using Hybrid Storage with AWS Storage Gateway](https://www.youtube.com/watch?v=9wgaV70FeaM)
* [AWS re:Invent 2017: Security Anti-Patterns: Mistakes to Avoid](https://www.youtube.com/watch?v=tzJmE_Jlas0)
* [AWS re:Invent 2017: Best Practices for Managing Security Operations on AWS](https://www.youtube.com/watch?v=gjrcoK8T3To)
* [AWS re:Invent 2017: IAM Policy Ninja](https://www.youtube.com/watch?v=aISWoPf_XNE)
* [AWS re:Invent 2017: Scaling Up to Your First 10 Million Users](https://www.youtube.com/watch?v=w95murBkYmU)
* [AWS re:Invent 2017: Elastic Load Balancing Deep Dive and Best Practices](https://www.youtube.com/watch?v=9TwkMMogojY)
* [AWS re:Inforce 2019: Managing Multi-Account AWS Environments Using AWS Organizations](https://www.youtube.com/watch?v=fxo67UeeN1A)
* [AWS re:Invent 2018: Aurora Serverless: Scalable, Cost-Effective Application Deployment](https://www.youtube.com/watch?v=4DqNk7ZTYjA)
* [AWS re:Invent 2015: A Technical Introduction to Amazon EMR](https://youtu.be/WnFYoiRqEHw)


## Other Resources
* [AWS Architecture Center](https://aws.amazon.com/architecture/)
* [AWS Solutions](https://aws.amazon.com/solutions/)
* [Self Paced Labs](http://aws.amazon.com/training/self-paced-labs)
* [AWS Documentation for services](https://docs.aws.amazon.com/index.html)
* [On-Demand AWS Tech Talks](https://aws.amazon.com/about-aws/events/monthlywebinarseries/on-demand/)
* [Best Practices for Amazon RDS](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/CHAP_BestPractices.html)
* [Managed PostgreSQL Databases on AWS](https://youtu.be/hdQ-geGBsq4)
* [Solution Architect Pro - Learning Path](http://jayendrapatil.com/aws-certified-solution-architect-professional-exam-learning-path/)
* [Tutorials Dojo - AWS Cheat Sheets](https://tutorialsdojo.com/aws-cheat-sheets/)
