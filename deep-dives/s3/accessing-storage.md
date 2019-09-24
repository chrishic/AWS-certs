# Accessing S3 Storage

## Data consistency model

* Read-after-write consistency for PUT requests of new objects
	- When new object is written to bucket, object is immediately visible to all clients and applications
* Eventually consistent data model for read-after-write if HEAD or GET request is made before object exists
* Eventual consistency for overwrite PUT and DELETE requests
	- i.e. if you overwrite or delete an existing object, some clients may see old data until all replication is complete


## Getting Data into S3

* Direct Connect
* Storage Gateway (in File Gateway mode)
	- NFS/SMB access to S3 bucket
* Kinesis
	- Kinesis Data Firehose
		- captures and automatically loads streaming data into S3 and Redshift
	- Kinesis streams
		- Kinesis Video Streams
			- auto scales to ingest streaming video data from millions of devices
			- uses S3 as underlying data store
		- Kinesis Data Streams
			- capture and store TB of data per hour from website clickstreams, social media feeds, IT logs, etc...
* S3 Transfer Acceleration
	- Leverages CloudFront's edge locations
* Snowball and Snowmobile


## Accessing Buckets and Objects

* Buckets and objects have two types of URLs
	- Path-style URL
		- specify region specific endpoint, bucket name and object key name
			- e.g. `http://s3-eu-west-1.amazonaws.com/mybucket/image01.jpg`
		- EXCEPTION: if your bucket is in US-EAST (N. Virginia), then use:
			- e.g. `http://s3.amazonaws.com/mybucket/image01.jpg`
		- NOTE:  Path-style URLs are being deprecated starting September 2020
			- See: [Amazon S3 Path Deprecation Plan – The Rest of the Story](https://aws.amazon.com/blogs/aws/amazon-s3-path-deprecation-plan-the-rest-of-the-story/)
	- Virtual-hosted-style URL
		- e.g. `http://bucketname.s3.amazonaws.com/object_key`
* Tip: to explore a public S3 bucket via browser use the URL:
	- https://<bucket-name>.s3.amazonaws.com/index.html
		- e.g. https://openaq-fetches.s3.amazonaws.com/index.html


## Operations on Objects

* PUT
	- Used to write an object to a bucket
	- Single operation
		- Upload or copy objects up to 5GB in single PUT
	- Multiple operations (multipart uploads)
		- For objects up to 5TB, you must use multipart upload API
		- Tip: you should consider using multipart upload for objects larger than 100MB
		- All parts kept on server until complete or aborted
			- So make sure you complete or abort each upload otherwise you will be charged
			- You can use lifecycle rules to clean up multipart uploads
				- Enable "Clean up incomplete multipart uploads" in lifecycle settings
* COPY
	- Create copies of object
	- Rename objects by creating copy and deleting original
	- Move objects to different location
	- Update object metadata
* GET
	- You retrieve whole object at once or in parts
	- Use "Range" HTTP header to specify the range of bytes needed
* DELETE
	- You can delete single or multiple objects in a single DELETE
	- Behavior depends upon whether versioning is enabled
	- If bucket does not have versioning enabled:
		- DELETE permanently removes the object, making it unrecoverable
	- If bucket has versioning enabled:
		- You can either permanently delete object or delete marker
		- If DELETE specifies key name only:
			- S3 inserts delete marker
			- you can recover object by removing delete marker
		- To permanently delete individual versions, invoke DELETE with key name and version ID
		- To completely remove object from bucket, ALL individual versions must be deleted


## Listing Object Keys

* You can enumerate keys contained in a bucket
* Keys are selected for listing by bucket and prefix
	- e.g. 
```	
		$ aws s3api list-objects-v2 --bucket scores --prefix 2017/ --query "Contents[].{Key:Key}"
		$ aws s3api list-objects-v2 --bucket scores --prefix 2017/scores/ --delimiter / --query "Contents[].{Key:Key}"
```
