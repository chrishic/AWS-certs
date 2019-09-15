# Optimizing Performance in S3


## Best Practices - Performance

* For faster uploads over long distances, use Transfer Acceleration
* For faster uploads of large objects, use Multipart Upload API
* Optimize GET performance with Range GETs and integrate with CloudFront
* Use S3 inventory to optimize listing objects
	- Report lists objects and their corresponding metadata
	- Scheduled alternative to sync LIST API


## Request Rate Performance

* S3 single partition throughput rates
	- GET: 5500 req/sec
	- PUT/POST/DELETE: 3500 req/sec
* You can increase performance by using multiple prefixes
	- S3 automatically creates additional partitions to scale if needed
		- New partition creation triggered by exceeding request rates (and getting 5xx errors)
* If you know that you will exceed the request rates, you can contact AWS beforehand to pre-create the partitions


## Multipart Uploads

* Single object uploaded in separate parts in parallel
* Benefits:
	- Improved throughput, faster uploads
	- Retransmit only failed parts
	- Upload parts independently and in any order
	- Pause and resume for uploads
	- Begin upload before you know final size
* Three step process
	- 1. Initiate upload and upload the object parts
	- 2. When done uploading, send a "complete multipart upload" request
	- 3. S3 then constructs the object from the uploaded parts
* When to use:
	- Object is over 100MB
	- Using spotty network
* Remember to cleanup:
	- Multipart uploads must be COMPLETED or ABORTED
		- they don't expire, and incomplete uploads will consume storage space
	- Use "AbortIncompleteMultipartUpload" API call
	- Lifecycle configuration for multipart cleanup
* Tip:  For Java environments, use "TransferManager" in the AWS SDK for Java
	- Makes extensive use of multipart uploads
	- Uses multiple threads
		- Default pool size is 10 threads
	- Can also do multipart downloads
		- Uses GET Range requests to download parts of object


## S3 Select

* Pull out only the data you need from S3 with SQL expression
* Works like GET request
	- it is an API call
* Works on objects stored in:
	- CSV, JSON, Apache Parquet
	- Also can work with GZIP and BZIP2 compressed JSON/CSV files
* Results can be in either CSV or JSON format


## CloudFront

* User is routed to edge location with least latency
* Can help reduce costs when workload is primarily GETs
	- fewer S3 GETs (replaced by CloudFront cache hits)
* Creating a CDN
	1. Create a S3 bucket to host your website content - make sure to make it public
	2. Go to Cloudfront - click "Create Distribution" - Select "Web Distribution"
		- specify S3 bucket as origin
		- restrict bucket access
		- Origin Access Identity
		- Restrict viewer access via signed URLs or signed cookies
		- Use pre-signed URLs to lock down CloudFront or S3 content
* CloudFront allows for "geo-restrictions" 
	- you can whitelist and blacklist
* You can invalidate objects at edge locations manually, but you are charged for it
* To delete a distribution:
	1. First disable it
	2. Then delete it


## Transfer Acceleration

* Change your endpoint, not your code
* Only used if AWS determines it is likely to be faster than standard
	- i.e. it is either faster or no charge incurred


## Links

* [S3 SELECT Object Content](https://docs.aws.amazon.com/AmazonS3/latest/API/RESTObjectSELECTContent.html)
* [Amazon S3 Transfer Acceleration](https://s3-accelerate-speedtest.s3-accelerate.amazonaws.com/en/accelerate-speed-comparsion.html)
