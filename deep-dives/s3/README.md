# Amazon Simple Storage Service (S3) Deep Dive

## Table of Contents

<!-- MarkdownTOC depth=4 -->

- [Security and Encryption](#security-and-encryption)
- [Presigned URLs](#presigned-urls)
- [CloudFront](#cloudfront)
- [Snowball](#snowball)
- [Links](#links)

<!-- /MarkdownTOC -->

---


## Security and Encryption

* You can secure buckets via either:
	1. Bucket policies
	2. Access Control Lists (ACLs)
* Two types of encryption:
	1. In transit (SSL/TLS)
	2. At rest
		- server side encryption
	  		- S3 managed keys - SSE-S3
	  		- KMS encrypted - SSE-KMS (provides audit trail)
	  		- customer provided keys - SSE-C
	 	- client side encryption
* Enforce encryption by default for S3 bucket:
    - any object placed in this bucket must be encrypted with a specific key
    - works with S3-managed (SSE-S3) or KMS master keys (SSE-KMS)
    - ensures encryption if the client calling S3 PUT command forgets to set encryption as header parameter
        - if your PUT request headers don't include encryption information, Amazon S3 uses the bucket’s default encryption settings to encrypt the objects
    - See: [Amazon S3 Default Encryption for S3 Buckets](https://docs.aws.amazon.com/AmazonS3/latest/dev/bucket-encryption.html)


## Presigned URLs
TBD.


## CloudFront

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


## Snowball
* Used to be known as Import/Export
* Previously customers could send in their own storage devices
	- Now AWS provides the storage appliance themselves
* Snowball can:
	- import to S3
	- export from S3
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


## Links
* [S3: Protecting Data Using Server-Side Encryption](https://docs.aws.amazon.com/AmazonS3/latest/dev/UsingServerSideEncryption.html)
* [Amazon S3 Default Encryption for S3 Buckets](https://docs.aws.amazon.com/AmazonS3/latest/dev/bucket-encryption.html)
