# AWS Certified Solutions Architect - Associate
Study guide for [AWS Certified Solutions Architect - Associate](https://aws.amazon.com/certification/certified-solutions-architect-associate/).

## Table of Contents

<!-- MarkdownTOC depth=4 -->

- [Preparation Steps](#preparation-steps)
- [Areas to Focus On](#areas-to-focus-on)
- [Tricky Areas from Actual Test](#tricky-areas-from-actual-test)
- [Sample Questions](#sample-questions)
- [Other Notes](#other-notes)
- [Whitepapers](#whitepapers)
- [FAQs](#faqs)
- [Other Resources](#other-resources)

<!-- /MarkdownTOC -->

---

## Preparation Steps
  1. Review the [Exam Guide](https://d1.awsstatic.com/training-and-certification/docs-sa-assoc/AWS_Certified_Solutions_Architect_Associate-Exam_Guide_EN_1.8.pdf) and [Sample Questions](https://d1.awsstatic.com/training-and-certification/docs/AWS_Certified_Solutions_Architect_Associate_Sample_Questions.pdf)
  2. [Cloud Guru preparation course](https://acloud.guru/learn/aws-certified-solutions-architect-associate)
  3. [Linux Academy preparation course](https://linuxacademy.com/course/aws-certified-solutions-architect-2019-associate-level/)
  4. Review the [Well Architected Framework deep dive](../../deep-dives/well-architected-framework/)
  5. Review [whitepapers](#whitepapers)
  6. Review [FAQs](#faqs)


## Areas to Focus On
  1. VPC - including ENI, EIP, route tables, NAT
  2. Content Delivery Networks (CDN) - CloudFront
  3. Networking - route tables, firewalls, NAT
  4. Pricing/cost (e.g., on Demand vs. Reserved vs Spot; RTO and RPO disaster recovery design)
  5. Hybrid IT architectures (e.g. Direct Connect, Storage Gateway, VPC, Directory Services)
  6. [AWS shared responsibility model](https://aws.amazon.com/compliance/shared-responsibility-model/)
  7. AWS CloudTrail
  8. [Service limits](http://docs.aws.amazon.com/general/latest/gr/aws_service_limits.html)
  9. [EC2 Instance Metadata and User Data](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-instance-metadata.html)


## Tricky Areas from Actual Test
  1. Lambda use cases ->  DynamoDB, SNS  (Route53, Redshift were incorrect answers)
  2. Kinesis Firehose vs Kinesis Streams
  3. SSO / SAML / federated identity
  4. Other IAM-related questions, especially with regard to multi-region environments
  5. CloudTrail
  6. Stateless (NACLs) vs stateful (security groups) access control
  7. NAT instances vs NAT gateways
  8. Bastion hosts
  9. Question on whether you can change CIDR mask for existing subnet (to decrease address space)
  10. Setup VPN between on-premises machines and AWS VPC


## Sample Questions

Q. You are building a system to distribute confidential training videos to employees. Using CloudFront, what method could be used to serve content that is stored in S3, but not publically accessible from S3 directly?

A. Create an origin access identity (OAI), which is a special CloudFront user, and associate the origin access identity with your distribution. Then change the S3 bucket policy so only the OAI has read permission.

Q. How to protect S3 data from both accidental deletion and accidental overwriting?

A. Enable Versioning for a bucket - S3 preserves existing objects anytime you perform a PUT, POST, COPY, or DELETE operation on them.

Q. What does Amazon CloudWatch include in the basic monitoring package for EC2?

A. CloudWatch provides hypervisor visible metrics such as CPU utilization. A responsibility boundary exists between the hypervisor and guest operating system. AWS does not have access to the guest operating system and therefore cannot see anything that is not visible to the hypervisor. Such information would be resource demands of the guest operating system that the hypervisor must service like, CPU usage.


## Other Notes

[S3 Request Rate and Performance Considerations](http://docs.aws.amazon.com/AmazonS3/latest/dev/request-rate-perf-considerations.html)

Amazon S3 automatically scales to high request rates. For example, your application can achieve at least 3,500 PUT/POST/DELETE and 5,500 GET requests per second per prefix in a bucket. There are no limits to the number of prefixes in a bucket. It is simple to increase your read or write performance exponentially. For example, if you create 10 prefixes in an Amazon S3 bucket to parallelize reads, you could scale your read performance to 55,000 read requests per second.

[EC2 Spot Instances](https://aws.amazon.com/ec2/spot/)

With Spot instances, you will never be charged more than the maximum price you specified.  While your instance runs, you are charged the Spot price that is in effect for that period.  If the Spot price exceeds your specified price, your instance will receive a two-minute notification before it is terminated, and you will not be charged for the partial hour that your instance has run.


## Whitepapers
  1. "Architecting for the Cloud: AWS Best Practices"
  2. "AWS Security Best Practices"
  3. "Amazon Web Services: Overview of Security Processes"
  5. "Development and Test on AWS"
  6. "Backup and Recovery Approaches Using AWS"
  7. "Amazon Virtual Private Cloud Connectivity Options"
  8. "How AWS Pricing Works"


## FAQs
  1. [EC2 FAQ](https://aws.amazon.com/ec2/faqs/)
  2. [S3 FAQ](https://aws.amazon.com/s3/faqs/)
  3. [VPC FAQ](https://aws.amazon.com/vpc/faqs/)
  4. [Route53 FAQ](https://aws.amazon.com/route53/faqs/)
  5. [RDS FAQ](https://aws.amazon.com/rds/faqs/)
  6. [SQS FAQ](https://aws.amazon.com/sqs/faqs/)


## Other Resources
  1. [AWS Certifications - Prepare for Certification](https://aws.amazon.com/certification/certification-prep/)
  2. [AWS Training and Certification Portal](https://www.aws.training)
  3. [AWS Training Frequently Asked Questions (FAQs)](https://aws.amazon.com/training/faqs/)
