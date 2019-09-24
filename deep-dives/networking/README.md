# VPC Networking Deep Dive

## General

* Networking topics you should know thoroughly:
	- physical layout of AZs and regions
	- VPC concept and how to create
	- Create public and private subnets
	- What NAT is and what "disable source/destination checks" is for
	- Route tables and routing terminology
	- IPv4 addressing and subnet mask notation (/16, /24, etc)
	- Other network terminology
		- MAC address
		- port
		- gateway vs router


## Networking 101

* Hardware components
    - NICs
    - Transport medium (cable, fiber, wireless)
    - Switches
    - Routers
* Software model
    - OSI Networking Model
        - OSI layers 1 through 7
            - Layer 7 = Application (HTTP/HTTPS)
            - Layer 6 = Presentation (TLS/SSL)
            - Layer 5 = Session (Setup/negotation/teardown)
            - Layer 4 = Transport (TCP)
            - Layer 3 = Network (IP/ARP)
            - Layer 2 = Data Link (MAC address)
            - Layer 1 = Physical (copper, fiber, cable)
        - TIP: Mnemonic to remember the seven layers:
            - Please Do Not Throw Sausage Pizza Away
        - Shared responsibility model
            - In general, AWS is responsible for layers 1 and 2
            - You are responsible for layers 3-7
    - Network layers/protocols to know
        - Layer 3 - IP addressing schemes:
            - Unicast
                - "One-to-one" messaging
                    - i.e. point-to-point messaging (like a phone call)
                - Explicit destination address
            - Broadcast
                - "One-to-all" messaging
                - Send to everything on network (not allowed within VPC)
                - IPv4 only (not IPv6)
                - Ex: DHCP clients
            - Multicast
                - "One-to-many" messaging
                - Group management is required to define which hosts are interested in receiving the packet
                    - e.g. IGMP (Internet Group Management Protocol)
            - Anycast
            - Geocast
        - Layer 4 (Transport) protocols:
            * TCP - connection-based, stateful, confirm receipt
                - Send, acknowledge, packets, MTU
            * UDP - stateless, connection-less (streaming media, DNS)
            * ICMP - management protocol (traceroute, ping)
                - Used by network devices to exchange info
* Addressing
    - MAC (media access control) address
        - unique ID assigned to NIC
    - IP address
    - Relationship between MAC and IP address
        - Ex: dual NICs
* Ephemeral ports
    - Short-lived transport protocol ports
    - Above "well known" IP ports (in general, well known ports lie between 1-1024)
    - Also known as "dynamic ports"
    - Suggested range is 49152-65535 but...
        - Linux kernels use 32568-61000
        - Windows defaults use starting at 1024
    - Flow:
        * Client connects to server on port 80
        * Server responds back with hello
        * Client then chooses ephemeral port and tells server to send back responses on that port
            - *client* chooses ephemeral port number
    - NACL and security group implications
        - OUTBOUND rule needs to take into account ephemeral port range


## VPC Networking

* CIDR addressing
    - max size for subnet: /16 (64k addresses)
    - min size for subnet: /28 (16 addresses)
    - /24 = 256 addresses
        - Divide/multiply by 2 to determine number of addresses based on mask, e.g:
            - /23 = 512
            - /22 = 1024
            - /21 = 2048
            - /20 = 4096
            - /19 = 8192
            - /18 = 16384
            - /17 = 32768
            - /16 = 65536
	- VPC Reserved IP addresses
		- 4 IPs are reserved for each subnet, plus broadcast address is unusable
			- Reserved/unusable addresses
				- .0 = network address
				- .1 = reserved for VPC router
				- .2 = reserved for DNS
				- .3 = reserved for future use
				- .255 - broadcast address (unusable)
    - Be careful! CIDR ranges cannot overlap between subnets (or between VPCs when peering)
