# Storage Gateway Deep Dive

## Table of Contents

<!-- MarkdownTOC depth=4 -->

- [Storage Gateway](#storage-gateway)
- [Types of Gateways](#types-of-gateways)
	- [File Gateway](#file-gateway)
	- [Volume Gateway Cached Mode](#volume-gateway-cached-mode)
	- [Volume Gateway Stored Mode](#volume-gateway-stored-mode)
	- [Tape Gateway](#tape-gateway)

<!-- /MarkdownTOC -->

---

## Storage Gateway

* Delivered as VM
	- Can be deployed on-premise with VMWare or Hyper-V or via hardware appliance
* Provides local storage resources backed by S3 and Glacier
* Often used in DR preparedness to sync to AWS
* Useful in cloud migrations
	- can be used in lazy manner to move data from on-prem to AWS
* Can be used with Direct Connect
* Can implement bandwidth throttling
* Can schedule snapshots
* Encryption support
	- In transit: All traffic is encrypted via SSL/TLS
	- At rest: All data encrypted in S3 via AES-256


## Types of Gateways

### File Gateway

* File interface
	- NFS/CIFS access to S3/Glacier
* Provides a virtual file server, exposing S3 objects via standard file system calls
	- S3 buckets are available as NFS/CIFS mounts
* Unlimited storage

### Volume Gateway Cached Mode

* Formerly known as "Cached-volume Gateway"
* iSCSI based block storage
* Only "hot" data stays local, the remainder goes to S3, uses EBS snapshots
* Each volume up to 32TB 
	- 32 volumes supported

### Volume Gateway Stored Mode

* Formerly known as "Stored-volume Gateway"
* iSCSI based block storage
* All data stored on the local volume, and point-in-time snapshots go to S3
	- Snapshots are stored as EBS snapshots in S3
* Each volume up to 16TB 
	- 32 volumes supported

### Tape Gateway

* Virtual media changer and tape library for use with existing backup software
* iSCSI VTL interface
* Virtual tape library
	- library sits on S3
	- 1500 virtual tapes
* Virtual tape shelf
	- Shelf is archived to Glacier
	- Unlimited storage
