# aws-saa

## Legend

- [IAM & AWS CLI](#iam-and-aws-cli)
- [IAM MFA](#iam-mfa)
- [EC2 Fundermentals](#ec2-fundermentals)
- [EC2 Solutions Architect Level](#ec2-solutions-architect-level)
- [EC2 Instance Storage](#ec2-instance-storage)

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
