# EC2 Notes

* Resizable compute capacity in the cloud


#### Pricing Models:

* On Demand
  * Low cost and flexibility without up front or long term commitment
  * Applications with short term, spiky, or unpredictable workloads
  * First time applications on EC2 to understand behavior
* Reserved (by contract)
  * Steady state or predictable usage
  * Applications that require reserved capacity
  * Users able to make upfront payments to reduce computing costs further
  * 3 Types: Standard, Convertible, and Scheduled
* Spot (bid for reserved compute capacity)
  * Flexible start and end times
  * Only feasible at low compute prices
  * Urgent computing needs with for large amounts of additional capacity
* Dedicated Hosts
  * Regulatory requirements that don't allow for multi-tenant virtualization
  * Licensing that does not support multi-tenancy or cloud deployments
  * Can be purchased *On-Demand* or *Reserved* for up to 70% off On-Demand price

#### Instance Types:

* F1–Field programmable gate array (crazy tech)
* I3–High speed storage e.g. NoSQL DB's, Data Warehousing
* G3–Graphics intensive
* H1–MapReduce workloads, HDFS
* T3–Low cost, general purpose
* D2–Dense storage e.g. fileservers, data warehousing, Hadoop
* R5–Memory optimized (memory intensive workloads)
* M5–General purpose e.g. application servers
* C5–Compute optimized e.g. CPU intensive
* P3–General purpose GPU e.g. ML, Bitcoin mining
* X1–Memory optimized for things like Apache Spark
* Z1D–High compute capacity and high memory footprint
* A1–Arm-based workloads e.g. scaling web servers
* U-6tb1–Bare metal (??)

#### Security Groups

* Security Groups are stateful–all traffic allowed in is allowed back out (instead allow Network Access Control Lists)
* Can specify allow rules but not deny rules
* Can't block individual IP's or ports with Security Groups
* All inbound traffic blocked by default
* All outbound traffic allowed
* Changes take effect immediately
* Multiple EC2 instances within
* Multiple attached to one EC2 instance

#### EBS–Elastic Block Store

* Provides persistent block storage volumes for use with EC2
* Automatically replicated within availability zone for high availability and durability
* 5 Types:
  * General purpose (SSD)–balances price and performance
  * Provisioned IOPS (SSD)–mission critical operations (high performance) e.g. databases
  * Provisioned throughput HDD–frequent accessed, throughput intensive workloads e.g. Big Data and Data Warehouses
  * Cold HDD–less frequently accessed workloads e.g. fileservers
  * Magnetic–dinosaur shit for infrequently accessed workloads
* To launch in new Availability Zone: Create snapshot —> create AMI from snapshot —> launch instance from AMI in new AZ
* To launch in new region: Copy AMI to region and then launch instance in that region
* Volumes exist on EBS. Think of as virtual hard disk
* Snapshots exist on S3 and are incremental. Think of as photograph of disk. Best practice to stop instance before taking snapshot
* Can change volume sizes on the fly. Volumes always in same AZ as EC2 instance

#### Instance Store (Ephemeral Storage)

* Storage for the Root Device (Root Device Volume): either Instance Store or EBS Backed Volume
* Instance stored volume created from a template stored in S3
* Cannot add additional instance store volumes after launching instance
* Cannot be stopped (only terminated). If underlying host fails, data is lost.

#### ENI vs ENA vs EFA

* ENI–Elastic Network Interface (virtual network card)
* EFA–Elastic Fabric Adapter (accelerate high performance computing e.g. ML), lower and more consistent latency than TCP transport, OS-bypass (bypasses kernel, only linux)
* EN–Enhanced Networking (uses single-root I/O virtualization) (ENA is a subset of this), supports network speeds up to 100 GB/s

#### Encryption of Volumes

* On instance creation (new feature)
* To encrypt already running instance create a snapshot of it –> copy the snapshot but encrypt the copy –> create AMI from encrypted snapshot –> launch instance from encrypted snapshot

#### CloudWatch

* Monitors performance
  * Compute: EC2 instances, Auto-scaling groups, ELBs, Route53 Health Checks
  * Storage & Content Delivery: EBS Volumes, Storage gateways, CloudFront
* CAREFUL: CloudTrail monitors API calls by user/IP in AWS platform (think auditing)
* Standard Monitoring 5 minutes; Detailed monitoring 1 minute
* Events (state changes in AWS resources), Alarms, Logs

##### * use IAM Roles vs Access Key/Secret Access Key to access AWS via CLI (they are far more secure), Roles are easier to manage, Roles are universal

#### EFS–Elastic File System

* File storage service for EC2
* Storage is elastic so grows and shrinks automatically.
* Supports NFSv4, only pay for what is used, can scale up to petabytes, can support thousands of NFS connections, stored across multiple AZ's in region, read after write consistency
* See FSx for Lustre alternative: used for HPC (High Performance Computing)

#### EC2 Placement Groups

* Three types:
  * Clustered: group of instances in same AZ (use for low network latency and/or high network throughput)
  * Spread: group of instances placed on distinct hardware (small number of critical instances that are kept separate from each other... think individual instances)
  * Partitioned: Divides instance groups into logical segments placed in partitions. Isolate impact of hardware failure. (HDFS, HBase, Cassandra)

#### Web Application Firewall (WAF)

* Monitor HTTP/HTTPS to CloudFront, ALB, API Gateway
* Control access to content
* Configure allowed IP addresses, query params necessary on requests
* Three behaviors:
  * Allow requests except ones specified
  * Block all requests except ones specified
  * Count the requests that match the properties specified
