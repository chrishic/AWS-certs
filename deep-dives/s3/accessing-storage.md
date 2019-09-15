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
			- e.g. http://s3-eu-west-1.amazonaws.com/mybucket/image01.jpg
		- EXCEPTION: if your bucket is in US-EAST (N. Virginia), then use:
			- e.g. http://s3.amazonaws.com/mybucket/image01.jpg
	- Virtual-hosted-style URL
		- e.g. http://bucketname.s3.amazonaws.com/object_key


## Operations on Objects

* PUT
	- used to write an object to a bucket
	- Single operation
		- upload or copy objects up to 5GB in single PUT
	- Multiple operations (multipart uploads)
		- for objects up to 5TB, you must use multipart upload API
		- tip: you should consider using multipart upload for objects larger than 100MB
		- all parts kept on server until complete or aborted
			- so make sure you complete or abort each upload otherwise you will be charged
			- you can use lifecycle rules to clean up multipart uploads
				- enable "Clean up incomplete multipart uploads" in lifecycle settings
* COPY
	- create copies of object
	- rename objects by creating copy and deleting original
	- move objects to different location
	- update object metadata
* GET
	- you retrieve whole object at once or in parts
	- use "Range" HTTP header to specify the range of bytes needed
* DELETE
	- you can delete single or multiple objects in a single DELETE
	- behavior depends upon whether versioning is enabled
	- bucket does not have versioning enabled
		- DELETE permanently removes the object, making it unrecoverable
	- bucket has versioning enabled
		- you can either permanently delete object or delete marker
		- if DELETE specifies key name only:
			- S3 inserts delete marker
			- you can recover object by removing delete marker
		- to permanently delete individual versions, invoke DELETE with key name and version ID
		- to completely remove object from bucket, ALL individual versions must be deleted


## Listing Object Keys

* You can enumerate keys contained in a bucket
* Keys are selected for listing by bucket and prefix
	- e.g. 
```	
		$ aws s3api list-objects-v2 --bucket scores --prefix 2017/ --query "Contents[].{Key:Key}"
		$ aws s3api list-objects-v2 --bucket scores --prefix 2017/scores/ --delimiter / --query "Contents[].{Key:Key}"
```
