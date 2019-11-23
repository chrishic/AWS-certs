# Designing Scalable Solutions

## Auto Scaling

### Types of Auto Scaling

* EC2 Auto Scaling
	- automatically provides horizontal scaling
	- triggered by an event or scaling action to either add/delete instances
	- availability, cost and system metrics can all factor into scaling
	- Four scaling options
		- Maintain
			- Keep specific number of instances running
		- Manual
			- Use maximum, minimum or specific number of instances
			- Manually change desired capacity via console or CLI
		- Schedule
			- Add/delete based on schedule
		- Dynamic
			- Scale based on real-time metrics of systems
			- To use, specify an Auto Scaling Policy
				- Target Tracking Policy
					- scale based on metric in relation to target value
				- Simple Scaling Policy
					- waits until health check and cool down period before evaluating new need
				- Step Scaling Policy
					- responds to scaling needs with more sophistication and logic
* Application Auto Scaling
	- API used to control scaling for non-EC2 resources
		- e.g. DynamoDB, ECS, EMR
	- Provides common way to interact with scalability of other services
	- Application Auto Scaling policies:
		- Target Tracking Policy
			- scale based on metric in relation to target value
		- Step Scaling Policy
			- based on a metric, adjusts capacity given certain defined thresholds
				- e.g. "I want to increase my Spot Fleet by 20% every time I add another 10,000 connections on my ELB"
		- Scheduled Scaling Policy
			- initiates scaling events based on predefined time, day or date
* AWS Auto Scaling
	- Provides centralized way to manage scalability for whole stacks
	- [Predictive Scaling for EC2 feature](https://aws.amazon.com/blogs/aws/new-predictive-scaling-for-ec2-powered-by-machine-learning/)
	- Console that can manage both EC2 and Application Auto Scaling


### Scaling Cooldown

* Configurable duration that gives scaling a chance to "come up to speed" and absorb load
* Default cooldown is 300 seconds
* Automatically applies to Dynamic Scaling and optionally to Manual Scaling
	- NOT supported for Scheduled Scaling
* You can override default cooldown via scaling-specific cooldown


## Kinesis

* For processing streams of various data
* Data in processed in "shards"
	- Each shard able to ingest at 1000 records/second
	- Default limit of 50 shards
		- But you can request increase up to unlimited number
* Record consists of:
	- Partition key
	- Sequence number
	- Data BLOB (up to 1MB)
* Kinesis is a transient data store
	- Default retention is 24 hours
		- Configurable up to 7 days
* Kinesis services
	- Kinesis Video Streams
		- ingests, durably stores, encrypts and indexes video streams for real-time and batch Analytics
	- Kinesis Data Streams
		- ingests and stores data streams for processing	
	- Kinesis Data Firehose
		- prepares and loads data continuously to the destinations you choose
	- Kinesis Data Analytics
		- run standard SQL queries against data streams


## CloudFront

* Can cache both static and dynamic content at edge locations
	- Dynamic content delivery is achieved using HTTP cookies forwarded from your origin
* Supports Adobe Flash Media Server's RTMP protocol but you have to choose RTMP delivery method
* Web distributions also support media streaming and live streaming
	- But they use HTTP or HTTPS
* Origins can be S3, EC2, ELB or another web server
	- multiple origins can be configured
	- use Behaviors to configure serving origin content based on URL paths
		- makes it easy to have single CloudFront distribution that gets static content from S3 but dynamic content from ELB, based upon path
* Cache Invalidation
	- Delete the file from origin and wait for TTL to expire
	- Use AWS Console to request invalidation for all content or a specific path
	- Use CloudFront API to submit invalidation request
	- Use 3rd party tools (CloudBerry, Ylastic, etc)
* CloudFront also supports zone apex DNS entries
* CloudFront supports geo-restrictions
	- you can whitelist or blacklist based upon geographic origin (by country)


## SNS

* SNS endpoint protocols include:
	- HTTP(S)
	- email
	- SMS
	- SQS
	- Lambda
	- Amazon Device Messaging (push notifications)


## SQS

* Integrates with KMS for encryption of messages
* Transient storage
	- default is 4 days, with max of 14 days
* Optionally supports FIFO queues
* Max message size is 256KB
	- Use Java SQS SDK to have messages as large as 2GB
		- Basically data is stored in S3, and pointer to S3 location is included in actual message

