# S3 Lifecycle Policies

## Storage Class Hierarchy

* Standard / Reduced Redundancy
* Intelligent Tiering
	- When you don't know the access patterns of your objects, let INTELLIGENT_TIERING automatically manage for you
* Standard IA
* One Zone IA
* Glacier
* Deep archive


## Lifecycle configuration

* A set of one or more rules (up to 1000)
* You can set lifecycle rules based on the bucket, object prefix or object tags
* Each rule defines an action, either:
	- Transition
		- move to STANDARD_IA, ONEZONE_IA or GLACIER based on object age
			- e.g. transition to STANDARD_IA storage class 30 days after you created them or archive to GLACIER one year after creation
	- Expiration
		- delete objects after the time you specify
	- For versioning-enabled buckets, additional actions:
		- NoncurrentVersionTransition
		- NoncurrentVersionExpiration			
* Automate transitions
	- Automate the tiering process from one storage class to another
	- Considerations:
		- no auto transition of objects less than 128KB to standard IA or One Zone IA
		- data must remain in current storage class for at least 30 days before it can be automatically moved to standard IA or One Zone IA
	- Data can be moved from any storage class directly to Glacier
* Transitions to INTELLIGENT_TIERING
	- You can transition from STANDARD->INTELLIGENT_TIERING after 1 day
* Transitions to STANDARD_IA/ONEZONE_IA
	- When you have objects less than 128KB in IA storage, get charged for full 128KB of storage
		- Difference between two is reported in CloudWatch metric reports at `OneZoneIASizeOverhead`
* Transitions to Glacier
	- One-way transition (cannot transition out of Glacier with lifecycle rules)
	- You can transition from any storage class to the GLACIER or DEEP_ARCHIVE storage classes
	- Objects transitioned to Glacier can only be viewed from S3, *not* Glacier directly
	- You can transition from STANDARD->GLACIER after 1 day
	- When you transition S3 objects to Glacier, each object uses 8KB of STANDARD storage for metadata about the object
		- Reported in CloudWatch metric reports as `GlacierS3ObjectOverhead`
	- For each archived object, there is 32KB of extra GLACIER storage for index and related metadata
	- Minimal storage duration periods
		- GLACIER: 90 days
		- DEEP_ARCHIVE: 180 days


## Transition Constraints

* There is a minimum 30-day storage charge associated with the INTELLIGENT_TIERING, STANDARD_IA, and ONEZONE_IA storage classes
* STANDARD/STANDARD_IA -> INTELLIGENT_TIERING
	- Only applies to objects > 128KB
* STANDARD -> STANDARD_IA/ONEZONE_IA
	- Only applies to objects > 128KB
	- Objects must be stored at least 30 days in the current storage class before you can transition them to STANDARD_IA or ONEZONE_IA
		- i.e. You *cannot* transition from S3 standard to S3 IA after one day; there is a minimum 30 day period before transition can happen
* STANDARD_IA -> ONEZONE_IA
	- Objects must be stored at least 30 days in the STANDARD_IA storage class before you can transition them to the ONEZONE_IA class


## Links
* [S3 Object Lifecycle Management](https://docs.aws.amazon.com/AmazonS3/latest/dev/object-lifecycle-mgmt.html)
* [Transitioning Objects Using Amazon S3 Lifecycle](https://docs.aws.amazon.com/AmazonS3/latest/dev/lifecycle-transition-general-considerations.html)
