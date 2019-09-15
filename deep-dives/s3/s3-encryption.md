# S3 Encryption

## Types of Encryption

* Two types of encryption:
	1. In transit (SSL/TLS)
	2. At rest
		- Encryption can be done either client-side or server-side


## Server-side Encryption

* S3 encrypts object before saving it to disk and decrypts when retrieving object
* Three choices:
	- SSE-S3
		- Keys managed by S3
		- Each object encrypted with unique key
		- Master key encrypts each unique key
			- Master key is rotated regularly
	- SSE-KMS
		- Keys managed by KMS
		- Per object data key encrypted with CMK
			- "envelope encryption"
		- S3 will create CMK for you if you don't specify one
		- Add layer of security in that anyone needing to decrypt must have access to CMK
			- Provides audit trail
		- TIP: Be aware of KMS API throttling when exceeding request/sec 
	- SSE-C
		- Keys managed by you
		- You must provide encryption key on each PUT and GET operation
		- S3 does *not* store the key
			- Instead it stores salted HMAC value of key to validate future requests


## Client-side Encryption

* Two choices:
	- KMS with S3 encryption client
	- Perform your own encryption/decryption with your own client-side master key


## Enforcing Encryption

* Enforce encryption by default for S3 bucket
	- Previously you had to have bucket policies that denied writes to unencrypted requests
	- Now, you can use the "Default encryption" option
		- All new object writes will be automatically encrypted
    - Works with S3-managed (SSE-S3) or KMS master keys (SSE-KMS)
    - Ensures encryption if the client calling S3 PUT command forgets to set encryption as header parameter
        - if your PUT request headers don't include encryption information, Amazon S3 uses the bucket’s default encryption settings to encrypt the objects

	
## Links

* [S3: Protecting Data Using Server-Side Encryption](https://docs.aws.amazon.com/AmazonS3/latest/dev/UsingServerSideEncryption.html)
* [Amazon S3 Default Encryption for S3 Buckets](https://docs.aws.amazon.com/AmazonS3/latest/dev/bucket-encryption.html)
* [Protecting Data Using Server-Side Encryption with Amazon S3-Managed Encryption Keys](https://docs.aws.amazon.com/AmazonS3/latest/dev/UsingServerSideEncryption.html)
