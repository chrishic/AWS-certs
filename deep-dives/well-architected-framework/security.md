# Security 

* "Ability to protect information, systems, and assets while delivering business value through risk assessments and mitigation strategies"
* Key service: AWS IAM


## Design Principles

* Implement strong identity foundation
* Enable traceability
* Security at all layers
* Automate security best practices
* Protect data in transit and at rest
* Keep people away from data
* Prepare for security events


## Focus Areas

* Identity and access management
	- Services: IAM, AWS Organizations, MFA
* Detective controls
	- Services: CloudTrail, CloudWatch, AWS Config, GuardDuty
* Infrastructure protection
	- Services: VPC, Shield, WAF
* Data protection
	- Services: KMS, ELB (encryption), Macie (detect sensitive data)
* Incident response
	- Services: IAM, CloudFormation


## Best Practices

* Identity and access management
	- AWS Cognito - act as broker between login providers; securely access any AWS service from mobile device
* Data protection
	- Encrypt
		- Encryption at rest
		- Encryption in transit
		- Encrypted backups
	- Versioning
	- Storage resiliency
	- Detailed logging
* Incident response
	- Employ strategy of templated "clean rooms"
		- create new trusted environment to conduct investigation
		- use CloudFormation to easily create the "clean room" environment
