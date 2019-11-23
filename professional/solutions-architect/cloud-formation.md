# CloudFormation

* CloudFormation Concepts
	- Templates
		-JSON/YAML file that contains instructions for building out AWS resources
	- Stacks
		- Entire env described by template and created/updated/deleted as single unit
	- Change sets
		- Summary of proposed changes to stack that allows you to see how changes impact existing resources before applying
	- Make sure to understand:
		- Mappings
		- Conditions
		- How do you test change sets in CloudFormation?
* Stack Policies
	- Protect specific resources within stack from being unintentionally deleted/updated
	- Add Stack Policy via console or CLI when creating
	- Add Stack Policy to existing stack only via CLI
	- Once applied, Stack Policy cannot be removed - but can be modified
	- By default, Stack Policy protects against everything - you must specifically add allow rules

