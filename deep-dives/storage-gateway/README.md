# Storage Gateway Deep Dive

## Table of Contents

<!-- MarkdownTOC depth=4 -->

- [Storage Gateway](#storage-gateway)
- [Types of Gateways](#types-of-gateways)
	- [File Gateway](#file-gateway)
	- [Cached-volume Gateway](#cached--volume-gateway)
	- [Stored-volume Gateway](#stored--volume-gateway)
	- [Tape Gateway](#tape-gateway)

<!-- /MarkdownTOC -->

---

## Storage Gateway

* Delivered as a virtual machine
* Can be deployed on-premise or as EC2 instance
* Can be used with Direct Connect
* Can implement bandwidth throttling
* can schedule snapshots
* In transit encryption: All traffic is encrypted via SSL/TLS
* At rest encryption: All data encrypted in S3 via AES-256


## Types of Gateways

### File Gateway
* File interface
	- NFS/CIFS access to S3/Glacier
* Provides a virtual file server, exposing S3 objects via standard file system calls
	- S3 buckets are available as NFS mounts
* Unlimited storage

### Cached-volume Gateway
* iSCSI based block storage
* Only "hot" data stays local, the remainder goes to S3, uses EBS snapshots
* Each volume up to 32TB 
	- 32 volumes supported

### Stored-volume Gateway
* iSCSI based block storage
* All data on the local volume, and point-in-time snapshots go to S3
	- Snapshots are stored as EBS snapshots in S3
* Each volume up to 16TB 
	- 32 volumes supported

### Tape Gateway
* iSCSI VTL interface, stored in S3 and archived to Glacier
* Virtual tape library
	- library sits on S3
	- 1500 virtual tapes
* Virtual tape shelf
	- Shelf is archived to Glacier
	- Unlimited storage