* Physical to logical assignment of AZs is done at the account level
	- i.e. what my account thinks is us-west-2a is perhaps what another account thinks is us-west-2c
	- This is done to load-balance traffic across AZs
		- i.e. if us-west-2a is very popular default choice, then there will still be even distribution of usage
* Routing
	- Route Tables
		- Remember, most specific route wins
	- Border Gateway Protocol
		- Routing protocol for Internet
		- Propages info about the network to allow for dynamic routing
		- Required for Direct Connect
			- Optional for VPN
		- Alterative to BGP is static routes
		- Requires port 179 + ephemeral ports
		- Autonomous System Number (ASN) - unique endpoint identifier
		- Weighting is local to router and higher weight is preferred path for outbound traffic
	

## VPC Internet Access

* Internet Gateway
	- Horizontally scales, redundant, HA component for communication between VPC and internet
	- No availability risk or bandwidth constraints
	- Supports IPv4 and IPv6
	- Two purposes
		- Provide route table target for internet-bound traffic
		- Perform NAT for instances with public IP addresses
			- Does NOT perform NAT for instances with private IPs only
* Egress-Only Internet Gateway
	- IPv6 addresses are globally unique
		- Therefore public by default
	- Provides outbound internet access for IPv6 addresses instances
	- Prevents inbound access to those IPv6 instances
	- Must create a custom route for ::/0 to the Egress-Only Internet Gateway
	- Use Egress-Only Internet Gateway instead of NAT for IPv6
* NAT Instance
	- EC2 instance from AWS-provided AMI
	- Translate traffic from many private IPs to a single public IP and back
	- Doesn't allow public internet initiated connections into private instances
	- Not supported for IPv6
	- NAT instance must live on public subnet
	- Private instances must have route to NAT instance
* NAT Gateway
	- Fully managed NAT service
	- Must be created in public subnet
	- Uses an Elastic IP for life of gateway
	- Up to 5Gbps bandwidth that can scale to 45Gbps
	- For multi-AZ redundancy, create NAT gateways in each AZ with routes for private subnets to use the local Gateway
	- Can't use NAT Gateway for access to VPC peering, VPN or DX - so make sure to include specific routes to those


## On-Prem Network to VPC Connectivity

* AWS Managed VPN
	- Managed IPsec VPN connection over existing internet
	- Quick and usually simple method for making secure connection to VPC
	- Can be used as redundant link for Direct Connect
	- Supports static routes or BGP peering/routing
	- How to setup:
		- Designate an appliance to act as your customer gateway (usually the on-prem router)
		- Create VPN connection in AWS and download config file for your customer gateway
		- Configure customer gateway with config file
* AWS Direct Connect (DX)
	- Dedicated network connection over private lines straight into AWS backbone
	- Predictable network performance
	- Supports BGP peering/routing
	- Up to 10Gbps provisioned connections
	- NOT inherently redundant
		- TIP: use VPN as secondary backup
	- How to setup:
		- Work with your existing data networking provider
		- Create virtual interfaces (VIFs) to connect to VPCs (private VIF) or other AWS services like S3/Glacier (public VIF)
* AWS Direct Connect + VPN
	- IPsec VPN connection over private lines
	- Use this when you want added security of encrypted tunnel over DX
* AWS VPN CloudHub
	- Connect locations in hub and spoke manner using Virtual Private Gateway
	- Allows remote locations to communicate with each other via the hub (Virtual Private Gateway in AWS)
	- Reuses existing internet connection
	- Supports BGP routes to direct traffic
		- e.g. use MPLS first then CloudHub VPN as backup
	- How to setup:
		- Assign multiple Customer Gateways to a Virtual Private Gateway, each with their own BGP ASN and unique IP ranges
* Software VPN
	- You provide your own VPN endpoint/software
	- Use this option if you must manage both ends of VPN connection
	- How to setup:
		- Install VPN software via Marketplace applicance or on EC2 instance
