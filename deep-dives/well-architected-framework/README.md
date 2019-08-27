# The AWS Well Architected Framework

## Table of Contents

<!-- MarkdownTOC depth=4 -->

- [Well Architected Framework](#well-architected-framework)
- [General Design Principles](#general-design-principles)
- [Pillars](#pillars)
	- [Operational Excellence](./operational-excellence.md)
	- [Security](./security.md)
	- [Reliability](./reliability.md)
	- [Performance Efficiency](./performance-efficiency.md)
	- [Cost Optimization](./cost-optimization.md)
- [The Well Architected Review](#the-well-architected-review)
- [The AWS Well Architected Tool](#the-aws-well-architected-tool)
- [Links](#links)
- [Whitepapers](#whitepapers)

<!-- /MarkdownTOC -->

---

## Well Architected Framework

* What is it? Why is it useful?
	- documents set of foundational questions to help determine if a specific architecture aligns with cloud best practices
	- provides consistent approach to evaluating systems against cloud best practices
	- helps advise changes necessary to make specific architecture align with best practices
* Comprised of 3 components:
	- General design principles
	- Pillars
	- Evaluation (series of questions & answers) - the "Well Architected Review"


## General Design Principles

* Cloud-native has changed everything. In cloud, you can:
	- Stop guessing capacity needs
	- Test at scale
	- Automate all the things to make experimentation easier (hello CloudFormation)
	- Allow for evolutionary architectures (you are never *stuck* with a particular technology)
	- Drive architectures using data (allows you to make fact based decisions on how to improve your workload)
	- Improve through game days


## Pillars

### [Operational Excellence](./operational-excellence.md)

### [Security](./security.md)

### [Reliability](./reliability.md)

### [Performance Efficiency](./performance-efficiency.md)

### [Cost Optimization](./cost-optimization.md)


## The Well Architected Review

* Centered around the question "Are you well architected?"
* The Well Architected review provides a consistent approach to review workload against current AWS best practices and gives advice on how to architect for the cloud
* Benefits of the review
	- Build and deploy faster
	- Lower or mitigate risks
	- Make informed decisions
	- Learn AWS best practices


## The AWS Well Architected Tool

* Cloud-based service available from the AWS console
* Provides consistent process for you to review and measure your architecture using the AWS Well-Architected Framework
* Helps you:
	- Learn
	- Measure
	- Improve
* Improvement plan
	- Based on identified high and medium risk topics
	- Canned list of suggested action items to address each risk topic
* Milestones
	- Makes a read-only snapshot of completed questions and answers
* Best practices
	- Save milestone after initially completing workload review
	- Then, whenever you make large changes to your workload architecture, perform a subsequent review and save as a new milestone


## Links

* [AWS Well-Architected](https://aws.amazon.com/architecture/well-architected/)
* [AWS Well-Architected Framework - Online/HTML version](https://wa.aws.amazon.com/index.html)
	- includes drill down pages for each review question, with recommended action items to address that issue
* [AWS Well-Architected Tool](https://aws.amazon.com/well-architected-tool/)


## Whitepapers

* [AWS Well-Architected Framework](https://d1.awsstatic.com/whitepapers/architecture/AWS_Well-Architected_Framework.pdf)
* [Operational Excellence Pillar](https://d1.awsstatic.com/whitepapers/architecture/AWS-Operational-Excellence-Pillar.pdf)
* [Security Pillar](https://d1.awsstatic.com/whitepapers/architecture/AWS-Security-Pillar.pdf)
* [Reliability Pillar](https://d1.awsstatic.com/whitepapers/architecture/AWS-Reliability-Pillar.pdf)
* [Performance-Efficiency Pillar](https://d1.awsstatic.com/whitepapers/architecture/AWS-Performance-Efficiency-Pillar.pdf)
* [Cost Optimization Pillar](https://d1.awsstatic.com/whitepapers/architecture/AWS-Cost-Optimization-Pillar.pdf)
