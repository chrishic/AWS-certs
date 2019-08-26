# Reliability

* "Ability to recover from failures, dynamically acquire resources to meet demand and mitigate disruptions such as network issues"
* Key service: CloudWatch


## Design Principles

* Test recovery procedures
* Auto recover from failures
* Scale horizontally to increase availability
* Stop guessing capacity
* Manage change with automation


## Focus Areas

* Foundations
	- Services: IAM, VPC, Trusted Advisor (visibility into service limits), Shield (protect from DDoS)
* Change management
	- Services: CloudTrail, AWS Config, CloudWatch, Auto Scaling
* Failure management
	- Services: CloudFormation, S3, Glacier, KMS


## Best Practices

* Foundations
	- Take into account physical and service limits
	- High availability
		- No single points of failure (SPOF)
		- Multi-AZ design
		- Load balancing
		- Auto scaling
		- Redundant connectivity
		- Software resilience
* Failure management
	- Backup and disaster recovery
		- RPO, RTO
	- Inject failures to test resiliency


## System Architecture and Design

* Architecture best-practice patterns
	- cell-based architecture
		- Watch: [AWS re:Invent 2018: How AWS Minimizes the Blast Radius of Failures - ARC338](https://www.youtube.com/watch?v=swQbA4zub20)
	- sharding
	- bulkheads
	- [Shuffle Sharding: Massive and Magical Fault Isolation](https://aws.amazon.com/blogs/architecture/shuffle-sharding-massive-and-magical-fault-isolation/)
	- fault isolation zones
* Distributed systems best-practice patterns
	- Implement loosely coupled dependencies
		- Loosely coupled dependencies include: Queuing systems, streaming systems, workflows, and load balancers
	- Throttling
		- defensive pattern to respond to an unexpected increase in demand
		- some requests will be honored, but rejected requests will return a message indicating they have been throttled
	- Retry with exponential fallback
		- invoking side of the throttling pattern
		- after getting throttled error, pause and then retry at later time
	- Fail fast
		- Simply return an error as soon as possible
		- Allows for releasing of resources and helps service recover
	- Use of idempotency tokens
		- Used to guarantee exactly-once execution
		- Caller issues API requests with an idempotency token; the same token is used whenever the request is repeated
		- When receiving request, service uses idempotency token to determine if work has already been completed
			- If so, return identical response as when the work was completed the first time
	- Circuit breaker
		- When remote system begins returning errors or exhibits high latency, enable circuit breaker
			- When enabled, dependency is ignored or results are replaced with locally-available data (such as from response cache)
		- Implement graceful degradation to transform applicable hard dependencies into soft dependencies
			- When a component's dependencies are unhealthy, the component itself does not report as unhealthy
				- it should continue to serve requests in a degraded manner
	- Bi-modal behavior and static stability
		- Bi-modal systems have different behavior under normal and failure modes
		- Instead, prefer systems that are statically stable and operate in only one mode


## Key Points

* Plan network topology
* Manage your AWS service and rate limits
* Monitor your system
* Automate responses to demand
* Backup
