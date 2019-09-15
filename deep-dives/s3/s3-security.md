# S3 Security

## Overview

* You can secure access to S3 objects via either:
	1. IAM policies
	2. Bucket policies
	3. Access Control Lists (ACLs)
* Policy Evaluation Logic
	- Decision starts at Deny
	- Evaluate all applicable policies/ACLs
		- Is there an explicit Deny?
			- YES: Final decision = Deny
		- Is there an Allow?
			- YES: Final decision = Allow
		- Final decision = Deny (default Deny)
* IAM Policies
	- Assign policies to users, roles, groups
	- Grants access permissions to AWS resources
	- With IAM policy, no principal is defined
		- You only need Actions, Effect, and Resource
* Bucket Policies
	- Attached to S3 buckets
	- Principal must be specified
	- Allows you to have user restrictions with using IAM


## Policy Language

* "Resource"
	- Specify ARN to identify resource
		- `arn:aws:s3:::myexamplebucket/*`
		- `arn:aws:s3:::myexamplebucket/images`
		- `arn:aws:s3:::myexamplebucket/images/image01.jpg`
* "Action"
	- For each resource, S3 allows a set of actions
		- `s3:GetObject`
		- `s3:ListBucket`
		- `s3:*`
* "Effect"
	- Allow or Deny
* "Principal"
	- Can specify user (IAM user, federated user, or assumed role user), AWS account, AWS service
* "Condition"
	- Used to build expressions with condition operators
	- Condition values can include:
		- date, time, IP address, ARN, username, userID, user agent
	- e.g.
```	
		"Condition": {
			"StringEquals": {
				"s3:RequestObjectTag/Project":"X" 
			}
		}
```		
* "NotPrincipal"
	- Specify an exception to list of principals
	- Do NOT use NotPrincipal with Effect Allow
		- Only use with Effect Deny
* "NotAction"
	- Matches everything except the list defined
	- Be careful using NotAction with Effect Allow
		- It could result in granting more permissions than expected
* "NotResource"
	- Explicitly matches everything except the specified list of resources
	- NotResource with Effect Allow
		- Provide access to all except listed
		- CAREFUL: could result in granting more permissions than intended
	- NotResource with Effect Deny
		- Deny access to all except listed


## Access Control Lists (ACLs)

* Sub-resource attached to buckets and objects
* Much more limited than IAM policies or bucket policies
* Predefined groups
	- "Authenticated Users" group
		- Allows user from ANY AWS account
	- "All Users" group
		- Requests can be authenticated or anonymous
	- "Log Delivery" group


## Avoid Accidental Deletes

* Use bucket policies to restrict deletes
* Enable MFA delete
	- Only on buckets with versioning enabled
	- You cannot enable MFA delete via the console
		- You must use CLI or SDK/API


## VPC Endpoints with S3

* Private connection between your VPC and S3
* You can assign VPC endpoint policies to control access


## Links

* [Identity and Access Management in Amazon S3](https://docs.aws.amazon.com/AmazonS3/latest/dev/s3-access-control.html)
* [Example IAM Identity-Based Policies](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_examples.html)
* [S3 Access Policy Language Overview](https://docs.aws.amazon.com/AmazonS3/latest/dev/access-policy-language-overview.html)
* [S3 Bucket Policy Examples](https://docs.aws.amazon.com/AmazonS3/latest/dev/example-bucket-policies.html)
* [Using S3 Block Public Access](https://docs.aws.amazon.com/AmazonS3/latest/dev/access-control-block-public-access.html)
