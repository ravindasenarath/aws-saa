# aws-saa

## Legend

- [IAM & AWS CLI](#iam-and-aws-cli)
- [IAM MFA](#iam-mfa)
- [EC2 Fundermentals](#ec2-fundermentals)
- [EC2 Solutions Architect Level](#ec2-solutions-architect-level)
- [EC2 Instance Storage](#ec2-instance-storage)
- [High Availability and Scalability](#high-availability-and-scalability)
- [AWS Fundermentals ( RSA + Aurora + ElastiCache )](#aws-fundermentals--rsa--aurora--elasticache-)
- [Route 53](#route-53)
- [S3](#s3)
- [Advanced S3](#advanced-s3)
- [S3 Security](#s3-security)
- [CloudFront and Global Accelerator](#cloudfront-and-global-accelerator)
- [AWS Storage extras](#aws-storage-extras)
- [Decouping applications: SQS,SNS,Kinesis,ActiveMQ](#decouping-applications-sqssnskinesisactivemq)
- [Containers on AWS(ECS, Fargate, ECR and EKS)](#)
- [Serverless](#serverless)
- [Databases in AWS](#aws-saa#databases-in-aws)
- [Data & Analytics](#aws-saa#data--analytics)

## IAM And AWS CLI

 - Identity and Access Management
 - Global service
 
### Users

 - Create users and assigne them to groups
 - Root user shouldn't be used or shared
 - Users can be grouped
 - Groups only contain users, not other groups
 - Users don't have to belong to a group and user can belong to multiple groups.
 
### Permissions

 - Users or groups and be assigned with policies
 - Policies define permissions for users
 - Apply least privilage principle when providing permissions
 
### IAM Policy Inheritance

 - Users belong to muliple groups inherite permissions from multiple policies
 - User can have inline policies
 
### IAM Policy Structure

 - Version
 - Id - Identifier for the policy ( Optional )
 - Statements
   - Sid - Identifier for the statement ( Optional )
   - Effect - If statement allows or denies access ( Allow / Deny )
   - Principle - Account/user/role to which this policy applied to
   - Action - List of api calls allowed or denied
   - Resource - List of resources to which actions applied to
   - Condition - Conditions for policy to be in effect ( Optional )
  
## IAM MFA

### IAM Password policy

 - Strong passwords
 - Password policy
   - Minimum length
   - Character types
   - Allow users to change password
   - Password expiration
   - Prevent password reuse
  
### Multi Factor Authentication ( MFA )

 - MFA = Password + device user own
 
### How to access AWS

 - AWS management console ( password + MFA )
 - AWS CLI ( access keys )
 - AWS SDK ( access keys )
 
### IAM roles

 - For services to take actions on behalf of users
 - Give permission for AWS services via IAM roles
 
### IAM Security tools

 - IAM Credentials report( account level ) : List all users and their credential status
 - IAM Access Advisor ( User level ) : Permissions granted to user user and when they are last used

## EC2 Fundermentals

 - Elastic Compute Cloud
 - Mainly consist in the capability of
   - Renting virtual machines ( EC2 )
   - Store data in virtual devices ( EBS )
   - Distribute load across machines ( ELB )
   - Scaling service ( ASG )

### EC2 sizing and configuration options

 - OS ( Linux, Windows, Mac OS )
 - CPU
 - RAM
 - Storage
   - Netowork attached ( EBS, EFS )
   - Hardware ( EC2 Instance store )
 - Network card
 - Firewall rules
 - Bootstrap script

### EC2 User data
 - Launching commands when machine starts
 - Only run once at the instance first start
 - Used to automate boot tasks
   - Install updates
   - Install software
   - Download common files
   - User data script run with root user

### EC2 Instance types
 - General purpose
   - Great for diversity of workloads
   - Balance between Compute, Memory and Networking
 - Compute Optimized
   - For compute intensive tasks that need high performance computing
 - Memory Optimized
   - Fast performance for processing large amount of data
 - Accelerated computing
 - Storage Optimized
   - Storage intensive tasks

### Security groups

 - Fundermental network security of AWS
 - Control how traffic is allowed into EC2
 - Only contain **allow** rules
 - Rules can be reference by ip or by security group

### Security groups good to know

 - Can attach to multiple instances
 - Locks down to region/VPC combination
 - Live outside of EC2
 - Good to maintain seperate security group for SSH
 - If application is not accessible(timesout) most probably SG issue
 - All inbound traffic is blocked by default
 - All outbound traffic is allowed by default

## EC2 Solutions Architect Level

### Elastic IPs

 - When EC2 is Stop/Start public ip will be changed
 - Elastic IP is a fix ip
 - Only 5 Elastic Ips per account
 - Try to avoid

### Placement groups
 - To have control over how EC2 placed on AWS infrastructure
 - Strategies
   - Cluster : law latency group in a single AZ **High performance**
   - Spread  : Spread across underlying hardware ( max 7 per AZ ) **High Available**
   - Partition : INstances spread across partitions within AZ **Hadoop/Cassendra/Kafka**

### Elastic Network Interfaces ( ENI )

 - Logical component in a VPC
 - Virtual Network Card
 - Can create on the fly and attach
 - Bound to AZ

### EC2 Hibernate

 - When EC2 
   - Stop : Data on EBS is kept
   - Terminate : EBS volues ( root ) is destroyed

 - On EC2 Start
   - OS Boot , User scripts run
   - Application starts, caches get warmed

 - Hibernate
   - RAM is preserved
   - Instance boots faster
   - RAM is written to the EBS
   - EBS should be encrypted
  
## EC2 Instance Storage
  
### What is an EBS volume
  
  - Elastic Block Storage is a network dirve that can be attached to instances while they running
  - Persist instance data even after termination
  - Bound to AZ
  - Analogy : Netowrk USB stick
  - By default when instance terminate only root EBS get deleted
   
### EBS Snapshots
 
  - Backups of EBS
  - Use to transfer EBS to another AZ
  - EBS snapshot archive: 75% cheap, take 24-72h to restore
  - Recycle bin of EBS: Can recover accidental delted EBS
  - Fast snapshot restore: Cost $$$
  
### AMI ( Amazon Machine Image )

 - Customizations of an EC2 instance
   - Software, configurations, OS, monitoring
   - Faster boot time
 - Build for a specific region ( can be copied to other regions )
 - Can launch AMI from 
   - Public AMI
   - Private AMI
   - Marketplace
  
### EC2 Instance Store

 - When need high performance hardware disk
 - If instance is stopped loose data
 - Eg buffer, cache
 
### EBS Volume Types

 - GP2/GP3 : General purpose SSD
 - io1/io2 : High performance SSD
 - st1 : Low cost HDD for throughput intensive work
 - sc1 : Lowest cost HDD for infrequently access data
 
### EBS Multi attach

 - Attach same EBS intance to multiple EC2 instances in same AZ
 - Only io2 family
 - Higher application availablity
 - Upto 16 EC2 instances at a time
 
### Amazon EFS ( Elastic File System )

 - Network file system
 - Multi AZ
 - Highly available, scalable, Expensive
 - Compatible with linux based AMI
 - Scalable, pay as use
 - Storage classes
   - Scale mode
   - Performance mode
   - Throughput mode
 - Storage tiers
   - Standard
   - EFS/IA ( Infrequently access )
 - Availability & Durability
   - Standard : Multi AZ ( Great for prod )
   - One Zone : One AZ ( Great for dev )

## High Availability and Scalability

### Elastic load balancing ( ELB )

 - Forward traffic to multiple servers
 - Why use load balancer
   - Spread load
   - Expose single point of access ( DNS )
   - Handle failures seamlessly
   - Provide SSL
   - Enfornce stickiness with cookies
   - High availability
   - Seperate public traffic from private traffic
 - Types of load balancers
   - Classic Load Balancer(CLB): HTTP/HTTPS/TCP/SSL **Deprecated**
   - Application Load Balancer(ALB): HTTP/HTTPS/Web socket
   - Network Load Balancer(NLB): TPC/TLS/UDP
   - Gateway Load Balancer: Operates at layer 3

### ALB

 - Layer 7 ( HTTP )
 - Load balancing across multiple http applications across machines ( target groups )
 - Support Http2/Websocket
 - Support redirects ( http to https )
 - Routing tables to different target groups
   - Based on path: example.com/users & example.com/posts
   - Based on hostname: users.example.com & posts.example.come
   - Based on query strings and headers: example.com?id=123&type=user
 - Great fit for micro services and container based applications ( Docker, ECS )
 - Has port mapping feature to redirect to dynamic ports of ECS
 - ALB target groups
   - EC2 instances ( HTTP )
   - ECS tasks ( HTTP )
   - Lambda functions ( HTTP request translated to JSON event )
   - IP addresses
 - Fixed host name
 - Clients don't see ip'x of applications directly
   - ip : `x-forwarded-for`
   - port : `x-forwarded-port`
   - protocol : `x-forwarded-proto`

### Network Load Balancer

 - Layer 4 ( TCP, UDP )
 - High performance ( millions of request per sec )
 - Has one static IP per AZ ( Helpful whitelisting ips )

### Gateway Load Balancer

 - Deploy manage and scale a fleet of 3rd party virtual appliances in AWS ( Eg firewall, Intrusion prevention and detection system etc )

### ELB ( Sticky Sessions/ Session affinity )

 - Same client always redirect to same instance behind load balancer
 - CLB, ALB and NLB support this
 - Cookie names
   - Application base cookies
     - Custom cookie
     - Generated by the target
     - Can include custom attributes
     - Cookie name must be individually specified for teach target group
     - **AWSALB,AWSALBAPP,AWSALBTG**  names reserved for the ALB
   - Application cookie
     - Generated by the LB
     - Name is **AWSALBAPP**
   - Duration base cookies
     - Cookie generated by the LB
     - Cookie name is **AWSALB** for ALB and **AWSELB** for CLB

### ELB: Cross zone load balancing

 - With cross zone LB: Each load balancer instance distributes evently across all registered instances
 - Without cross zone LB: Requests are distributed in the instances of the node of the ELB
 - ALB
   - Enabled by default ( Can disable at target group level)
   - No charge for cross AZ
 - NLB
   - Disabled by default
   - Charged if enabled
 - CLB 
   - Disabled by default
   - No charge if enabled

### ELB: SSL/TLS certificates

  - SSL Certificate encrypt traffic betwen client and load balancer. ( in flight encryption)
  - SSL(Secure Socker Layer) use to encrypt connections
  - TLS(Transport Layer Security) is the newer version
  - SSL Certificate has a expiry date
  - LB manage certificates using ACM(AWS Certificate Manger)
  - HTTPS listner
    - Must specify a default certificate
    - Can add optional certificates for multiple domains
    - Client can use SNI(Server Name Indication) to specify hostname they reach
  - SNI
    - Solves the problem of loading **multiple certificates** in to one server
    - Client indicate the hostname to initiate SSL handshake
    - Only works for ALB, NSB and Cloudfront

### ELB: Connection draining

  - Connection draning(CLB), Deregistration delay(ALB/NLB)
  - Time to complete in-flight request while the instance is deregistering or unhealthy
  - No requests sent while instance is deregistering

### Auto scaling group(ALG)

  - Scale out/in to match demand
  - Ensure have minimum/maximum instances running
  - ALG attributes
    - Launch template
      - AMI + Instance type
      - EC2 User date
      - EBS Volumes
      - Security Groups
      - SSH Key pair
      - IAM roles for ec2 instances
      - Network + Subnet info
      - Load balancer info
  - Can scale based on cloud watch alarms

### ALG: Scaling policies
  - Target tracking scaling
    - Align to a default baseine
    - Eg: Keep average ASG CPU usage around 40%
  - Simple/Step scaling: 
    - When CloudWatch alarm triggers(CPU > 70%) add two units
    - When CloudWatch alarm triggers(CPU < 30%>) remove one units
  - Schedule actions
    - Based on known actions
    - Eg: Scale capacity on Friday
  - Predictive scaling
    - Forecast load and schedule ahead
  - After a scaling activity there is a cool down priod ( default 300 sec)

## AWS Fundermentals ( RSA + Aurora + ElastiCache )

### Amazon RDS overview

  - RDS ( Relational Database Service )
  - Manged database service for DB use SQL as query language
  - Supported databases
    - Postgres
    - MySQL
    - MariaDB
    - Oracle
    - MS SQL Server
    - Aurora
  - Provided fetures
    - Automated provitioning, OS patching
    - Continues backups and restore to specific timestamp
    - Monitoring dashboards
    - Read replicas for improved read performance
    - Muti AZ for DR ( Disaster recovery )
    - Scaling capability
    - Storage backup by EBS(gp2 or io1)
    - When free db storage running out it scales automatically ( need to set maximum storage threshold )
    - Automatically scale storage if
      - Free space less than 10%
      - Low storage last at least 5min
      - 6 hours passed since last modification

### RDS Read replicas vs Multi AZ

  - Read Replicas
    -  Read replicas - read scalability
    -  Upto 15 Read Replicas
    -  Witin AZ, Cross AZ or Cross region
    -  Replication is ASYNC
    -  For RDS read replicas with in same AZ network cost is free
  - Multi AZ
    - SYNC replication
    - One DNS automatic failover
    - Not used for scaling 

  - **Can set up read replicas as multi AZ for DR**
  - Single AZ to multi AZ zero down time operation
   
### RDS Custom

  - For oracle and MS SQL Server
  - Has access to underlying db and OS
  - De-activate automation mode to perform customization

### Amazon Aurora

  - Proprietary technology
  - Compatible with Postgres and MySQL
  - 5x performance of MySQL on RDS or 3x of Postgres
  - Grows upto 10GB upto 128 TB
  - Upto 15 replicas ( MySQL only 5)
  -  Instantaneous failover
  - Cost 20% more
  - 6 copies of data across 3 AZ
  - Self healing
  - One master ( take writes ) + Upto 15 Read replicas
  - Writer endpoint and Reader endpoint

### Aurora advance concepts

  - Replica auto scaling ( scale when high reads )
  - Custom endpoints ( Bigger replicas for high demand use )
  - Aurora Serverless
    - Good for infrequent unpredictable workloads
    - No capasity planning needed
    - Pay per second
  - Aurora Multi Master
    - Failover for writer node
  - Global Aurora
    - For disaster recovery
    - **Take less than 1 second to cross region replication**
  - Aurora Machine Learning
    - Enable ML based predictions to your applications via SQL
    - Support SageMaker, Comprehend
    - Fraud detection, ads targeting, product recommendation

### RDS & Aurora - Backup and Monitoring

  - Automated backups
    - Daily full backups
    - Ability to restore to any point in time(Oldest 5 mins ago)
  - Manual DB Snapshots
  - If used for shot time take a snapshot delete database to save money
  - Aurora backups
    - 1-35 days(cannot be disabled)
  - RDS & Aurora Restore options
    - Creates a new database
    - Restore MySQL RDS from S3 : Backup > Store in S3 > Restore to RDS
    - Restore MySQL Aurora cluster from S3 : back up using Percona Xtrabackup > Store in S3 > Restore
  - Aurora DB cloning
    - Create new from existing
    - Useful to create staging db from prod db

### RDS & Aurora Security

  - At-rest encryption ( Master and replica encrypted using KMS )
  - In flight encryption
  - IAM Authentication
  - Security groups
  - Audit logs can be enabled 

### Amazon RDS Proxy

  - Fully managed proxy for RDS
  - Pool and share connections
  - If have lots of connection to reduce stressing on db 
  - Serverless, autoscaling, high available ( Multi AZ)
  - Enforce IAM authentication
  - Not publicly available ( via VPC )
  - Used for lambda functions

### ElastiCache

  - Managed Redis or Memcached
  - Cache ( In memory for high performance, low latency)
  - Need heavy application changes
  - Session store
  - Redis
    - Multi AZ with auto failover
    - Read replicas to high availability
    - Bakup and restore features
  - Memcache
    - Multi Node
    - No high availability
    - Non persistent
    - No backup and restore

### Elasticache Security

  - IAM for Redis
  - Redis Auth ( extra security )

### ElastiCache - Redis use case
  - Gaming leaderboards


## Route 53

### DNS

  - Domain registrar: Amazon Route 53, GoDaddy
  - Name server: Resolve dns queries
  - DNS records: A, AAAA, CNAME, NS
  - Top level domain(TLD): .com, .us, .gov
  - Second level domain(SLD): amazon.com, google.com

### Route 53 overview

  - Highly available, scalable, fully managed and Authoritative DNS(client can update DNS)
  - Can check health of resources
  - Records
    - Domain/subdomain: eg example.com
    - Record type: A or AAAA
    - Value: ip
    - Routing policy: How to respond
    - TTL: Time the record is cached
  - Supported DNS record types
    - A/AAAA/CNAME/NS
    - CAA/DS/MS etc
  - Route 53 Record types
    - A - maps a hostname to IPv4
    - AAAA - maps a hostname to IPv6
    - CNAME - maps a hostname to another hostname
      - Target domain name must have an A or AAAA record
      - Can't create for top node of a DNS namespace ( Zone Apex )
    - NS - Name servers for the hosted zone
  - Hosted Zones (A container for records that define how to route traffic to a domain and subdomains)
    - Public Hosted Zones: How to route traffic on the internet ( eg myApp.mypublicdomain.com)
    - Private Hosted Zones: How to route traffic within one or more VPCs ( eg app1.company.internal)

### TTL(Time to live)

  - Ask client to cache IP for certain time
  - Mandatory for all records expect Alias

### CNAME vs Alias

  - CNAME
    - Point hostname to any other hostname(myapp.mydomain.com => app.other.com)
    - **Only for non root domain**
  - Alias
    -  Points a hostname to an aws resource ( myapp.mydomain.com => abcd.amazonaws.com )
    - **Works for root and non root domains**
    - Free of charge
    - Native health check
    - Alias records is always of type A/AAAA for aws resources(IPv4/IPv6)
    - Can't set TTL
    - ELB, CloudFront, API Gateway, Elastic Beanstalk environments, S3 Websites, VPC Interface endpoints, Global Accelerator accelerator, Route 53 record in the same hosted zone 
    - Can't set alias to EC2 dns name

### Routing policy - Simple

  - Route traffic to a single resource
  - Can specify multiple values(client choose one random)
  - Can't associate with health checks

### Routing policy - Weighted

  - Control % of requests go to each specific resource
  - Assign each record a relative weight
  - % = weight of specific record/ sum of all weights
  - Doesn't sum upto 100
  - Can assiciate with health checks
  - Use for load balancing, Testing new versions
  - Assing 0 not to send traffic to one

### Routing policy - Latency

  - Redirect to the resource that has the latest latency to the client
  - Based on traffic between users and AWS regions
  - Can associate with health checks

### Routing policy - Health checks

  - Check health of public resources
  - Types
    - Monitor an endpoing
    - Monitor other health checks
    - Monitor Cloudwatch alarms

  - Calculated Health Checked
    - Combine the result of multiple health checks
    - Can use conditions
    - Upto 256 child health checks
    - Usage: Perform maintenance without causing all health checks to fail
  - Health CHecks - Private Hosted Zone
    - Create a CloudWatch Metric and associate a CloudWatch Alarm

### Routing policy - Failover ( Active-Passive )

  - If health health check failed redirect to secondary record

### Routing policy - Geolocation

  - Routing based on user location
  - Use cases: Localization, Restrict content

### Routing policy - Geoproximity

  - Based on geographic location of users and resources
  - Ability to shift more traffic to the resources based on a defined bias

### Routing policy - Ip-based routing

  - Based on clients IP addresses
  - Provide a list of CIDRs for clients
  - Use cases: Optimize performance, reduce network cost

### Routing policy - Multi - Values

  - Routing traffic to multiple resources
  - Can associate with health checks
  - Up to 8 healthy records
  - **Not a substitude for ELB**

### 3rd Party Domains vs Route 53

  - Create a public hosted zone
  - Update NS records on 3rd party website to use Route 53 NS.
  - Copy name servers from 3rd party dns registrar

## S3

### Overview

  - Infinitely scaling storage
  - Use casesa
    - Used for backup & storage
    - Disaster recovery
    - Archive
    - Host application, media
    - Data lakes, big data analytics
    - Static websites
  - Buckets
    - Store objects(files) in **buckets**(directories)
    - Buckets must have a globally unique name
    - Buckets are defined in globak level
    - Naming convention
      - No uppercase, No underscore
      - 3 - 63 long
      - Not ips
  - Objects
    - Objects have a key
    - Key is the full path (eg: s3://my-bucket/my_folder/my_file.txt)
    - Values are content of body
      - Max 5TB
      - More than 5GB must use multi part upload

### S3 - Security

  - Bucket policy
    - User based: IAM policies
    - Resource based
      - Bucket policies - cross account
      - Object Access Control List(ACL)
      - Bucket Access COntrol List(ACL)
  - IAM principle can access and S3 object if
    - User IAM permission ALLOW it OR the resource policy ALLOWS it AND there is no explicit DENY

  - Encryption

### S3 - Bucket policies

  - Json policy
    - Resource: Buckets and Objects
    - Effect: Allow or Deny
    - Actions: Set of API to allow ro deny
    - Principle: The account or user to apply the policy to

  - Use cases
    - Grant public access
    - Force objects to be encrypted at upload
    - Grand access to another account

### S3 - Static Website Hosting

  - If **403 Forbidden** check bucket policy for public access

### S3 - Versioning

  - Best practice against unintended deltes
  - Files not versioned prior to enabling versioning will have version **null**

### S3 - Replication

  - Must enable versioning in source and destination
  - Cross Region Replication(CRR)
  - Same Region Replication(SRR)
  - Use Cases
    - CRR - Complience, lower latency
    - SRR - Log aggregation, live replication
  -  After enabling only new objects are replicated, to replicate existing object use **S3 Batch Replication**
  - No chaning replication

### S3 - Storage classes

  - Standard - General Purpose
    - 99.99% Available
    - Used for frequently accessed data
    - Low latency and high throughput
    - Use cases: Big data, mobile & gaming apps, content distribution
  - Standard - Infrequent Access (IA)
    - For data less frequently accessed
    - Lower cost
    - 99.9% avaialable
    - Use Cases: Disaster Recovery, backups
  - One Zone - Infrequent Access
    - High durability in single AZ
    - 99.5% Available
    - Store secondary copies or recreatable
  - Glacier Instant Retrieval
    - Min storage 90 days
  - Glacier Flexible Retrieval
    - Expedited(1-5min), Standard(3-5 hr), Bulk(5-12hr - free)
    - Min storage 90 days
  - Glacier Deep Archive
    - Long term storage
    - Standard(12 hr), Bulk(48hr)
    - Min storage duration 180days
  - Intelligent Tiering
    - Move between tiers based on usage

## Advanced S3

### Lifecycle rules

  - Transition actions
    - Configure objects to transition into another storage class
    - eg: Move to Standard IA after 30 days from creating

  - Expiration actions
    - Configure objects to expire(delete) after some time
    - Delete incomplete multi part uploads

  - S3 Analytics - Storage class analysis
    - Help to decide when to transition objects into right storage class.
    - Recommended for Standard and Standard IA

### S3 Requester Pays

  - Requester instead of bucket owner pays for the request

### S3 Event notifications

  - Events: Object created, Object removed etc
  - React to events and notify then to SNS, SQS and lambda functions
  - Has integration with Amazon event bridge

### S3 Performance

  - Scale into high request rates and has low latency
  - At least 3500 PUT/COPY/POST/DELETE or 5500 GET/HEAD requests per second per prefix
  - Improve performance
    - Multi part upload
    - S3 Transfer Acceleration - Increase transfer speed by using edge locations
    - S3 byte range fetches - Parallelize GETs by reqeusting specific byte ranges

### S3 Select & Glacier Select

  - Use SQL to perform server side filtering

### S3 Batch operation

  - Perform bulk operations on exiting S3 objects using a single request
  - Examples
    - Change attributes, metadata
    - Encrypt unencrypted objects
  - Can use S2 to get list of objects to perform operation on

## S3 Security

### S3 Encryption

  - SSE(Server side encryption) 
    - Server-side encryption with Amazon S3 managed keys(SSE-S3) - Enabled by default
      - Keys handled, managed and owned by AWS
      - Encryption type **AES256**
      - Must set header to **x-amz-server-side-encryption: AES256**
    - Server-side encryption with KMS keys stored in AWS KMS(SSE-KMS)
      - Advance control with KMS (audit key usage)
      - Must set header to **x-amz-server-side-encryption: aws:kms**
    - Server-side encryption with customer provided keys(SSE-C)
      - HTTPS must be used
  - Client side encryption

  - Encryption in transit(SSL/TLS)
    - has two endpoints(http/https)

### S3 Default Encryption

  - SSE-S3 automatically applies to new objects
  - Can force encryption using bucket policy( Refuse PUT without proper headers )

### S3 CORS

  - Need to be enabled

### MFA delete

  - Force generated code when permenantly deleting a object
  - Need to enable versioning first
  - Only bucket owner can enable/disable MFA delete

### S3 Access logs

  - Log all access to S3
  - Can be anlyzed using Athena
  - Target bucket should be in same region

### Pre-signed urls
  - User given a pre-signed url inherit the permissions of the user that granted the url for GET/PUT

### Glacier vault lock & S3 Object Lock

  - Glacier vault lock
    - Adopt(Write once Read Many) model
    - Create a vault lock policy
    - Lock the policy for future edits(Can't changed or deleted)
  - S3 Object lock
    - Need vesioning
    - Object level block
    - Retention mode: Complience
      - Object version cannot be overritten or deleted by any user
      - Retention cannot be changed or shortened
    - Retention mode: Governance
      - Some users have special permission to change
  - Legal hold
    - Protect object indefinitly
    - Can remove with the *s3:PutObjectLegalHold* permission

### S3 Access Points

  - Simplify security management
  - Access points for different data
  - Each has own dns name(Internet origin or VPC origin)
  - Grant R/W permission to prefix via a policy
  - Bucket policy become simple
  - VPC origin - access via VPC(Need a VPC endpoint and it's policy must allow access)

### S3 Lambda

  - Need S2 access points
  - Alter objects before provided

## CloudFront and Global Accelerator

### Overview

  - Content Delivary Network(CDN)
  - Improve read performance, content is cached at the edge
  - 216 points globally
  - Get DDoS protection, integrated with Shield and Web applicaiton firewall
  - Origins
    - S3 bucket
      - Enhance security with OAC(Origin Access Control)
      - Can used as an ingress(upload to S3)
    - Custom endpoint
      - ALB
      - EC2
      - S3 website
      - Any http backend

### CloudFront - ALB as an origin

  - Direct to EC2: Should allow IP's of edge location to access
  - Via ALB

### Geo restriction

  - Can restrict base on country ( allow/ blocked)

### Price classes

  - Cost differs based on edge location
  - Price classes
    - All: Best performance
    - 200: Most regions, exclude most expensive regions
    - 100: Only least expensive regions

### Cache Invalidation

  - Has a TTL
  - Can force and entire or partial cache refresh
  - All files or part of files

### Global Accelerator

  - Unicase and Anycast IP ( Anycase multiple servers hold same id and client is routed to the closest one)
  - GA use anycast
  - Leverage AWS internal network
  - Via Edge location to anycast ip
  - Works with Elastic IP, EC2, ALB, NLB(public or private)
  - Have health checks
  - Only need to whitelist 2 ips, DDos, Shield protection

### Global accelerator vs CloudFront

  - CloundFront
    - Improve performance for cacheable content
    - Improve performance for dynamic content
    - Content is served at edge

  - GA
    - Improve performance of wide range of apps over TCP  or UDP
    - Proxy packets form Edge to application
    - Suits for non http 
    - Good for http that need static ip

## AWS Storage Extras

### Snow family

  - Highly secure portable devices to collect and process data at the edge and migrate data in and out of AWS
  - Data migratio
    - Snowcone
    - Snowball Edge
    - Snow Mobile
  - Edge computing
    - Snowcone
    - Snowball Edge

  - Snowball Edge
    - Storage optimized: 80TB
    - Compute optimized: 42TB/28TB
  - Snowcone: Small, Portable, Light(2.1kg)
    - Snowcone(8TB)
    - Snowcone SSD(14TB)
  - Can sent back offline or use **AWS Data Sync**

  - Snow mobile
    - A truck
    - For more than 10PB of data
  - AWS OpsHub - Software to work with snow family devices

### Snowball into Glacier

  - Cannot directly import to Glacier
  - Import to S3 and use lifecycle policy import into Glacier

### Amazon FSX

  - Launch 3rd party high performances on AWS
  - FSx for Lustre
    - Parellel distributed file system for large scale computing
    - Machine lerning, High Performance Computing
    - Seamless integration with S3
    - File systems
      - Scratch file system(Temporary, data not replicated, high burst)
    - Persistent File System
      - Long term
      - Data replicated multi AZ 
  - FSx for NetApp ONTAP
    - Point in time instantaneous cloning
  - FSx for Windows file server
    - Can mount on EC2
    - Support MS Distributed file system Namespaces
    - Can configured to be Multi AZ
  - FSx for OpenZFS
    - Point in time instantaneous cloning

### Storage Gateway

  - Bridge between onpermises and AWS cloud data
  - Use cases: Disaster Recovery, Migration, Cache low latency file access
  - Types
    - S3 Gateway
      - Most recently used files are cached at gateway
    - FSx Gateway
      - Access to FSx for Windows file server
      - Local cache for frequently accessed data
    - Voulme Gateway
      - Cached Volumes
      - Stored Volumes
    - Tape Gateway

### Transfer Family

  - Fully managed service for file transfers in/out of S3/Amazon EFS using FTP
  - Supported Protocal
    - AWS Transfer for FTP
    - AWS Transfer for FTPS
    - AWS Transfer for SFTP

### Data Sync

  - Move large amount of data in/out AWS
  - Can sync to S3/FSx/EFS

## Decouping applications: SQS,SNS,Kinesis,ActiveMQ

### SQS

  - Simple Queue Service
  - SQS - Standard Queue
    - Unlimited throughput, Unlimited messages in queue
    - Limit 256KB per message
    - Can have duplicate messages
    - Can have out of order messages
  - Can integrate with ASG for automatic scalling
  - Secuirty
    - Inflight (HTTPS)
    - At rest using KMS keys
    - Access control with IAM policies
    - SQS Access policies
### Message visibility timeout

  - When message is polled it become invisible for a configurable time to other consumes
  - Default message visibility timeout is **30 Sec**
  - If processing takes more time use **ChangeMessageVisibility** api to get more time

### Long polling

  - When a consumer request message from queue and if there are no messages consumer can wait for a given time. 
  - Decrease number of API calls to SQS while increasing efficiency of the applicatoin.

### SQS -FIFO Queue

  - Ordering of the messages in the queue is gurenteed
  - Limited throughput

### SNS
  - Simple Notification Service
  - Pub/Sub method
  - Have the concept of topic (100k topics limit)
  - Subscribers
    - Email
    - SMS and mobile notification
    - Http(s) endpoints
    - SQS
    - Lambda
    - Kinesis Data Firehose
  - Publish
    - Topic Publish ( to the topic )
    - Direct Publish ( to the endpoint )
  - Secuirty (Same as SQS)

### SNS + SQS (Fan out pattern)

  - Push once into a SNS topic and subscribe many SQS
  - Cross region delivery
  - FIFO topic has similar features as SQS FIFO
  - Can only have SQS FIFO queues as subscribers
  - Limited throughtput
  - Message filtering - Filter messages sent to SNS

### Kinesis

  - Makes it easy to collect, process and analyze stream data
  - Services
    - Kinesis Data Streams: capture, process and store data streams
    - Kinesis Data Firehose: Load data streams into AWS data stores
    - Kinesis Data Analytics: Analyze the data streams using SQL or Apache Flink
    - Kinesis Video Streams: Capture process and store video streams

### Kinesis Data Streams
  - Need to provision shards
  - Input Record ( key, blob[up to 1MB])
  - Output Record ( Partition Key, Sequence No, Data blob)
  - Consumers: Apps, Lambda, Kinesis Data firehose, Kinesis Data Analytics
  - Modes
    - Provisioned mode
    - On-demand mode
  - Security
    - IM policies
    - In flight Https
    - At rest uisng KMS
    - VPC endpoints are available

### Kinesis Data Firehose

  - Sources: Apps, Clients, SDK/KPL, Kinesis Agent, Kenesis Data Streams, Amazon CloudWatch, AWS IoT
  - Optionally transform using Lambda
  - Written in batches to destinations
  - Destinations
    - AWS: S3, Redshift(via S3), OpenSearch
    - 3rd Party: Datadog, Mongo, Splunk,
    - HTTP endpoint
  - Send all or failed data to S3
  - Near Real time 
  - No replay

### Kinesis Data Analytics
  - In analytics section

### Amazon MQ

  - Migrate form non propritary service
  - Managed message broker for RabbitMQ and ActiveMQ
  - For High availability (multi AZ) need EFS

## Containers on AWS(ECS, Fargate, ECR and EKS)

### ECS

  - Elastic Container Service
  - Launch Docker containers on AWS = Launch ECS Tasks on ECS clusters
  - EC2 Launch type : Must provision & maintain infrastructure(EC2)
    - Each EC2 instance run ECS Agent
    - EC2 Instance profile
      - Used by ECS agent
      - Makes API calls to ECS service
      - Send container logs to CloudWatch
      - Pull docker images from ECR
      - Refer to secrets manager
    - ECS Task Role
      - Allow each task to have a specific role
  - Fargate Launch Type: Serverless
    - Need to craete task definition
  - Data Volumes(EFS)
    - Mount EFS onto ECS tasks to share data between tasks

### ECS Auto Scaling

  - AWS Application Auto Scaling
    - CPU utilization
    - Memory utilization
    - Request count per target
  - Target Tracking - based on target value
  - Step Scaling -  based on CloudWatch alarm
  - Scheduled Scaling - based on specified date/time
  - Auto Scaling EC2 Instances
    - Auto Scaling Group Scaling
      - Scale ASG based on CPU
      - Add EC2 Instances
    - ECS Cluster Capacity Provider
      - Automatically provision and scale

### ECR (Elastic Container Registry)

  - Store and manage docker images
  - Backed by S3
  - Access controlled by IAM

### EKS (Elastic Kubernates Service)

  - Managed Kubernates Cluster
  - Support ECS and Fargate launch mode
  - Node Types
    - Managed Node groups
      - Use EC2 but manged it self
    - Self Managed Nodes
      - Use EC3 but need to manage manually
    - Fargate
      - No management needed

### AWS App  Runner
  - Fully managed service for web applications and APIs
  - Use cases: web apps, APIs, Micro services, RAD

## Serverless

  - Do not manage servers

### Lambda functions

  - Virtual functions
  - Limited by time - short execution
  - Run on demand
  - Scaling is automated
  - Upto 10GB of RAM (Improve CPU and Network)
  - Integrations
    - API Gateway
    - Kinesis
    - DynamoDB
    - S3
    - CloudFront
    - CloudWatch Events/Event Bridge
    - CloudWatch Logs
    - SNS/SQS
    - Cognito

### Limits(Per Region)
  - Execution
    - Memory allocation: 128MB - 10GB(1MD increments)
    - Maximum execution time: 900 seconds(15min)
    - Environment variables: 4KB
    - Disk capacity: 512MB to 10GB
    - Concurrent executions: 1000(can be increased)
  - Deployments
    - Function deployment size: 50MB
    - Uncompressed deployment size: 250MB

### Lambda@Edge & CloudFront functions

  - Edge functions
    - Code attach to CloudFront distributions
    - Run close to users to minimize latency

  - CloudFront Functions
    - Lightweight written in JS
    - Sub ms startup millions of request/second
    - Used to change viewer requests and responses
    - Use cases
      - Cache key normalization(Transform headers, cookies, query strings, URL)
      - Header Manipulation
      - URL rewrites
      - Request Authenticaiton/Authrization

  - Lambda@Edge
    - Lambda in NodeJS or Python
    - Scales to 1000s of request/second
    - Used to change CloudFront requests/responses
    - Use Cases
      - Longer tasks
      - Network access to external services
      - File access or access to html body

### Lambda in VPC
  - Lambda launched in AWS own VPC so cannot access resources in private VPC
  - Define VPC id and subnet and security groups

### DynamoDB
  - Fully manged, highly available, Multi AZ
  - NoSQL
  - Provisioned Capacity mode
    - Pre plan
  - On-Demand Mode
    - Automatic
    - More expense

### DynamoDB - Advanced Features
  - DynamoDB Accelerator(DAX)
    - Solve read congestion by caching
  - Stream Processing
    - Ordered stream of item-level notifications in a table
  - Use cases
    - React to changes in real time
    - Real time analytics
    - Insert into derivative tables
    - Cross region replication
    - Invoke Lambda on changes
  - DynamoDB Streams
    - 24 hr retention
    - Limited consumers
    - Process using Lambda or Kinesis adapter
  - Kinesis Data Stream
    - 1 yr retention
    - High no of consumers
    - Process using Lambda, Kinesis Data Analytics, Kinesis Data Firehose, Glue Streaming ETL
  - Global table
    - Table replicated across multiple tables
    - Law latency accessibility
    - Need to enable Dynamo db streams
  - TTL
    -  Automatically delete after expiry timestamp
  - Integration with S3
    - Can export to S3
    - Then query with Athena

### API Gateway

  - Create public REST API's
  - Proxy requests
  - Features
    - With Lambda: Fully serverless
    - API versioning
    - Handle environments
    - Handle security
    - API Keys, request throttling
    - Transform and validate req/res
    - Cache responses
  - Integrations
    - Lambda
    - HTTP
    - AWS Service
  - Endpoit Types
    - Edge Optimized
      - Global
      - Via CloudFront
    - Regional
    - Private
      - Only for VPC
  - Authentication
    - IAM roles
    - Cognito
    - Custom Authorizer 
  
### Step Functions

  - Build serverless visual workflow to orchestrate lambda functions
  - Can have human approval steps

### Cognito

  - GIve users and identity to interact with web/mobile apps
  - Cognito user pools
    -  Sign in functionality
    - Integrate with API Gateway and ALB
  - Cognito Identiy Pools(Federated Identity)
    - Provide AWS credentials to can access AWS resources directly
    - Integrate as identity provider

  - Serverless database of users for web/mobile
  - Simple login
  - Password reset
  - Email/Phone Number verification
  - MFA
  - Federated Identities

## Databases in AWS

### RDS
  - Managed PostgreSQL/MySQL/Oracle/SQL Server/MariaDB/Custom
  - Support Read replicas and Multi AZ

### Aurora
  - Compatible API for PostgreSQL/MySQL
  - Storage: Data in 6 replicas, across 3 AZ - Highly available, self healing, auto scaling
  - Custom endpoints for writer and reader
  - Serverless
  - Multi Master for continues failover
  - Aurora Global

### ElastiCache
  - Manged Redis/Memcached
  - Must provision EC2 Instance type
  - Redis suport clustering and MultiAZ, Read replicas
  - Code changes needed

### DynamoDB
  - Proprietary, managed serverless, NoSQL
  - Can replace Elasticache
  - DAX cluster

### S3
  - Key value store
  - Great for Big objects
  - Serverless

### DocumentDB
  - Aurora for MongoDB
  - Fully managed, highly available across 3 AZ

### Neptune
  - Fully managed graph database

### Keyspaces
  - Managed Apache Cassandra

### QLDB
  - Quantum Ledger Database
  - Ledger for recording financial transactions
  - Immutable system

### Timestream
  - Fully managed,fast, scalable, serverless time series database

## Data & Analytics

### Athena
  - Serverless query service for data in S3
  - Use SQL
  - Can use QuickSight to create report and dashboards
  - Improve performance
    - Use columnar data for cost saving(Parquet or ORC)
    - Use Glue to convert data
    - Compress data
    - Partition dataset in S3
    - Use larger files
  - Federated Query
    - Run queries in other services or on permises data sources

### Redshift
  - Database and analytics engine
  - Based on Postgres
  - OLAP - online analytical processing
  - Columnar storage of data
  - Pay as go
  - Can configure to automatically copy snapshot to another region
  - Redshift Spectrum
    - Query data in S3 without loading it

### OpenSearch
  - Successor of ElasticSearch
  - Can search any field, even partially matches
  - Opensearch dashboards for visualization

### EMR
  - Elastic Map Reduce
  - Create hadoop clusters(Big Data)
  - Made of hundreds of EC2 clusters
  - Use cases: data processing, machine learning, web indexing
  - Nodes
    - Masters: Long running
    - Core Node: Long running
    - Task Node : Can use spot

### QuickSight
  - Serverless machine learning powered business intelligence service to create interactive dashboards
  - Use cases: Business analytics, visualization
  - In memory computation using SPICE
  - Column level security

### Glue
  - Managed ETL service
  - Prepare and transform data for analytics
  - Glue data catalogue: catalogue of datasets
  - Glue job bookmarks: prevent data re process
  - Glue Elastic views: combine and replicated data scross multiple data stores using SQL
  - Glue DataBrew: Normalize data
  - Glue Studio: GUI for ETL
  - Glue Streaming ETL

### Lake Formation
  - Helps create data lakes
  - Data lake = central place to have all your data for analytics purpose
  - Discover, cleanse, transform and injest data into data lake
  - Out of the box blue prints
  - Fine grained access controls(row and columns)
  - Centralized permissions 

### Kinesis data analytics
  - For SQL
    - Sources: Kinesis Data Streams, Kinesis Data Firehose
    - Use SQL and use S3 to enrich data
    - Sinks: Kinesis Data Streams, Kinesis Data Firehose
  - For Apache Flink
    - Use Java, Scala, SQL

### MSK(Managed Streaming for Apache Kafka)
  - Fully managed apache kafka
  - Create, Update, Delete clusters
  - Deploy cluster on VPC, Multi AZ

### Big data ingestion pipeline
  - Fully sererless pipeline

## Machine Learning

### Rekognition
  - Find objects, people, text, scenes in **imaegs and videos**
  - **Facial analysis** and **facial search**
  - Use cases
    - Labeling
    - Content Moderation
    - Text detection
    - Face detection and analysis
    - Face search and verification
    - Celebrity Recognition
    - Pathing( for sports name analysis)

### Transcribe
  - Convert **speach to text**
  - Use deep learning process called automatic speech recognition
  - Automatically remove personally identifiable information(PII)
  - Supports Automatic Language Identification for multi-lingual audio
  - Use cases
    - Transcribe cutomer service calls
    - Automate closed captioning and subtitling

### Polly
  - Convert **Text into speech**
  - Use Lexicon to customize pronunciation of words with pronumciation lexicons
  - SSML(Speech Synthesis Markup Language)
    - Emphasizing
    - Breathing sounds, whispering


### Translate
  - Natural and accurate **language transalte**

### Lex + Connect
  - Amazon Lex(technology powers Alexa ) 
  - Amazon Connect
    - Receive calls, create contact flows, cloud based virtual contact center)
    - Can hook with CRM

### Comprehend
  - Natural Language Processing(NLP)
  - Find insights and relationships in text
    - Langulage
    - Extract key phrases, people etc
    - Understand positive or negative

### Comprehend Medical
  - Detects and return useful informaiton in clinical text

### SageMaker
  - Fully managed service to build ML models 

### Forecast
  - Fully managed service that use ML to deliver accurate forecasts
  - Use casesa: Product demand planning, financial planning

### Kendra
  - Document search service powered by ML
  - Extract answers from document
  - Natural language serach

### Personalize
  - ML service build apps with real time personalize recomendations
    - Integrates into websites, SMS, email

### Textract
  - Use to extract text from any scanned documents
  - Extract driver licence no, dob etc

## AWS Monitoring & Audit(CloudWatch, CloudTrail, Config)

### CloudWatch metrics
  - Metrics for every services in AWS
  - Metric is a variable to monitor(CPU, Network, RAM)
  - Metric belong to namespaces
  - Dimension is an attribute of a metric
  - Metric Streams(Near real time delivery to KDF or 3rd party)

### CloudWatch Logs
  - Store logs on AWS
  - Log groups: name, usually app name
  - Log stream: app instance
  - CloudWatch Logs can send logs to 
    - S3
    - KDS
    - KDF
    - Lambda
    - OpenSearch
  - Sources
    - SDK, Logs Agent, Unified Agent
    - Elastic beanstalk
    - ECS
    - Lambda
    - VPC flow
    - API Gateway
    - CloudTrail
    - Route 53
  - Metric filter and Insights
    - filter logs
  - Not near real time or real time
  - Subscriptions(Send logs to some where)
  - Log aggregation, multi account and multi region

### CloudWatch Agent & CloudWatch Logs Agent
  - Logs agent
    - Old version
    - Can only send to CloudWAtch Logs
  - CloudWAtch Unified Agent
    - Collect additional system level metrics
    - Can configure using SSM Parameter Store
    - Metrics
      - CPU
      - Disk metrics
      - RAM
      - Netstat
      - Processes
      - Swap space
      
### CloudWatch Alarm
  - Used to trigger notifications
  - Status
    - OK
    - INSUFFICIENT_DATE
    - ALARM
  - Targes
    - Actions on EC2
    - Trigger auto scaling action
    - Send notification to SNS
  - Composite alarms(combine multiple alarms)

### EventBridge
  - Schedule Cron jobs
  - Event Pattern: Event rules to react to a service doing something(IAM user login)
  - Sources > Filter > JSON > Destination
  - Types
    - Default Event Bus (AWS services)
    - Partner Event Bus (Partner services)
    - Custom Event Bus (Custom apps)
  - Can archive events
  - Replay archived events
  - Schema Registry
    - analyze the events in your bus and infer the schema
    - Tell application how data looks like
  - Resource based policy
    - Manage permissions
    - Aggregate events of organization

### CloudWatch Insights and Operational Visibility
  - Collect, aggregate, summerize metrics and logs from containers
  - Lambda Insights
  - Contributor insights
    - Analyze log data and create timeseries display contributor data
  - Lambda Insights
  - Contributor Insights
  - Application Insights

### CloudTrail
  - Provide **governance, compliance and audit** for your AWS account
  - History of events/API calls 
  - Kind of event
    - Management events
      - Operations that are performed on resources
      - Read events/Write events
    - Data events
      - Not logged by default
      - eg: Reas of S3
    - CloudTrail Insight Events
      - Detect unusual activities
      - Can integrate with EventBridge
  - Events are stored for 90 days

### AWS Config
  - Helps with auditing and recording compliance of resources
  - Eg: Is there unrestrictd SSh access to security groups, Do my buckets have public access
  - Per region can be aggregated cross regions
  - Config Rules(Does not prevent)
    - AWS managed rules
    - Custome rules
  - Remediations
    - Automate remediation of non-compliant resources using SSM automation documents
  - Notification
    - Using EventBridge

 ## IAM Advanced

### AWS Organizations
  - Allows to manage multiple AWS accounts
  - Management account and member accounts
  - Consolidated billing
  - Share reserved instances and savings plans discounts
  - Can define Service Control Policies to all memeber accounts

### IAM - Advanced Policies
  - IAM Conditions
    - aws:SourceIp : Restrict client IP from API calls are made
    - aws:RequestedRegion: Restricting region
    - ec2:ResourceTag: Restrict base on tags
    - ec2:MultiFactorAuthPresent: Force MFA
    - aws:PrincipleOrgID: Rescrict to organization

 ### IAM Roles vs Resource based policies
  - When you assume a role, you give up your original permissions and take the permissions assigned to the role.
  - When using resource based policy , doesn't have to give up permissions

### IAM - Policy Evaluation Logic
  - Permission boundaries
    - Supported for users and roels (not groups)
    - Advanced feature to use a manged policy to set maximum permissions an IAM can get

### IAM - Identify Center
  - SSO for all AWS accounts

### AWS Directory Services
  - Way to create Active directory in AWS
  - AWS Manged Microsoft AD
  - AD Connector
  - Single AD

### AWS Control Tower
  - Set up and govern a secure and compliant multi-account AWS environment
  - Guardrails
    - Preventive Guardrails - Using SCPs
    - Detective Guardrails - Using AWS Config

## Security & Encryption(KMS, SSM Parameter Store, CloudHSM, Shield, WAF)

### KMS(Key Mangement Service)
  - Symmetric (AES-256 keys)
    - Single Key
    - Don't get access to key
  - Asymmetric (RSA & ECC key pair)
    - Public and private key pair
  - Types of KMS keys
    - AWS Owned keys(free)
    - AWS Managed Key (free)
    - Customer managed key (priced)
  - Automatic key rotation
  - KMS key policies
    - Default KMS Key policy
      - Complete access to the root user
    - Custom KMS Key Policy

### KMS Multi Region Key
  - Identical KMS keys in different regions that can be used interchangeably
  - Have same key ID, key material, automatic rotation
  - Encrypt in one region and decrypt in another region
  - NOT global
  - Use cases: DynamoDB Global, Aurora Global

### S3 Replication with Encryption
  - Unencrypted objects and objects encrypted with SSE-S3 are replicated by default
  - Objects encrypted with SSE-KMS 
  - Can use multi-region keys but they are treated as independent keys by S3

### Sharing encrypted AMI via KMS
   -  Source accoune encreypt with KMS key
   -  Add **Launch Permission** to target account
   -  Share KMS keys
   -  Create IAM role at target account with enough permissions

### SSM Parameter Store
  - Secure store for configuration and secrets
  - Encryption(optional)
  - Versioning
  - Notification via EventBridge
  - Support hierarchy
  - Expiration policy(TTL)

### AWS Secret Manger
  - Capability to force rotation of secrets every X days
  - Automatic generation of secrets using Lambda
  - Integration with Amazon RDS
  - Multi region secrets
    - Replicate secrets across multiple AWS regions
    - Secret manager keep them in sync

### AWS Certificate Manager(ACM)
  - Easily provision, manage and deploy **TLS certificates**
  - Support both private and public certificates
  - Free for public certificates
  - Auto TLS certificate renewal
  - ACM sends daily expiration events starting 45 days prior to expiration to EventBridge
  - AWS Config has a managed rule to check expiring certificates
  - Integration API Gateway 
    - Edge Optimized: TLS must be in same region as CloudFront
    - Regional: TLS must be imported on API Gateway, in the same region as the API Stage

### AWS WAF(Web Application Firewall)
  - Protest against Layer 7 exploits
  - Layer 7 is HTTP
  - Deploy on
    - ALB
    - API Gateway
    - CloudFront
    - AppSync GraphQL API
    - Cognito User Pool
  - Define Web ACL(Access Control List) Rules
    - IP Set
    - SQL injection, Cross-Site Scripting
    - Geo-match(block countries)
    - Rate based : DDos protection
  - Web ACL are Regional except for CoundFront
  - Rule group - reusable set of rules

### AWS Shield: protect from DDos attack
  - Shield Standard
  - Shield Advanced
    - For more sophisticated attacks

### Firewall Manager
  - Manage rules in all accounts of an AWS organisation

### GuardDuty
  - Intelligent threat discovery
  - Use ML
  - Input data
    - CloudTrail event logs
    - VPC flow logs
    - DNS logs
    - Kubernates Audit logs
  - Good against CryptoCurrency attacks
  - Can link to EventBridge

### Amazon Inspector
  - Automated security assessments
  - For EC2
    - Using SSM agent
    - unintented network accessibility
    - OS vulnerabilities
  - For Container images at ECR
  - For Lambda functions

### Amazon Macie
  - Fully manage data security and data privacy service
  - Machine learning & pattern matching to discover and protect sensitive data
  - Sensitive data(Personal identifiable Information)

## Networking (VPC)

### CIDR
  - Classless Inter-Domain Routing - Method of allocating IP addresses
  - BaseIP
    - Represents an IP contained in the range
  - SubnetMask: How many bits can change
    - Two forms
      - /8  -> 255.0.0.0
      - /16 -> 255.255.0.0
      - /24 -> 255.255.255.0
      - /32 -> 255.255.255.255
    - Allows part of the underlying IP to get additional next values from base IP
      - 192.168.0.0/32 -> allows 1 IP(2^0) -> 192.168.0.0
      - 192.168.0.0/31 -> allows 2 IP(2^1) -> 192.168.0.0 - 192.168.0.1
      - 192.168.0.0/30 -> allows 4 IP(2^2) -> 192.168.0.0 - 192.168.0.3
      - 192.168.0.0/29 -> allows 8 IP(2^3) -> 192.168.0.0 - 192.168.0.7

### Default VPC
  - Has Internet connectivity and all EC2 inside have public IPv4

### VPC
  - Virtual Private Cloud
  - Mutp 5 per region(soft limit)
  - Max 5 CIDR per region

### Subnet
  - AWS reserves 5 IP addresses(first4 and last one)
  - Eg CIDR block 10.0.0.0/24
    - 10.0.0.0  - Network address
    - 10.0.0.1  - Reserved for VPC router
    - 10.0.0.2  - Reserved for Amazon provided DNS
    - 10.0.0.3  - Reserved for future use
    - 10.0.0.255 -  Network broadcast address

### Internet Gateway (IGW)
  - Allow resources in VPC to connect to Internet
  - IGW need a route table updated also

### Bastion Host
  - To SSH into private EC2 instances
  - Bastion is in a public usbnet in same VPC
  - Bastion host security group must allow inbout from internet on port 22 from restricted CIDR(company CIDR)
  - Security group of EC2 should allow traffic private IP of Bastion host

### NAT Instances
  - Network Address Translations
  - Ec2 in private subnets to connect to internet
  - Must disable source/destination check
  - Must have a Elastic IP attached
  - Preconfigured Amazon Linux AMI agailable

### NAT Gateway
  - AWS managed NAT
  - Created in a specific AZ and use an elastic IP
  - Can't use by EC2 in same subnet
  - Require an IGW(Private subnet -> NATGW -> IGW)
  - Need multiple AZ NAT for fault tolerance

### Security groups & NACLs
  - Stateless(Security group is stateful)
  - Both ALLOW and DENY rules
  - Like a firewall, control traffic from and to subnets
  - One NACL per subnet, new subnets are assigned the Default NACL
  - User define NACL Rules
    - Rules have a number
    - Higher precedence with lower number
    - Fist rule match drive the decision
    - Last rule is * and deny everything
  - Ephemeral Ports
    - Clients connect to a defined port expect a response on an ephemeral port
  - Ephemeral port range should be allowed in NACL in respective cases

### VPC Peering
  - Connect two VPC using AWS network
  - Must not have overlapping CIDR
  - Not transitive
  - Must update route tables in each VPC

### VPC Endpoints
  - Services to access other services without internet 
  - Use PrivateLink
  - In case of issues check
    - DNS settings in VPC
    - Route tables
  - Interface Endpoints(PrivateLink)
    - Provision an ENI(private IP), must attach to security group
    - Support most services
    - Paid
    - Preferred when connection from on permisses or different VPC
  - Gateway Endpoints
    - Provisions a gateway must be used as a target in a route table
    - Support S3 and DynamoDB
    - Free

### VPC Flow Logs
  - Capture IP traffic going into interfaces
  - Help to monitor connectivity and troubleshoot issues

### Site to Site VPN, Virtual Private Gateway & Customer Gateway

  - Site to Site VPN
    - Customer Gateway : Corperate side, If device doesn't have a public IP can use behind a NAT device
    - VPN Gateway/VGW(Virtual Private Gateway) : VPC side
    - S2S connection betwen public internet
    - If need to ping EC2 need to enable ICMP protocol
  - AWS VPN CloudHub
    - Secure communication between multiple sites, if u have multiple VPN
    - Low cost hub and spoke model for primary or secondary network connectivity between VPN

### Direct Connect(DX) & Direct Connect Gateway
  - Direct Connect(KX)
    - Dedicated private connection from a remote network to VPC
    - Need Virtual Private Gateway on VPC
    - Access public resources(S3) and private(EC2) on VPC
  - Direct conmect gateway
    - To setup one or more VPC in many regions
  - Connection types
    - Dedicated connections: 1Gbps, 10Gbps, 100Gbps
  - Hosted Connections: 50Mbps, 500Mbps to 10Gbps
  - Lead times are often longer than 1 month for new connection
  - Encrypt - no encryption

### Transit Gateway
  - Solve complex VPC peering
  - Have transitive peering between thousands of VPC and on permises in hub and spoke model
  - Regional, can work cross region
  - Route tables: Limit who can talk to who
  - Support **IP Multicast**
  - **ECPM - Equal cost multi path routing** ( Create multiple Site-to-Site VPN connections to increase the bandwidth of connections)
  - Share direct connect betwen multiple VPN
    - VPN(s) -> Transit GW -> Direct Connect GW -> Direct Connect endpoint -> Customer Router

### VPC Traffic Mirroring
  - Capture and inspect network traffic in VPC
  - Content inspection, threat moitoring

### IPv6 for VPC
  - IPv4 cannot be disabled for VPC and subnets
  - If EC2 cannot be launched in subnet
    - No available IPv4 in subnet
    - Create a new IPv4 CIDR

### Egress only Internet Gateway
  - Used for IPv6 only
  - Similar to NAT GW but for IPv6
  - Must update route tables

### AWS Network Firewall
  - Sophisticated way to project entire VPC
  - Layer 3 to 7 protection
  - Traffic filtering: Allow, drop or alert for traffic that matches rules
  - Active flow inspection

## Disaster Recovery & Migrations

### Overview
  - Kinds of DR
    - On-permise -> On-permise
    - On-permise -> AWS Cloud
    - AWS Cloud region A -> AWS cloud region B
  - Terms
    - RPO: Recovery Point Objective
      - How often backup is taken
      - How much time tolerated
    - RTO: Recovery Time Objective
      - Amount of down time between disaster
  - DR Strategies
    - Backup & Restore
      - Easy, High RPO, High RTO, Cheap
    - Pilot Light
      - Small version of critical app running in cloud
      - Low RPO, Low RTO
    - Warm Standby
      - Full system up and running at minimum size
      - Costly
    - Hot Site / Multi Site Approach
      - Very low RTO, RPO
      - Very expensive

### Database Migration Service(DMS)
  - Migrate databases to AWS, resilient, self healing
  - Need to craete EC2 to perform the replication tasks
  - Schema convertion Tool(SCT)
    - When need to convert schema

### RDS & Aurora Migration
  - Take DB Shnapshot and restore in Aurora
  - Crete read replica and once no replication lag promote to own DB cluster
  - Use Percona XtraBackup to backup in S3(For external Mysql)

### On-Premise strategy with AWS
  - Download Amazon Linux 2 as a VM(iso) and restore in VMWare etc
  - AWS Application Discovery Service
  - AWS Database Migration Service(DMS)
  - AWS Server Migration Service(SMS)

### AWS Backup
  - Centrally manage and automate backups
  - Can creaet backup policies(Backup Plans)
  - Backup Valut Lock
    - Backup cannot be 
    - Even root user can't delete

### Application Migration Service(MGN)
  - Plan migration by gathering on permisses data center
  - Agentless Discovery
  - Agent-based Discovery(more info)
  - Result can view at AWS migration Hub