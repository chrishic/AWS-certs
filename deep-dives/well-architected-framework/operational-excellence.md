# Operational Excellence

* "Ability to run and monitor systems to deliver business value and to continuously improve supporting processes and procedures"
* Key service: CloudFormation


## Design Principles

* Perform operations as code
* Annotate documentation
* Make frequent, small, reversible changes
* Refine operations procedures frequently
* Anticipate failure
* Learn from all operational failures


## Focus Areas

* Prepare
	- Services: AWS Config, AWS Config Rules
* Operate
	- Services: CloudWatch, X-Ray, CloudTrail, VPC Flow Logs
* Evolve
	- Services: Elasticsearch (for searching log data to gain insights), CloudWatch Insights


## Best Practices

* Prepare
	- Implement telemetry for:
		- Application
		- Workload
		- User activity
		- Dependencies
	- Implement transaction traceability
	- Instrument at multiple levels
		- Record latency, error rates, and availability for all of the following:
			- API requests
			- dependencies
			- key operations
		- Look for outlying data points because this can be another indication of impending problems
			- Commonly known as percentile monitoring
	- Monitor all of your external (public) endpoints from remote locations
		- "User canary" apps: these execute some number of common tasks performed by consumers of the application
		- Helps to improve time to detection of problems
* Operate
	- Any event for which you raise an alert should have associated runbook
		- runbook defines triggers for escalations
	- Users should be notified when system is impacted
	- Communicate status through dashboards
		- provide dashboards to communicate the current operating status of the business and provide metrics of interest
		- AWS dashboards
			- [AWS Service Health Dashboard](https://status.aws.amazon.com/)
				- displays the general status of AWS services
			- [AWS Personal Health Dashboard](https://aws.amazon.com/premiumsupport/technology/personal-health-dashboard/)
				- gives a personalized view into the performance and availability of the AWS services underlying your AWS resources
* Evolve
	- Feedback loops
		- identify areas for improvement
		- gauge impact of changes to the system (i.e. did it make an improvement?)
		- perform operations metrics reviews
			- retrospective analysis of operations metrics
				- use these reviews to identify opportunities for improvement, potential courses of action, and share lessons learned


## Key Points

* [Runbooks](https://wa.aws.amazon.com/wat.concept.runbook.en.html)
	- Use runbooks to perform procedures
		- Runbooks are documented procedures to *perform specific actions* to achieve specific outcomes
		- Enable consistent and prompt responses to well-understood events by documenting procedures in runbooks
		- Implement runbooks as code and trigger the execution of runbooks in response to events where appropriate
		- Examples of runbook entries
			- send encrypted backup data to Amazon S3
			- restore encrypted backup to RDS
			- update DNS to route requests to static website during failures
			- deployment of infrastructure to new AZ (via CloudFormation)
			- perform software updates
			- perform deployment rollback
			- reporting requirements and performance tracking
			- handling region or availability zone failover
* [Playbooks](https://wa.aws.amazon.com/wat.concept.playbook.en.html)
	- Use playbooks to identify issues
		- Playbooks are documented processes to *investigate* issues
		- Enable consistent and prompt responses to failure scenarios by documenting investigation processes in playbooks
		- Implement playbooks as code and trigger playbook execution in response to events where appropriate
		- Examples of playbook entries
			- Dealing with common hardware failures, urgent software updates, and other disruptive changes
			- How to use logging to establish root cause
			- Dealing with security-related incidents
			- Failed deployments
			- Dealing with common database problems
			- How to troubleshoot telemetry to identify performance/availability issues
* Document environments
* Make small changes through automation
* Monitor workload with business metrics
* Exercise your response to failures
	- Game days
		- Practice failure recovery procedures using runbooks
* Have well-defined escalation management
