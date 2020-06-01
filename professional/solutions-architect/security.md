# AWS Security

## AWS Shared Responsibility Model

* Customer
	- Responsible for security "IN" the cloud
	- E.g.
		- Customer data
		- Platform, apps, identity and access management
		- OS, network, firewall 
		- Encryption
* AWS
	- Responsible for security "OF" the cloud
	- E.g.
		- Compute, Storage, Database, Networking
		- Infrastructure
		- Regions, AZs, edge locations


## Security Facets

* Identity
	- "Who are you?"
* Authentication
	- Prove your claimed identity
* Authorization
	- Are you allowed to do this?
* Trust
	- Do other entities that I trust say they trust you?
		- e.g. Cross-account access, SAML-based federation


## Data Protection

* [Encryption Deep Dive](../../deep-dives/encryption/)
* AWS Secrets Manager
	- Store passwords, encryption keys, API keys, SSH keys, PGP keys, etc.
	- Alternative to storing secrets in a "vault"
	- Can access secrets via API with fine-grained access control provided by IAM
	- Automatically rotate RDS credentials for MySQL, PostgreSQL, and Aurora


## Authentication and Authorization

* SAML 2.0
	- Handles both authorization and authentication
	- XML-based protocol
	- Can contain user, group membership and other useful information
	- Assertions in the XML for authentication, attributes or authorization
	- Best suited for SSO for enterprise users
* OAuth 2.0
	- Allow sharing of protected assets without having to send login credentials
	- Handles authorization only, NOT authentication
	- Issues token to client
	- Application validates token with Authorization Server
	- Delegate access, allowing client apps to access information on behalf of user
	- Best suited for API authorization between apps
* OpenID Connect
	- Identity layer build on top of OAuth 2.0, adding authentication
	- Uses REST/JSON message flows
	- Best suited for SSO for consumers


## Multi-Account Management

* Most large organizations will have multiple AWS accounts
	- Why? Segregation of duties, cost allocation and increased agility
* Need methods to properly manage and maintain them
* Tools for account management
	- AWS Organizations
	- Service Control Policies
	- Tagging
	- Resource Groups
	- Consolidated Billing
* Use cases for multi-account scenarios
	- Identity Account Structure
		- Manage all user accounts in one location
		- Users trust relationship from IAM roles in sub-accounts to Identity Account to grant temporary access
	- Logging Account Structure
		- Centralized logging repository
		- Can be secured and made immutable
		- Can use SCP to prevent sub-accounts from changing logging settings
	- Publishing Account Structure
		- Common repository for AMIs, containers, code
* AWS Organizations
	- Centrally manage billing
	- Control access, compliance, and security
		- Implement Service Control Policy (SCP) permission guardrails
		- Configure central logging of all actions performed across your organization using AWS CloudTrail and centrally aggregate data
		- Use AWS Config to audit your environment for compliance
	- Share resources across your AWS accounts
	- API for creating new accounts
	- Organizational units (OUs)
		- logical grouping of accounts
		- enable you to organize your accounts into a hierarchy
		- make it easier for you to apply management controls
	- OU best practices
		- Create set of foundational OUs
			- Infrastructure
				- Used for shared infrastructure services such as networking and IT services
				- Create accounts for each type of infrastructure service you require
			- Security
				- Used for security services
				- Create accounts for logs, security read-only access, security tooling, etc
	- Service Control Policy (SCP)
		- Policy that defines the AWS service actions that accounts in your organization can perform
		- Acts as permission guardrails
		- Attach to:
			- Root account - applies to all accounts in your organization)
			- Individual OU - applies to all accounts in the OU including nested OUs
			- Individual account
		- Best practice:
			- Configure CloudTrail logging to be enabled for all accounts under Organization
			- All logs go to a single (locked-down) account
			- Use SCP to prevent accounts from disabling CloudTrail logging


## Directory Services

* Types of Directory Services
	- AWS Cloud Directory
		- Cloud-native directory to share and control access to hierarchical data between apps
	- Cognito
	- AWS Directory Service for MS AD
		- AWS-managed full Microsoft AD
	- AD Connector
		- Allow on-prem users to log into AWS services using their existing AD credentials via IAM roles
		- Allow EC2s to join AD domain
		- You must have an existing AD
		- Supports MFA via existing RADIUS-based MFA instrstructure
	- Simple AD
		- Low-scale, low-cost AD implementation based on Samba
		- Supports user accounts, groups, group policies and domains
		- Kerberos-based SSO
		- MFA *not* supported
		- No trust relationships


## Attacks

* Mitigating DDoS
	- Minimize attack surface
		- NACLs, security groups, VPC design
	- Scale to absorb attack
		- Auto Scaling groups, CloudFront, static websites via S3
	- Safeguard exposed resources
		- Route 53 (like regionalized routes), WAF, Shield
	- Learn normal behavior
		- GuardDuty, CloudWatch
	- Have a plan
* Intruder Prevention and Detection
	- IDS (Intruder Detection System)
		- watches network and systems for suspicious activity
	- IPS (Intruder Prevention System)
		- more active solution than IDS
		- tries to prevent exploits by sitting behind firewalls and scanning/analyzing suspicious activity
	- Normally comprised of collection/monitoring system and agents on each host
	- Logs collected and analyzed


## AWS Service Catalog

* Framework allowing admins to create pre-defined products and landscapes for their users
* Granular control over which users have access to which offerings
* Makes use of adopted IAM roles so users don't need underlying service access
* Allows end users to be self-sufficient while upholding enterprise standards for deployment
* Based on CloudFormation templates
* Admins can version and remove products
	- Existing running product versions will not be shutdown
* Service Catalog constraints
	- Three types of constraints
		- Launch constraint
			- IAM role that Service Catalog assumes when an end-user launches a product
			- Without launch constraint, the end-user would need all permissions within their own IAM credentials
		- Notification constraint
			- Specifies the SNS topic for stack events
			- Can get notified when products are launched or have problems
		- Template constraint
			- one or more rules that narrow allowable values an end-user can select
			- adjust product attributes based on choices a user makes 
* Multi-Account scenarios
	- You can share Service Catalog portfolios among separate AWS accounts


## Links

* [AWS re:Invent 2017: IAM Policy Ninja](https://www.youtube.com/watch?v=aISWoPf_XNE)
* [AWS re:Invent 2017: Security Anti-Patterns: Mistakes to Avoid](https://www.youtube.com/watch?v=tzJmE_Jlas0)
* [AWS re:Invent 2017: Best Practices for Managing Security Operations on AWS](https://www.youtube.com/watch?v=gjrcoK8T3To)
* [AWS re:Inforce 2019: Managing Multi-Account AWS Environments Using AWS Organizations](https://www.youtube.com/watch?v=fxo67UeeN1A)
