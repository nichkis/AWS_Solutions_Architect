# High Availability Architecture

#### Load Balancers

* Application Load Balancers
  * Load balancing of HTTP and HTTPS traffic
  * Operate at Layer 7 and are application aware
* Network Load Balancers
  * Load balancing of TCP where extreme performance is imperative
  * Operate at Layer 4 connection level
  * Can handle millions of requests per second at ultra-low latencies
  * Intelligent
* Classic Load Balancers
  * Legacy elastic load balancers
  * Can use Layer 7 or Layer 4
  * If application failing responds with 504
* X-Forwarder-For Header to get client public IP address not load balancer IP
* Sticky sessions allow binding of a users IP address to a specific EC2 instance. On classic and application load balancers although for ALBs traffic is sent at the Target Group level
* Cross Zone Load Balancing can route traffic to instances across zones
* Path Patterns make it possible forward requests by URL path

#### Auto Scaling

* 3 Components:
  * Groups – logical component; Webserver group, Database group, Application group
  * Configuration Templates – used to scale/provision EC2 instances
  * Scaling Options – see dynamic scaling (CPU utilization), or scheduled (business hour ramp up)
    1. Maintain current instance levels at all times
    2. Scale manually
    3. Scale based on schedule
    4. Scale based on demand
    5. Predictive scaling

#### HA Architecture

* Design for failure... Everything fails!
* Use multiple AZs in multiple regions wherever possible
* Know difference between Multi-AZ and Read Replicas for RDS
* Scaling out (more instances) vs scaling up (bigger resources)
* Always consider the cost element
* Know different S3 storage classes

#### Exam Tips

* Read all FAQ for load balances
