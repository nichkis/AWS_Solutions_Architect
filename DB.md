# Database Notes

#### RDS

* Relational Databases in AWS
  * SQL server
  * Oracle
  * MySQL
  * Postgres
  * MariaDB
  * Aurora
* Two Key Features
  1. Multi AZ for disaster recovery
  2. Read Replicas for performance
* Runs on virtual machines, cannot login to these operating systems
* Automated backups: retention period 1-35 days, stored in S3, enabled by default
* DB Snapshots: done manually, stored even after you delete instance
* Restoring automated backups and snapshots will be on a new RDS instance and new DNS endpoint
* Encryption at rest
* Read Replicas: used for scaling, automatic backups must be turned on to deploy read replica, up to 5, there can be read replicas of read replicas, can have in a second region, they can be Multi AZ

#### Online Transaction Processing vs Online Analytics Processing

* OLTP–think query row of data (websites)
* OLAP-think joins and aggregations of databases (data warehousing)

#### DynamoDB

* Stored on SSD
* Spread across three geographically distinct data centers
* Eventual consistent reads
* Strongly consistent reads (read within one second from update/insert)

#### Redshift

* Fast, powerful, fully managed, petabyte scale data warehousing service
* OLAP
* Uses different type of architecture from infrastructure layer and database perspective
* Single Node (160 GB), Multi Node: leader node, compute node (up to 128)
* Advanced compression (columnar), no indexes or materialized views, samples data to choose best compression scheme
* Massively Parallel Processing e.g. distributes data
* Backups (default 1 day, max 35 days)
* Encrypted in transit SSL, Encrypted at rest using AES-256
* No Multi AZ functionality
* Used for business intelligence
* Maintains at least 3 copies of data (original and replica on compute nodes, and a backup in S3)
* Asynchronously replicate snapshots to S3 in another region for disaster recovery

#### Aurora

* 2 copies of data in each AZ with minimum of 3 AZs
* Aurora serverless for simple, cost-effective option for infrequent, intermittent, or unpredictable workloads

#### ElastiCache

* In-memory cache on the cloud
* Redis and memcached
* Increase database and web application performance
