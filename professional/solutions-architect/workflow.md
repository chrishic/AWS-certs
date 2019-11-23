# Workflow


## AWS Step Functions

* Managed workflow and orchestration platform
* Define your app as a state machine
* Create tasks, sequential steps, parallel steps, branching paths or timers
* Uses Amazon State Language declarative JSON
* Apps can interact and update the stream via Step Function API
* Visual interface describes flow and realtime status
* Detailed logs for each step execution


## Simple Workflow Service (SWF)

TBD.


## Step Functions vs. Simple Workflow Service

* In general, use Step Functions
	- They provide out-of-the-box coordination of AWS service components
* Use SWF when you need to support external processes or specialized execution logic
	- E.g. need to perform manual steps


## AWS Batch

* Management tool for creating, managing and executing batch-oriented tasks using EC2s
	- Use case:
		- Scheduled or recurring tasks that do not require heavy logic
- Steps for using:
	- 1. Create a compute environment
		- Managed or unmanaged
		- Spot or On Demand
		- Number of vCPUs
	- 2. Create job queue with priority and assigned to a compute environment
	- 3. Create job definition
		- Can be script or container
	- 4. Schedule the job
		- If you chose managed compute environment, Batch will scale out to handle jobs
