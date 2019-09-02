# AWS Encryption Deep Dive

## Table of Contents

<!-- MarkdownTOC depth=4 -->

- [Encryption essentials](#encryption-essentials)
- [Encryption in practice](#encryption-in-practice)
- [Digital certificates](#digital-certificates)
	- [AWS Certificate Manager](#aws-certificate-manager)
	- [AWS Certificate Manager Private Certificate Authority](#aws-certificate-manager-private-certificate-authority)
- [Symmetric cryptography](#symmetric-cryptography)
- [CloudHSM](#cloudhsm)
- [KMS](#kms)
- [Links](#links)
- [Whitepapers](#whitepapers)

<!-- /MarkdownTOC -->

---

## Encryption essentials

* Encryption
	- process of converting plaintext to ciphertext
		- requires an encryption key
* "Where" to encrypt
	- Encryption at rest
		- data is encrypted when it is stored
		- where does encryption happen? two choices:
			- Client-side encryption
				- encrypt data before you give it to an AWS service
				- can use AWS Encryption SDK to perform client-side encryption
				- where to get keys? On-prem, KMS, CloudHSM
			- Server-side encryption
				- ask AWS service to encrypt data after you upload it
				- you tell service what CMK key to use when encrypting
	- Encryption in transit
		- data encrypted when transmitted to another computer
* Types of encryption
	- Hashing
		- Output of hash algorithm is called hash value or digest
			- same input will always produce same hash
			- one way only - impossible to reverse hash value to original data
			- given only the hash value, it is infeasible to create another string that produces same hash value (a "collision")
		- Requires no keys
		- Examples of hash algorithms:
			- SHA
			- MD5
			- bcrypt
				- password hashing function based on the Blowfish cipher
				- incorporates a salt to protect against rainbow table attacks
				- adaptive function: over time, the iteration count can be increased to make it slower for resistance to brute-force search attacks
	- Symmetric key
		- Encryption and decryption keys are the same
		- Communicating parties must have the same key in order to communicate securely
		- Examples of symmetric key encryption implementations:
			- 3DES
			- AES
			- Blowfish
			- RC4
			- AWS KMS
	- Public key (aka asymmetric key)
		- the encryption key (public key) is published for anyone to use to encrypt messages
		- only the receiving party has access to the decryption key (private key) that enables messages to be read
		- message sent from one computer to another won't be secure since the public key used for encryption is known to anyone
			- but no one can *read* the message without the private key
		- key pair based on prime numbers
		- Examples of public key implementations:
			- SSL / TLS
			- PGP (Pretty Good Privacy)
			- RSA

## Encryption in practice

* TLS (Transport Layer Security)
	- Three main components:
		- authentication
		- encryption
		- integrity
	- TLS connection starts with "TLS handshake" sequence to initiate session
		- Two parts to handshake:
			- server proves identity to the client via SSL certificate
				- when requesting secure page, browser checks that:
					- certificate comes from a trusted party
					- certificate is valid
						- not expired and not on CRL (certificate revocation list)
					- certificate matches domain of web server
			- establish shared encryption key (symmetric key)
				- uses public key encryption to send encrypted data key
					- browser creates a new symmetric key (the data key) and encrypts message with this key
					- browser then encrypts the symmetric key with the public key of web server
					- web server uses private key to decrypt symmetric key
	- Once session established, messages are sent encrypted using data key
		- Both parties can use symmetric key to decrypt message
	- Integrity 
		- verifies both the data integrity (message hasn't been tampered with) and the authentication of a message
		- message is signed with message authentication code (MAC)
			- HMAC (hashed-key message authentication code) 
				- hashes data in message, and combines resulting hash with key
					- key is negotiated during the TLS handshake
				- the result is short MAC that is sent alongside the message to ensure that the message has not been altered in transit


## Digital certificates

* Digital certificates are unique identifiers used to prove identity of server
* PKI (public key infrastructure)
	- system for the creation, storage, and distribution of digital certificates
	- PKI creates digital certificates which map public keys to entities, securely stores these certificates and revokes them if needed
	- PKI allows one party to establish the identity of another party using certificates if they both trust a third-party (CA)
* Types of certificate validation
	- Domain validation (DV)
	- Organizational validation (OV)
	- Extended validation (EV)
* Certificate authority (CA)
	- Functionality
		- certificate authority is middleman that each computer (browser client and web server) trusts
		- CA confirms identity of web server
		- CA digitally signs the certificate with their own private key (so browsers can verify CA)
		- certificate contains public key for web server (private key is only known to web server)
	- CA types
		- Public CAs
			- public CAs must follow strict rules, provide operational visibility, and meet security standards imposed by the browser/OS vendors
			- browser/OS vendors decide which CAs they trust automatically
		- Private CAs
			- used for managing private certificates
			- administrator must explicitly configure applications to trust certificates issued by private CAs
	- Certificate Revocation List (CRL)
		- Lists certificates that have been revoked by the issuing CA before their scheduled expiration date
			- Certificates on this list should no longer be trusted
* Self-signed certificates
	- Certificates issued without a CA
	- Act as their own root
	- They can be used to provide on the wire encryption
	- Limitations
		- Cannot be used to verify identity
		- Cannot be revoked

### AWS Certificate Manager

* Provision, manage, and deploy public and private SSL/TLS certificates for use with AWS services
	- Must deploy to ACM-integrated AWS resources
		- e.g ELB, CloudFront, API Gateway
	- certificates are free (you only pay for underlying ACM-integrated AWS resources)
* Certificates are used to establish identity of resources (public and private) 
* Removes time-consuming manual process of purchasing, uploading, and renewing SSL/TLS certificates

### AWS Certificate Manager Private Certificate Authority

* Managed private CA service that allows for managing your private certificates
* Pay monthly for the private CA and for private certificates you issue
* You can use private certificates issued with ACM Private CA wherever you choose
	- does *not* have to be with ACM-integrated AWS resources


## Symmetric cryptography

* With symmetric cryptography, you have a single key to encrypt/decrypt data.
	- Need to protect your symmetric key
	- Encrypt the symmetric key with a "master key" to produce an encrypted data key.
	- At some point, plain text keys need to exist somewhere (i.e you can't keep encrypting master key).
		- Where should you store your plain text master key?  Options include:
			- Option 1: On-prem in your HSM (hardware security module)
				- Pros: 
					- you control the device, authentication, authorization
					- it looks familiar to auditors
				- Cons: 
					- latency, availability, durability, scalability are all YOUR responsibility
					- limited integration with cloud managed services
			- Option 2: In-cloud in your HSM (e.g. AWS CloudHSM)
				- Pros: 
					- you control authentication, authorization
					- lower latency to apps in the cloud
					- AWS manages the device, making it easier to have higher availability/durability/scalability
					- it looks familiar to your auditors (for example, during FIPS-142 audit reviews)
				- Cons: 
					- limited integration with cloud managed services
						- uses cryptographic protocols like PKCS#11, JCE, CNG
			- Option 3: In-cloud in a managed HSM (e.g. AWS KMS)
				- Pros: 
					- you control authentication, authorization
					- lower latency to apps in cloud
					- AWS owns all availability/durability/scalability
					- deep integration with cloud managed services
				- Cons: 
					- looks unfamiliar to auditors

## CloudHSM

* AWS CloudHSM
	- HSM = Hardware Security Module
		- provides secure key storage and cryptographic operations within tamper-resistant hardware device
		- designed to securely store cryptographic key material and use the key material...
			- *without* exposing it outside the cryptographic boundary of the hardware
	- cloud-based hardware security module (HSM) to generate and use your own encryption keys
	- you manage your own encryption keys using FIPS 140-2 Level 3 validated HSMs
		- single-tenant access
		- reside in your own VPC
		- tamper-resistant HSM instances
	- fully managed service, AWS handles:
		- hardware provisioning
		- software patching
		- high-availability
		- backups
			- automatic encrypted backups taken every 24 hours
	- easy to scale HSM capacity quickly by adding and removing HSMs from your cluster on-demand
		- automatically load balances requests and duplicates keys to all other HSMs in the cluster
	- securely transfer exportable keys between CloudHSM and most commercial HSMs using one of several supported RSA "key wrap" methods
	- application access is via CloudHSM client
		- client runs as a daemon on your EC2
			- mutual TLS between HSM client and HSM cluster
		- client daemon listens to your application using a local Unix Domain Socket
			- protocols supported include:
				- PKCS#11
					- C_Decrypt
					- C_Encrypt
					- C_GenerateKey
				- Java Cryptography Extensions (JCE)
				- Microsoft CryptoNG (CNG)


## KMS

* Overview
	- AWS Key Management Service (AWS KMS):
		- Uses distributed fleet of FIPS 140-2 validated hardware security modules (HSMs)
		- Scaled for the cloud
	- Functionality:
		- generate and manage cryptographic keys
		- operates as a cryptographic service provider for protecting data
	- Symmetric encryption
		- AES-256
* Customer master keys
	- your key hierarchy starts with a CMK
	- A CMK can be used to directly encrypt data blocks up to 4 KB
		- You can encrypt up to 4KB of arbitrary data such as an RSA key, a database password, or other sensitive information
		- To encrypt data of any size, create arbitrary data key and secure data key with CMK (encrypt/decrypt then happens outside KMS with data key)
	- Two categories of CMKs
		- AWS-managed
			- Can only be used to protect resources within the specific AWS service for which it’s created
				- Does not provide level of granular control that a customer-managed CMK provides
			- E.g. SSE-KMS for S3 encryption
		- Customer-managed
			- Three options for creating the underlying key material
				1. AWS KMS created keys
					- you let KMS randomly create cryptographic material when creating key
					- AWS KMS key generation is performed on the HSM (uses NIST SP800-90A Deterministic Random Bit Generator)
				2. Customer-provided keys (you import cryptographic material to KMS after creating key)
				3. Custom key stores (use keys already stored in CloudHSM)
					- allows you to access keys stored in CloudHSM via KMS
						- note CloudHSM exists in your VPC, whereas KMS HSM is outside your VPC
					- to implement, AWS essentially created a "connector" that ties CloudHSM to KMS endpoint, effectively allowing better cloud service integration for custom keys	
					- best of both worlds 
						- combines single-tenant HSMs under your control with KMS ease of use & integration
 * Identity and Access Management
	- AWS KMS uses Hardware Security Modules (HSMs) to protect the security of your keys
	- CMK *never* leaves the HSM
		- encryption/decryption with CMK happens within KMS itself
 		- all objects encrypted under a CMK can be decrypted only on an HSM via a call through KMS
 	- IAM protection
		- Use IAM policies with key policies to control access to customer master keys (CMKs) 
		- identity-based policies (IAM policies)
			- attached to identities (users, groups, roles)
		- resource-based policies
			- attached to resources
		- With KMS, you attach resource-based policies to your CMKs (these are called key policies)
			- primary way to control access to CMKs
			- defines permissions on the use and management of the key
		 	- Key policies specify a resource, action, effect, principal, and conditions to grant access to CMKs. 
		- Enforce least privilege by granting granular access to KMS API calls using identity-based IAM policy
		- Sample permissions that can be applied to Customer Master Key:
			- can only be used for encrypt/decrypt by <these users/roles> in <these accounts>
			- can be used by app A to encrypt and only used by app B to decrypt (quasi-asymmetric key)
			- can be managed only by this set of admin IAM users or IAM roles
			- can be used by <these external accounts> but only for encrypt/decrypt, not admin tasks
	 - Encryption context
		- Additional layer of authentication for your KMS API calls
		- if supplied with encrypt operation, it must be supplied for decrypt
		- encryption context should not be considered sensitive information and should not require secrecy.
			- encryption context will be logged in CloudTrail events
		- AWS services that use KMS use encryption context to limit scope of keys
			- e.g. EBS:
				- uses volume ID as encryption context when encrypting/decrypting a volume
				- uses snapshot ID as context when taking a snapshot
					- if EBS did not use this encryption context, an EC2 instance would be able to decrypt any EBS volume under that specific CMK
		- encryption context helps to further ensure that only authorized parties and/or applications can access and use the CMKs
			- party will need:
				- have IAM permissions to AWS KMS
				- CMK policy that allows them to use the key in the requested fashion
				- know the expected encryption context values
* Auditing
	- KMS integrates with CloudTrail
	- To audit usage of your KMS keys, enable CloudTrail logging in your AWS account
		- All KMS API calls made on keys in your AWS account then automatically logged in files delivered to the S3 bucket you specify
* Infrastructure security
	- When operational with key material provisioned:
		- no AWS operator access to HSM; no software updates allowed
		- clear text key *NEVER* leaves HSM; only ciphertext
	- After reboot and in a non-operational state:
		- no key material on host
		- software can only be updated when the following occurs:
			- multiple AWS employees have reviewed the code
			- under quorum of multiple authorized KMS operators
		- this is to prevent malware being loaded onto HSMs (for example, to add an API to extract key in clear text)
	- 3rd party verified evidence
		- FIPS 140-2 level 2 overall with Level 3 physical security, design assurance, cryptography
		- SOC 1 - Control 4.5
			- "customer master keys used for cryptographic operations in KMS are logically secured so that no single AWS employee can gain access to key material"
* Data protection
	- Common use cases
		- Secret management using KMS and S3
		- Encrypting Lambda env vars
		- Encrypting data within Systems Manager Parameter Store
		- Data at rest encryption
			- S3, EBS, RDS, DynamoDB, etc
				- Ex: Encrypting data being written to EBS volume:
					1. EBS obtains encrypted volume key under a CMK through KMS over TLS and stores the encrypted key with the volume metadata
					2. When the EBS volume is mounted, the encrypted volume key is retrieved
					3. A call to KMS over TLS is made to decrypt the encrypted volume key
						- KMS identifies the CMK and makes an internal request to an HSM to decrypt the encrypted volume key
						- KMS then returns the volume key back to EC2 host that is mounting volume
					4. The volume key is used to encrypt and decrypt all data going to and from the attached Amazon EBS volume
						- EBS retains the encrypted volume key for later use in case the volume key in memory is no longer available
* KMS at scale
	- Recommendation: use envelope encryption to scale your KMS implementation
		- Envelope encryption
			- encrypt plaintext data with a unique data key, and then encrypt data key with a key encryption key (KEK)
			- Within AWS KMS, the CMK is the KEK
			- you encrypt your message with the data key and then encrypt the data key with the CMK
				- encrypted data key can be stored along with the encrypted message
				- you can cache the plaintext version of the data key for repeated use, reducing the number of requests to AWS KMS 
		- AWS Encryption SDK includes API support for performing envelope encryption using a CMK from KMS
* Incident Response
	- CMK rotation
		- When CMK is rotated, new HBK (hardware backed HSM key) is created and marked as active key for all new requests
		- current active key is moved to the deactivated state and remains available for use to decrypt any existing ciphertext values
		- older ciphertexts can be re- encrypted to the new HBK by calling the ReEncrypt API call
	- CMK deletion
		- To ensure that a CMK is not deleted by mistake, KMS enforces a waiting period before actually being deleted
			- Minimum waiting period is 7 days
				- You can increase this to a maximum of 30 days
			- During "pending deletion" waiting period, all calls to use the key for cryptographic operations will fail


## Links

* [Cryptographic hash function](https://en.wikipedia.org/wiki/Cryptographic_hash_function)
* [bcrypt](https://en.wikipedia.org/wiki/Bcrypt)
* [How Encryption Works](https://computer.howstuffworks.com/encryption.htm)
* [A Stick Figure Guide to the Advanced Encryption Standard (AES)](http://www.moserware.com/2009/09/stick-figure-guide-to-advanced.html)
* [AWS Certificate Manager](https://aws.amazon.com/certificate-manager/)
* [AWS Certificate Manager Private Certificate Authority](https://aws.amazon.com/certificate-manager/private-certificate-authority/)
* [Are KMS custom key stores right for you?](https://aws.amazon.com/blogs/security/are-kms-custom-key-stores-right-for-you/)
* [AWS re:Inforce 2019: Encrypting Everything with AWS - SEP402](https://www.youtube.com/watch?v=oqHLLbOoxDg)
* [AWS re:Inforce 2019: Achieving Security Goals with AWS CloudHSM - SDD333](https://www.youtube.com/watch?v=_gezaWmwzYY)


## Whitepapers

* [AWS Key Management Service Best Practices](https://d0.awsstatic.com/whitepapers/KMS-Best-Practices.pdf)
* [AWS Key Management Service Cryptographic Details](https://d1.awsstatic.com/whitepapers/KMS-Cryptographic-Details.pdf)
