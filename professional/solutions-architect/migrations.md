# Migrations


## General

* Focus areas
	- Existing workloads and processes for potential migration to the cloud
	- Migration tools or services for new and migrated solutions based on detailed AWS knowledge
	- Strategies for migrating existing on-premises workloads to the cloud
	- New cloud architectures for existing solutions
* Migration strategies
	- The 6 R's of migration:
		- Re-Host
			- "lift and shift"
			- move assets from on-prem to cloud with no change
		- Re-Platform
			- "lift and reshape"
			- move assets to cloud but change underlying platform
		- Replace (Purchase)
			- "drop and shop"
		- Refactor (or Re-architect)
			- most $$$ option
			- redesign application in cloud-native manner
		- Retire
			- get rid of apps not needed
		- Retain
			- do nothing
	- Make sure you understand the difference between:
		- Re-host
		- Re-platform
		- Re-architect


## Frameworks

* What is a framework?
	- Helps you get your mind around problem
	- Open for localization and interpretation
	- Not a perfect step-by-step recipe for success
* TOGAF
	- The Open Group Architectural Framework
	- Approach for designing, planning, implementing and governing enterprise IT architectures
	- Favored by Forture 500
	- Not a cookbook
		- It's a framework, guidelines - not absolutes
* Cloud Adoption Framework
	- Business
		- Create a strong business case for cloud adoption
		- Business goals align with cloud objectives
		- Ability to measure results (TCO, ROI)
	- People
		- Evaluate organizational roles and structures, new skills and identify gaps
		- Career management aligned with evolving roles
		- Training options appropriate for learning styles
	- Governance
		- Portfolio management to determine cloud eligibility and priority
		- Align KPIs with newly enabled business capabilities
	- Platform
		- Architecture patterns adjusted to leverage cloud-native
		- New development skills and processes enable more agility
	- Security
		- Identity and access management modes change
		- Logging and auditing capabilities will evolve
		- Shared Responsibility Model adds/subtracts some facets
	- Operations
		- Service monitoring has potential to be fully automated
		- Performance management
		- Business continuity and disaster recovery options


## Application Migration Process

* Plan
	- Discover
		- Assessment and profiling
		- Data requirements and classification
		- Prioritization
		- Business logic and infrastructure dependencies
	- Design
		- Detailed migration plan
		- Estimate effort
		- Security and risk assessment
* Build
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
* Run
	- Operate
		- Staff training
		- Monitoring
		- Incident management
		- Provisioning
	- Optimize
		- Monitoring-driven optimization
		- CI/CD
		- Well Architected framework


## Hybrid Architectures

* Storage Gateway as bridge between on-prem and AWS
	- Seamless to end-users
	- Common first step into cloud since low risk and appealing economics
* Middleware integrations
	- E.g. On-prem ERP system that sends SQS messages for post processing into DynamoDB
	- Loosely-coupled
* Transparent migrations of VMs
	- VMware vCenter plugin allows for migration of VMs to and from AWS
	- VMware Cloud furthers this concept with more public cloud-native features


## Network Migrations

* Start with VPN connection to AWS
* As usage grows, start using Direct Connect
	- But keep VPN as backup
	- Transition from VPN to Direct Connect seamless via BGP
		- Configure both VPN and Direct Connect within the same BGP prefix
	- From on-prem side, make sure DX path (instead of VPN) is always preferred, through BGP weighting or static routes


## Data Migrations

* Data migration
	- Consider downtime/orchestration
		- The more downtime you are allowed, the simpler/cheaper the migration can be
	- Methods:
		- image backup/restore
		- file copy
		- replication
* Choose appropriate storage options
	- Ensure you have a comprehensive understanding of the entire AWS storage portfolio.
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


## Migration Tools

* Storage Migration
	- AWS Storage Gateway
	- Snowball
* Server Migration Service
	- Automates migration of your on-premises VM servers
		- VMware vSphere
			- Requires vCenter
		- Microsoft Hyper-V/SCVMM
		- Azure VMs
	- Incrementally replicates server VMs to AWS, syncing volumes and creating periodic AMIs
	- Minimizes cutover downtime by syncing VMs incrementally
	- Getting started:
		- You need to install the Server Migration Connector to your on-prem environment
			- Server Migration Connector is downloaded as virtual appliance into your on-prem vSphere or Hyper-V setup
			- The connector is what copies VM snapshots from on-prem to AWS cloud
* Database Migration Service
	- Works along with Schema Conversion Tool (SCT)
	- SCT can copy database schemas for homogeneous migrations and convert for heterogeneous migrations
	- DMS used for smaller, simpler conversions
		- Also supports MongoDB and DynamoDB
	- SCT used for larger, more complex databases
	- DMS has replication function for on-prem to AWS or to Snowball or S3
* Application Discovery Service
	- Gathers information about on-prem data centers to help with migration planning
	- Helps produce full inventory/status of data center assets
	- Collects config, usage, and behavior data from your servers to help estimate TCO of running in AWS
	- Can run agent-less (VMware environment) or agent-based (non VMware)
* AWS Migration Hub
	- Rolls up all the migration services and tools into one place in the AWS Console
		- Application Discovery Service
		- Database Migration Service
		- Server Migration Service
* VMware Support
	- [AWS Management Portal for vCenter Server](https://aws.amazon.com/ec2/vcenter-portal/)
		- Portal installs as a vCenter Server plug-in within your existing vCenter Server environment
		- It enables you to migrate VMware VMs to Amazon EC2 and manage AWS resources from within vCenter Server
		- IMPORTANT:  AWS recommends using AWS Server Migration Service to migrate VMs from a vCenter environment
			- SMS automates the migration by replicating on-premises VMs incrementally and converting them to AMIs
			- You can continue using your on-premises VMs while migration is in progress
			- You should only use AWS Management Portal for vCenter Server if you want to manage EC2 resources from within vSphere Client
	- VMware Cloud on AWS
		- [VMware Cloud on AWS](https://cloud.vmware.com/vmc-aws)
		- [VMware Cloud on AWS - Overview](https://youtu.be/wibLL71pOBk)


## Links

* [Whitepaper: AWS Migration Whitepaper](https://d1.awsstatic.com/whitepapers/Migration/aws-migration-whitepaper.pdf)
* [Whitepaper: An Overview of the AWS Cloud Adoption Framework](https://d1.awsstatic.com/whitepapers/aws_cloud_adoption_framework.pdf)
* [AWS re:Invent 2017: Migrating Databases and Data Warehouses to the Cloud](https://www.youtube.com/watch?v=Y33TviLMBFY)
* [AWS Data Pipeline](https://aws.amazon.com/datapipeline/)