* Transit VPC
	- Common strategy for connecting geographically dispersed VPCs and locations in order to create a global network transit center
	- Use when locations and VPC-deployed assets are across multiple regions and need to talk to each other


## VPC-to-VPC Connectivity

* Networking options for connecting VPCs
	- VPC Peering
		- AWS-provided network connectivity between two VPCs
		- Uses AWS backbone without going over internet
		- Does not support transitive peering
		- How to setup:
			- VPC Peering request is made
			- Accepter accepts request
				- either within account or across accounts
			- Both sides update route tables
	- Software VPN
	- Software to AWS Managed VPN
	- AWS Managed VPN
	- Direct Connect
	- PrivateLink
		- AWS-provided network connectivity between VPCs and/or AWS services using interface endpoints
		- Use when you want to keep private subnets truly private
		- How to setup:
			- Create endpoint for needed AWS service in all necessary subnets
			- Access via the provided DNS hostname
* VPC Endpoints
	- Interface endpoint
		- ENI with a private IP
		- uses DNS entries to redirect traffic
		- API Gateway, CloudFormation, CloudWatch
		- secure with security groups
	- Gateway endpoint
		- gateway that is a target for a specific route
		- uses prefix lists in route table to redirect traffic
		- S3, DynamoDB
		- secure with VPC Endpoint Policies
			- e.g. only allow access to S3 bucket when traffic coming through endpoint


## Route 53

* Route 53 Routing Policies
	- Simple
	- Failover
		- Uses healthchecks to switch from primary to failover
	- Geolocation
		- Route based on physical location of requester
			- Simple calculation of closest location
	- Geoproximity
		- Can have bias to increase/decrease radius of geolocation
			- Useful when you have overloaded regions due to densely populated areas
	- Latency
		- Route to lowest latency address
	- Multivalue Answer
		- Return collection of IP addresses (round-robin, let client choose)
	- Weighted


## CloudFront

* Distributed CDN (content delivery network)
	- Static assets
	- Or media such as 4k live and on-demand video streaming
* You should know:
	- How to create a CloudFront distribution
	- Understand edge location concept
* Integrated with Amazon Certificate Manager and supports SNI
	- SNI (Server Name Indication)
		- Allows server to offer up multiple certificates
		- Makes it possible to host many domains from single IP


## Elastic Load Balancers (ELBs)

* NOTE: Load balancers consume IP addresses within a VPC subnet
* Types of load balancers
	- Application Load Balancer
		- Layer 7
		- Path or host-based routing
		- Support for WebSockets
		- Supports SNI
		- Sticky sessions
		- Handles TLS termination
		- No static/Elastic IP
		- Supports user authentication
	- Network Load Balancer
		- Layer 4
		- Support for WebSockets
		- Does allow static/Elastic IP
		- Handles TLS termination (as of January 2019)
		- No SNI
	- Classic Load Balancer
		- Layer 4 / Layer 7
		- Sticky sessions
		- Handles TLS termination
		- No WebSockets
		- No SNI


## Links

* [Amazon Virtual Private Cloud Connectivity Options](https://d0.awsstatic.com/whitepapers/aws-amazon-vpc-connectivity-options.pdf)
* [Overview of AWS Security - Network Security](https://d1.awsstatic.com/whitepapers/Security/Networking_Security_Whitepaper.pdf)
* [AWS re:Invent 2017: Networking Many VPCs: Transit and Shared Architectures](https://www.youtube.com/watch?v=KGKrVO9xlqI)
* [AWS re:Invent 2017: Another Day, Another Billion Flows](https://www.youtube.com/watch?v=8gc2DgBqo9U)
* [DHCP Options Sets](https://docs.aws.amazon.com/vpc/latest/userguide/VPC_DHCP_Options.html)
* [VPC - Ephemeral Ports](https://docs.aws.amazon.com/vpc/latest/userguide/vpc-network-acls.html#nacl-ephemeral-ports)
