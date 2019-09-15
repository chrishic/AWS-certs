# Managing S3 Storage

## Toolbox

* Lifecycle policies
* Object tags
* S3 inventory
* Cross-region replication
* CloudWatch request metrics
* Event notifications
* Storage class analysis
* CloudTrail data events


## Bucket Options

* Versioning
	- Enabled at bucket level
	- Protects from unintended deletes or app logic failures
	- No performance penalty
	- New version with every upload
	- Three states:
		- Unversioned
		- Versioning-enabled
		- Versioning-suspended
* Server access logging
	- Access logs contain:
		- requestor, bucket name, request time, action, response status, error code
	- To enable:
		- turn on log delivery for source bucket
		- grant S3 Log Delivery group permission to the logs destination bucket
* Object-level logging
	- CloudTrail logs capture bucket level and object level requests
* Static website hosting
* Default encryption
* Object tags
	- Can have up to 10 tags per object
	- Can use tags in conjunction with:
		- access control
		- lifecycle policies
		- analysis
		- CloudWatch metrics, display by tags
* Transfer acceleration
	- When you enable transfer acceleration, you will get a special endpoint to the bucket that leverages the acceleration
		- the standard endpoint for the bucket will use normal network
* Event Notifications
	- Notify when objects are created via PUT, POST, COPY, DELETE or multipart upload
	- Filter on prefixes and suffixes
	- Trigger workflow with SNS, SQS, Lambda
* Requester Pays


## S3 Inventory

* Async alternative to S3 list objects
	- Costs less than half of what it costs when using S3 list API
* S3 Inventory report is generated each time a daily or weekly bucket inventory is run
	- Report can be configured to include all of the objects in a bucket, or to focus on a prefix-delimited subset
	- Report can be encrypted
* Generates report with selected fields
* You can use S3 Event Notifications to get notified when reports are ready
	- e.g. Trigger on `manifest.json` file being written


## Cross-Region Replication

* Bucket-level feature
* Automated, fast, reliable async replication of data across AWS Regions
* Not replicated:
	- Delete markers
	- Lifecycle actions
	- Objects in source bucket before replication enabled
	- Objects with SSE-C
	- Objects in source bucket that already have been replicated
- Requirements:
	- Versioning must be enabled on buckets
	- Source and destination buckets must be in different regions
	- IAM role that has proper permissions
- Use cases
	- Compliance
	- Minimize latency
	- Operational 
	- Data protection
- Ownership overwrite
	- Changes object owner to owner of destination bucket


## Links

* [Hosting a Static Website on Amazon S3](https://docs.aws.amazon.com/AmazonS3/latest/dev/WebsiteHosting.html)
