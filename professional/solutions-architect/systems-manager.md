# AWS Systems Manager

* AWS System Manager
	- SSM agent
		- Comes by default with Amazon AMIs
		- Can download and install for on-premises servers
* Services
	- Inventory
		- Collect OS, app and instance metadata
			- E.g. Which instances have Apache 2.2.x or earlier?
	- State Manager
		- Creates states that represent certain configurations
	- Logging
		- CloudWatch Log agent to stream logs directly to CloudWatch from instances
	- Parameter Store
		- Shared secure storage for config data, connection strings, etc.
	- Insight Dashboards
		- Account level view of CloudTrail, Config, Trust Advisor
	- Resource Groups
		- Group resources via tagging
	- Maintenance Windows
		- Define schedules for instances to patch, update apps, run scripts, etc
	- Automation
		- automate routine maintenance tasks with scripts
			- e.g. stop DEV and QA instances on Friday and restart Monday morning
	- Run Command
		- run commands/scripts without SSH
	- Patch Manager
		- automates process for patching instances with updates
	- Session Manager
* System Manager Documents
	- Command Document
		- Used with: Run Command, State Manager
	- Policy Document
		- Used with: State Manager
	- Automation Document
		- Used with: Automation
