# To Do


## Action Items

* VPC peering
	- To have different VPCs talk with each other, do you just create the peering and then update the route tables on both VPCs?
* Play around with Systems Manager
* Setup VPN between on-premises machines and AWS VPC
* Storage Gateway
	- Gateway Stored vs Gateway Cached
* Techniques to allow mobile users to make calls to DynamoDB
	- Cognito // User pools vs identity pools // Security Token Service // Web Identity Provider
* CloudFormation
* Revisit S3 lifecycle policies and rules of when you can transition to Glacier
* Design a hybrid architecture using key AWS technologies (e.g., VPN, AWS Direct Connect)
* Architect a continuous integration and deployment process
* Blue/green deployments
* Understand cache population techniques
	- Write-through
	- Lazy loading	


## Items to Research

* CloudTrail 
	- log file integrity validation
* Elastic Beanstalk
	- Deployment types (immutable, rolling)
	- Does *not* support deleting application bundles after deploy
* CloudFront
	- CloudFront Trusted Signer
	- CloudFront-signed URL
* CloudFormation
	- Mappings
	- Conditions
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
	- AWS Application Discovery Service
	- AWS Database Migration Service
* Understand how EMR works
* CloudWatch Events rule
* Kibana and integration with Elasticsearch

