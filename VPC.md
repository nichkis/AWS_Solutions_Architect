# VPC Notes

#### Overview

* Should be able to build out own VPC from memory before going into exam!!!
* Leverage multiple layers of security: security groups, network ACLs
* VPC lives within a region
* Public and Private subnets (see bastion hosts to connect to private instance from public instance)
* See CIDR.xyz website for CIDR range visualization
* Consider Certified Advanced Networking Exam (one of most coveted certs in tech because it's fucking hard to get)
* Launch instances into subnet of choice
* Assign custom IP address ranges in each subnet
* Configure route tables between subnets
* Create internet gateways and attach to VPC
* Much better security control
* Default VPC vs. Custom VPC
* VPC Peering (connect one to another by direct network route using private IP addresses); Peering is a star configuration; Peer between regions
* Exam tips:
  * Think of VPCs as logical datacenters in AWS
  * Consists of Internet Gateways (or Virtual Private Gateways), Route Tables, Network Access Control Lists, Subnets, and Security Groups
  * 1 Subnet = 1 Availability Zone
  * Network ACL stateless (allow/disallow), Security Groups stateful
  * NO TRANSITIVE PEERING
* When creating a VPC a default Route Table, Network Access Control List, and Security Group are created; No subnets created initially nor internet gateways; AZ's are randomized e.g. US-EAST-1A in one account may be different than the US-EAST-1A in another account; Amazon always reserves 5 IP addresses within subnets; Only 1 internet gateway per VPC; Security Groups can't span VPCs

#### Network Address Translation (NAT)

* NAT Instances:
  * Must disable source/destination checks (acting as bridge from private subnets to internet gateway)
  * Must be in public subnet
  * Must be a route out of private subnet to NAT instance
  * The amount of traffic NAT instances can support depends on instance size —> increase if bottlenecking.
  * Create high availability using Autoscaling Groups, multiple subnets in different AZ's, and script to automate failover
  * NAT Instances always behind a security group
* NAT Gateways: (always use over NAT Instances)
  * Redundant inside AZ
  * Preferred by enterprise
  * Scales automtically
  * No need to patch
  * Not associated with security groups
  * Automatically assigned public IP address
  * Remember to update Route Tables
  * No need to disable source/destination checks
* See Ephemeral Ports

#### Network Access Control Lists (NACLs)

* VPC automtically comes with a default network ACL; by default it allows all inbound and outbound traffic
* Create custom network ACLs; by default each custom network ACL denies all inbound and outbound traffic until rules are added
* Each subnet in VPC must be associated with a netowork ACL; if subnets are not explicitly associated with a network ACL then they are associated with the default network ACL
* Block IP addresses using network ACLs; can't do this with security groups
* Can associate a network ACL with multiple subnets; however, only a subnet can be associated with only one network ACL at a time. When associating a subnet to a network ACL the previous association is removed
* Network ACLs contain a numbered list of rules that is evaluated in order, starting with the lowest numbered rule (use increments of 100)
* Separate inbound and outbound rules; each rule can either deny or allow traffic
* Network ACLs are stateless

#### VPC Flow Logs

* Enables capturing information about the IP traffic going to and from network interfaces in a VPC; can view/retrieve data from AWS CloudWatch
* At 3 levels: VPC, Subnet, Network Interface
* After a flow log has been created it's configuration cannot be changed e.g. cannot change IAM Role
* Cannot enable flow logs or VPCs that are peered with a VPC unless those VPCs are your AWS account
* Can tag flow logs
* Not all traffic monitored. For example, instances that contact the Amazon DNS server, traffic to and from instance metadata, DHCP traffic, traffic for reserved IP to default VPC

#### Bastion Hosts

* Used to securely administer EC2 instances
* NAT Instances/Gateways used to provide internet traffic to instances in private subnets
* Cannot use NAT Gateway as a bastion host

#### Direct Connect

* Done on dedicated lines.  Direct and privately connect from premises to AWS. Network experience more consistent than Internet-based connections. Useful for high throughput workloads

#### Global Accelerator and VPC Endpoints

* Global Accelerator directs traffic to optimal endpoints over the AWS global netowrk; provides with 2 static IP addresses that are associated with accelerator (can use your own)
* Global Accelerator includes following components:
  * Static IP addresses
  * Accelerator – directs traffic to optimal endpoints over AWS global network; includes one or more listeners
  * DNS Name – assigns default DNS name to each accelerator that points to static IP addresses
  * Network Zone – services static IP addresses from unique IP subnet. Similar to AZ it is an isolated unit with own set of physical infrastructure
  * Listener – processes inbound connections from clients to GA based on port range and configured protocol. Each listener has one or more Endpoint Groups and these Endpoint Groups are associated with listerns by Regions traffic will be distributed to. Traffic is then distributed to optimal endpoints within the group.
  * Endpoint Group – Each associated with a specific AWS region. Include one or more endpoints in region. Can increase/decrease the percent traffic sent to an endpoint with something called a traffic dial (used for blue/green deployment for new releases)
  * Endpoint – NLBs, ALBs, EC2 instances, or Elastic IPs. ALB can be internet facing or internal.  Can configure weights for each endpoint (useful for performance testing)
* VPC Endpoints:
  * They are virtual devices
  * Enable private connection of VPC to AWS Services without requiring an internet gateway, NAT device, VPN connection, or AWS Direct connection
  * Horizontally scaled, highly available, and redundant that allow communication between instances within VPC and services without imposing availability risks or bandwidth constraints on network traffic
  * Two types: Interface Endpoints & Gateway Endpoints
  * An Interface Endpoint is an ENI with a private IP address that serves for an entry for traffic destined to a supported service
  * Gateway Endpoints currently only supported for DynamoDB and S3
