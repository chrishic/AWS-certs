# Amazon Simple Storage Service (S3) Deep Dive

## Table of Contents

<!-- MarkdownTOC depth=4 -->

- [Accessing Storage](./accessing-storage.md)
- [Managing Storage](./managing-storage.md)
- [Lifecycle Policies](./lifecycle-policies.md)
- [Security](./s3-security.md)
- [Encryption](./s3-encryption.md)
- [Optimization](./s3-optimization.md)
- [Security Tools and Services](./security-tools-services.md)
- [Monitoring and Analyzing S3 Data](#monitoring-and-analyzing-s3-data)
- [Presigned URLs](#presigned-urls)
- [CORS](#cors)
- [Snowball](#snowball)
- [S3 Batch Operations](#s3-batch-operations)

<!-- /MarkdownTOC -->

---

## Monitoring and Analyzing S3 Data

* Storage Class Analysis
	- Identify hot data (frequently-accessed) vs warm/cold data (infrequently-accessed)
	- Delivers daily report of S3 data access patterns to help visualize hot, warm and cold data
	- Offers guidance on how to tune lifecycle policies to save $$$
		- Recommendations of when to transition data occurs 30 days after Storage Class analysis has been enabled
	- Can drill deep by bucket, prefix or object tag
	- Can export data to S3 bucket for analysis with other BI tools
* QuickSight
	- Allows you to easily analyze and visualize S3 Storage Class analytics data
		- Gain insights into usage and growth patterns
	- Integrated with S3
		- No need for manual exports or data preparation
		- Can setup a daily refresh of storage class analytic data
	- You can publish/share visualizations as a dashboard
* CloudWatch Metrics
	- Two types of metrics
		- Storage metrics
			- Reported once per day at no additional cost
				- `BucketSizeBytes`
					- You can see amount of data storage per each storage class, along with per-class overhead metrics
				- `NumberOfObjects`
			- When you transition S3 objects to Glacier, each object uses 8KB of STANDARD storage for metadata about the object
				- Reported in CloudWatch metric reports as `GlacierS3ObjectOverhead`
			- When you have objects less than 128KB in IA storage, get charged for full 128KB of storage
				- Difference between two is reported in CloudWatch metric reports at `OneZoneIASizeOverhead`
		- Request metrics
			- Must be enabled
			- Available in 1 minute intervals and incur additional cost
				- Examples:
					- `AllRequests`, `PutRequests`, `GetRequests`, `BytesDownloaded`, etc.


## Presigned URLs

* Presigned URLs: PUT
	- Allow for restricted access, but not require users to have AWS credentials
	- When you create the presigned URL, you provide:
		- Your security credentials
			- the creator of the presigned URL must have permissions to access the object
		- Bucket name
		- Object key
		- HTTP method (PUT, GET)
		- Expiration date and time
* Presigned URLs: GET
	- Allow anyone with URL to retrieve objects from bucket


## CORS

* Cross-Origin Resource Sharing (CORS)
	- You can selectively allow cross-origin access to your S3 resources
	- To enable CORS:
		- Create a CORS configuration XML file with rules to identify the origins that are allowed to access your bucket


## Snowball

* Used to be known as Import/Export
* Previously customers could send in their own storage devices
	- Now AWS provides the storage appliance themselves
* Snowball can:
	- Import to S3
	- Export from S3
* Three flavors:
	1. Snowball 
		- storage only 
		- available in all regions 
		- 80TB
	2. Snowball Edge 
		- storage + compute
		- 100TB
	3. Snowmobile 
		- semi-truck of storage capacity


## S3 Batch Operations

* Requires a list of S3 objects to be processed in the batch job
	- List is either Inventory Report or CSV file
* Actions for batch operations:
	- PUT copy
	- Invoke Lambda
	- Replace all tags
	- Replace ACL
	- Restore (from Glacier)
* Requires IAM role that gives S3 permission to:
	- read the objects in the manifest
	- perform desired actions
	- write optional completion report
	- ALSO: if using Lambda, Execution Role for the function must trust S3 Batch Operations
* See: [New – Amazon S3 Batch Operations](https://aws.amazon.com/blogs/aws/new-amazon-s3-batch-operations/)
