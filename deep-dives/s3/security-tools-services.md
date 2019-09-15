# AWS Security Tools and Services with S3

## AWS Config

* Monitor S3 for configuration changes
* Can send notifications when configuration is not compliant
* To setup:
	- Specify resource types that you want Config to record
	- Set up S3 bucket to receive configuration snapshot
	- Set up SNS topic to send configuration notifications
	- Grant AWS Config permissions to access bucket and SNS topic
	- Specify rules you want Config to use to evaluate compliance
* Built-in rules:
	- `s3-bucket-logging-enabled`
	- `s3-bucket-public-read-prohibited`
	- `s3-bucket-public-write-prohibited`
	- `s3-bucket-ssl-requests-only`
	- `s3-bucket-versioning-enabled`


## CloudTrail

* For each request, CloudTrail logs:
	- who made API call, timestamp, resources acted upon
* Can enable at bucket or prefix level, and filter on reads, writes or both
* By default, CloudTrail logs for bucket-level calls
* You can capture object-level calls by enabling S3 Data Events in CloudTrail configuration
* Integrates with CloudWatch
	- S3 Data Events are delivered to CloudWatch Events within seconds
* Make sure destination S3 bucket has permissions to allow CloudTrail to write log writes
	- This happens automatically if you create destination bucket when creating/updating trail in the console
	- But if you specify existing S3 bucket, you must attach policy to bucket to allow CloudTrail access
* Best practice: 
	- Use dedicated bucket for CloudTrail logs


## Security inspection

* Use S3 Inventory to view encryption status of your objects


## Trusted Advisor

* Checks S3 buckets and shows which have public access
* Can check buckets for versioning
* Can check for bucket logging


## Macie

* Searches bucket for sensitive events
	- e.g. PII, PHI, access keys, credit card information
* Uses CloudTrail events to evaluate all requests sent to bucket


## Links

* [Logging Amazon S3 API Calls by Using AWS CloudTrail](https://docs.aws.amazon.com/AmazonS3/latest/dev/cloudtrail-logging.html)
* [Amazon S3 Bucket Policy for CloudTrail](https://docs.aws.amazon.com/awscloudtrail/latest/userguide/create-s3-bucket-policy-for-cloudtrail.html)
