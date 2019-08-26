# Performance Efficiency

* "Ability to use resources efficiently to meet system requirements and maintain that efficiency as demand changes and technology evolves"
* Key service: CloudWatch


## Design Principles

* Easy to try new advanced technologies (by letting AWS manage them, instead of standing them up yourself)
* Go global in minutes
* Use serverless architectures
* Experiment more often
* Mechanical sympathy (use the technology approach that aligns best to what you are trying to achieve)


## Focus Areas

* Selection
	- Services: EC2, EBS, RDS, DynamoDB, Auto Scaling, S3, VPC, Route53, DirectConnect
* Review
	- Services: AWS Blog, AWS What's New
* Monitoring
	- Services: CloudWatch, Lambda, Kinesis, SQS
* Tradeoffs
	- Services: CloudFront, ElastiCache, Snowball, RDS (read replicas)


## Best Practices

* Selection
	- Choose appropriate resource types
		- Compute, storage, database, networking
	- Take advantage of specific AWS services and features that increase performance efficiency such as:
		- [Enhanced Networking](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/enhanced-networking.html)
			- uses single root I/O virtualization (SR-IOV) to provide high-performance networking capabilities on supported EC2 instance types
		- [Amazon EBS-optimized instance](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/EBSOptimized.html)
			- uses an optimized configuration stack and provides additional, dedicated capacity for Amazon EBS I/O
		- [Amazon S3 Transfer Acceleration](https://docs.aws.amazon.com/AmazonS3/latest/dev/transfer-acceleration.html)
			- takes advantage of edge locations (CloudFront) to provide better proximity to end users
		- [VPC Endpoint](https://docs.aws.amazon.com/vpc/latest/userguide/vpc-endpoints.html)
			- enables you to privately connect your VPC to supported AWS services and VPC endpoint services powered by PrivateLink
			- traffic between your VPC and other service does not leave the Amazon network
* Trade Offs
	- Proximity and caching
