# Route 53 Notes

#### DNS

* Lookup a domain name; get an IP address (IPv4 vs IPv6)
* IPv4 is a 32 bit field and has over 4 billion different addresses
* IPv6 was created to solve this depletion issue and has an address space of 128bits which is 340 undecillion addresses (created to solve issue of running out of IPv4 addresses)
* **Top-level domain** (.com, .edu, .gov, .co.uk) controlled by the Internet Assigned Numbers Authority (IANA) in a root zone database which is a db of all top level domains.
* All names in given domain have to be unique so registrars are used to assign domain names directly under one or more top-level domains. These are registered with the InterNIC, a service of ICANN, which enforces uniqueness of domain names across the internet. Each domain name becomes registered in a central db known as the WhoIS database.  Amazon, GoDaddy.com, etc popular domain registrars.
* SOA (Start of Authority Record)
  * name of server that supplied the data for the zone
  * the admin of zone
  * current version of the data file
  * default # of seconds for the TTL file on the resource records
* NS (Name Server) Records: used by top level domain servers to direct traffic to the Content DNS server which contains authoritative DNS records
* An "A" Record is the fundamental type of DNS record. Stands for "Address". Used by a computer to translate the name of the domain to an IP address.
* CNAME (A Canonical Name) used to resolve one domain name to another e.g. mobile.site.com --> m.site.com
* Alias records are used to map resource record sets in your hosted zone to ELB's, CF dists, or S3 buckets that are configured as websites. Work like CNAME records.
* Exam tips: ELBs do not have pre-defined IPv4 addresses; resolved using DNS name... Understand difference between Alias Record and CNAME; always choose Alias over CNAME if given the choice... Common types: SOA, NS, A, CNAMES, MX, PTR

#### Routing Policies

* Simple Routing
  * One record with multiple IP addresses; if multiple values are specified in a record then Route53 returns all of them in a random order
* Weighted Routing
  * Split traffic based on different weights assigned e.g. 10% to one instance, 90% to another
  * Can set Health Checks on individual record sets; if a record set fails HC it will be removed from Route53 until it passes the HC
* Latency-based Routing
  * Route based on lowest network latency i.e. which region will give them the fastest response time
* Failover Routing
  * Used for creating an active/passive setup e.g. primary site on one instance failover to passive site if health check fails
* Geolocation Routing
  * Choose where traffic is sent based on location of users
* Geoproximity Routing (Traffic Flow Only)
  * Gets complicated! Beyond scope of Assoc. Solutions Architect exam
  * Based on geographic location of users and resources. Route more or less traffic to resource based on a value known as *bias*
* Multivalue Answer Routing
  * Same as simple with one key difference: return multiple values, such as IP addresses, for web servers.  Route53 returns only values for healthy resources.  Key difference it allows to put health checks on each record set.

#### Exam Tips

* ELBs do not have pre-defined IPv4 addresses; resolve them to a DNS name
* Use Alias Record over CNAME if given choice; understand both of these
* **REALLY UNDERSTAND ALIAS RECORD AND CNAME** did poorly testing on this
