# aws-saa

## Legend

- [IAM & AWS CLI](#iam-and-aws-cli)
- [IAM MFA](#iam-mfa)
- [EC2 Fundermentals](#ec2-fundermentals)
- [EC2 Solutions Architect Level](#ec2-solutions-architect-level)
- [EC2 Instance Storage](#ec2-instance-storage)
- [High Availability and Scalability](#high-availability-and-scalability)
- [AWS Fundermentals ( RSA + Aurora + ElastiCache )](#aws-fundermentals--rsa--aurora--elasticache-)
- [Route 53](#route53)

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

## CloudFront and Globl Accelerator

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