# AWS Solutions Architect Associate

## Reference
[Do Your Homework: 7 AWS Certified Solutions Architect Exam Tips (Toptal)](https://www.toptal.com/aws-cloud-engineers/aws-certified-solutions-architect-exam-tips?utm_campaign=Toptal%20Engineering%20Blog&utm_source=hs_email&utm_medium=email&utm_content=82924000&_hsenc=p2ANqtz-9e8pBmq7n-oAcI1KJysb6eOjefvmT1pyAikVEgPso5N_p9HFPVsj2xKEao5RfwkluPNyub0hFcKkHCTQAsbC_TUsD0zQ&_hsmi=82924000)

## 10,000 Foot Overview
- [AWS Global Infrastructure](https://aws.amazon.com/about-aws/global-infrastructure/)
    - Availability Zone =  data center
    - Region: Composed of two or more availability zones
    - Edge Location: endpoints for caching location (CDN)
- Support Plan
    - Basic (Free), Developer and Business
    - Enterprise: if you need an AWS TAM

## [IAM - Identity and Access Management](https://aws.amazon.com/iam/)
- [FAQs](https://aws.amazon.com/iam/faqs/)
- Manage users and their level of access to AWS
- Centralized control
    - IAM is global (not tied to specific regions)
- Granular Permissions
- Identify Federation (Faceebok, LinkedIn, etc)
- [MFA](https://aws.amazon.com/iam/features/mfa/)
- You can implement your own password rotation policy
- Groups are a collection of Users
- Policy: documents in JSON used to grant permissions
- [IAM Role](https://aws.amazon.com/iam/faqs/#IAM_role_management): a way to allow a resource the ability to talk to another resource
- Root Account: account used when 1st setup your AWS account
- IAM is a feature of your AWS account offered at no additional charge
    - You will be charged only for use of other AWS services by your users
- [IAM Best Practices](http://docs.aws.amazon.com/IAM/latest/UserGuide/IAMBestPractices.html)
- Princing
    - IAM is a feature of your AWS account offered at no additional charge
    - You will be charged only for the use of other AWS services by your users

## [S3](https://aws.amazon.com/s3/)

[![Introduction to S3](https://img.youtube.com/vi/_I14_sXHO8U/0.jpg)](https://youtu.be/_I14_sXHO8U)

- Safe place to store files
- Object-based storage
- Files are stored in buckets
    - 0 < file size <= 5 TB
- Objects
    - Key: name
    - Value: sequence of bytes
    - Version ID: important if bucket has versioning enabled
    - Metadata: data about your object
    - Subresources: ACLs and Torrents

### S3 Data Consistency
- Read after write for PUTS
    - *If you upload a new file, you're able to read it immediately*
- Eventual consistency for overwrite PUTS and DELETES
    - *If you upload/delete an existing file and try to read it immediately, you may get its older version*

### S3 Storage Classes
- Standard
- Infrequently Accessed (IA)
    - Data accessed less frequently but still requires fast access
- One Zone (IA)
    - Data accessed less frequently but still requires fast access
    - Do not require multiple AZs relience
- Intelligent Tiering
    - Automatically moves data to cheaper storage using Machine Learning
- Glacier
    - Secure, durable and low-cost storage
    - Retrieval time adjustable from minutes to hours
- Glacier Deep Archive
    - Cheapest storage solution in AWS
    - Retrieval time: 12 hours

### S3 Versioning
- Stores all versions of an object (including all writes even if you delete the object)
- Once enabled cannot be disabled, only suspended
- You can change permissions of any object version
    - Old versions permissions *DO NOT* change if you change permissions on the latest version
    - *File A v1 is public, you upload a new version of it, becoming v2, then you change File A to be private, users will still be able to access File A v1*

### S3 Cross-Region Replication
*Replication of objects between buckets in != regions*
- Versioning must be enabled on both Source and Destination buckets
- Existing files in the source bucket are not replicated automatically to destination
- All subsequent updated files are replicated automatically
- *DELETES* are not replicated (including the deletion of individual versions)

### S3 Lifecycle Management
- Automates moving your objects between != storage tiers
- Can be used with versioning
- Can be applied to current and previous versions

### S3 Encryption At Rest
*Allows you to encrypt your data while it's stored in S3 (encryption at transit is met with HTTPS)*
- Server Side
    - SSE-S3
    - SSE-KMS
    - SSE-C (customer-provided keys)
- Client Side
    - You have to encrypt your data somewhere and then upload it to S3

### S3 Cross-Account Access
- Programatic Access Only
    - Using bucket policies and IAM
    - Using bucket ACLs and IAM on individual objects
- Both Programatic and Console Access
    - Cross-account IAM roles

### S3 Pricing
- Size
    - More GB, more $$$
- Requests
    - More requests to files, more $$$
- Storage Management
    - != storage classes have != pricing
- Data Transfer
    - More file moves, more $$$
- Data Transfer Acceleration
    - Transfer Accelearation allows you to transfer files to the closest AWS Edge location, then use AWS backbone infrastructure to transfer those files to the actual bucket
- Cross-Region Replication

### S3 Query and Auditing (Athena x Macie)
- Athena
    - *Interactive query service*
    - Allows you to query data in S3 using SQL
    - Serverless
    - Commonly used to analyze logs in S3
    - [FAQs](https://aws.amazon.com/pt/athena/faqs/)
- Macie
    - *Security service*
    - Uses AI to analyze data in S3
    - Can analyze Cloud Trail

### CloudFront
- *Content Delivery Network (CDN)*
- Edge Location: where content will be cached (read and write)
- Origin: source of all files CDN will distribute (S3, EC2, ELB and R53)
- Distribuition: CDN given name, which consists of a collection of Edge Locations
- Web Distribuition: used for websites
- RTMP: media streaming
- Objects are cached for the life of TTL
- You can invalidate cache, but you're charged for it

### Storage Gateway
- NFS (File GTW): flat files stored in S3
- Volume GTW
    - Stored: entire dataset on site and backed up to S3 asynchronously
    - Cached: entire dataset on S3 and most frequently accessed data is cached on sit
- GTW Virtual Tape Library

### S3 FAQs
- [S3 FAQs](https://aws.amazon.com/s3/faqs/)
- [S3 Glacier FAQs](https://aws.amazon.com/glacier/faqs/)

## EC2
*Provides resizable compute capacity on the cloud*

- [FAQs](https://aws.amazon.com/ec2/faqs/#longer-ids)

### Pricing
- On demand
- Reserved
    - Predictable usage
    - Standard Reserved Instances
    - Convertible Reserved Instances: can change instance type (t2.medium -> m2.medium)
        - For more information, check out [Standard  vs. Convertible Offering Classes](https://docs.aws.amazon.com/whitepapers/latest/cost-optimization-reservation-models/standard-vs.-convertible-offering-classes.html)
    - Scheduled Reserved Instances: launched within specific time windows
- Spot
    - Flexible start and end times (if AWS stops the instance, you don't pay; if you stop it, you pay)
- Dedicated Hosts

### Launching EC2 Instances
- Termination protection is turned off by default
- Root Volumes
    - By default, deleted when the instance is terminated
    - Can now be encrypted
- Additional volumes can be encrypted

### Security Groups
- All inbound traffic is blocked by default
- All outbound traffic is allowed by default
- SG changes take effect immediately
- 1 SG * EC2 instances
- 1 EC2 instance 5 SGs
- You can specify allow rules but not deny rules
- You cannot block specific IP addresses
    - Use Network ACLs instead

### [EBS - Elastic Block Storage](https://aws.amazon.com/ebs/)
- [FAQs](https://aws.amazon.com/ebs/faqs/)
- Like a virtual HD on the cloud

### EBS x Instance Store
- [Instance Store](https://docs.aws.amazon.com/AWSEC2/latest/WindowsGuide/InstanceStorage.html)
    - Provides ephemeral block-level storage for your instance
    - It is ideal for temporary storage of information that changes frequently, such as buffers, caches, scratch data, and other temporary content, or for data that is replicated across a fleet of instances, such as a load-balanced pool of web servers
    - Cannot be stopped (reboot/terminate)
- EBS can be stopped
- Both Root volumes are deleted on termination by default

### Volumes & Snapshots
- [Snapshots](https://aws.amazon.com/ebs/faqs/#Snapshots)
    - Snapshots exist on S3
    - Snapshots are point in time copies of volume
    - Snapshots are incremental (deltas)
        - For more information, check out [How Snapshots Work](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/EBSSnapshots.html#how_snapshots_work)
    - Stop the instance before taking a snapshot of the root volume
    - [EBS Deleting Snapshot](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ebs-deleting-snapshot.html)
- [EBS Encryption](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/EBSEncryption.html)
    - [FAQs](https://aws.amazon.com/ebs/faqs/#Encryption)
    - EBS encryption enables data at rest security by encrypting your data using Amazon-managed keys, or keys you create and manage using the [AWS Key Management Service (KMS)](https://aws.amazon.com/kms/).
    - How to create an **encrypted** volumes from an **unencrypted** one?
        - Take a snapshot of the encrypted volume
        - Copy that snapshot and select the encryption method
        - Create an AMI from that copied snapshot
        - Create an instance from that AMI
            - Its root volume must be encrypted too
    - Snapshots of encrypted evolumes are encrypted automatically
    - Volumes restored from encrypted snapshots are encrypted automatically
    - You can launch an encrypted EBS instance from an unencrypted AMI
        - When you launch an instance from an AMI backed by unencrypted EBS snapshots, you can encrypt some or all of the volumes during launch
        - For more information, check out [AMI Encryption](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/AMIEncryption.html)
    - You can share encrypted snapshots and AMIs with other accounts
        - You can share an AMI with specific AWS accounts without making the AMI public. All you need is the AWS account IDs
        - If you share an AMI with encrypted volumes, you must also share any CMKs used to encrypt them
        - For more information, refer to [Sharing an AMI with specific AWS accounts](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/sharingamis-explicit.html)
- [Pricing](https://aws.amazon.com/ebs/faqs/#Billing_and_metering)
    - You will be billed for the IOPS provisioned when it is disconnected from an instance
    - When a volume is detached, we recommend you consider creating a snapshot and deleting the volume to reduce costs

### ENI x EN x EFA
- [Elastic Network Interface](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/using-eni.html)
    - Basic networking
    - Traditional virtualized network interface
    - Perhaps you need a separate management network to your PROD network on a separate logging network at low cost
- [Enhanced Networking](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/enhanced-networking.html)
    - Enhanced networking uses single root I/O virtualization (SR-IOV) to provide high-performance networking capabilities on supported instance types
    - SR-IOV is a method of device virtualization that provides higher I/O performance and lower CPU utilization when compared to traditional virtualized network interfaces
    - Enhanced networking provides higher bandwidth, higher packet per second (PPS) performance, and consistently lower inter-instance latencies
    - Pricing
        - There is no additional charge for using enhanced networking
    - Enhanced Network Adapter (ENA)
        - Amazon EC2 provides enhanced networking capabilities through ENA
        - To use enhanced networking, you must install the required ENA module and enable ENA support
- [Elastic Fabric Adaptor](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/efa.html)
    - Network device that you can attach to your Amazon EC2 instance to accelerate High Performance Computing (HPC) and machine learning applications
    - EFA provides lower and more consistent latency and higher throughput than the TCP transport traditionally used in cloud-based HPC systems

### [Cloud Watch](https://aws.amazon.com/cloudwatch/)
*Monitoring service to monitor AWS resources and applications*
- [FAQs](https://aws.amazon.com/cloudwatch/faqs/)
- Monitor performance
- CW with EC2 will monitor events every 5 minutes by default
    - You can have 1-minute intervals by turning on detailed monitoring
- You can create [alarms](https://aws.amazon.com/cloudwatch/faqs/#Alarms) to monitor any CloudWatch metric
    - For example, you can create alarms on an Amazon EC2 instance CPU utilization, Amazon ELB request latency, Amazon DynamoDB table throughput, Amazon SQS queue length, or even the charges on your AWS bill
    - You can also create an alarm on custom metrics that are specific to your custom applications or infrastructure
    - When you create an alarm, you can configure it to perform one or more automated actions when the metric you chose to monitor exceeds a threshold you define
        - You can set an alarm that sends you an email, publishes to an SQS queue, stops or terminates an EC2 instance, or executes an Auto Scaling policy
        - You can also use any notification type supported by SNS
- [CloudTrail](https://aws.amazon.com/cloudtrail/faqs/) is all about auditing
    - CloudTrail is a web service that records activity made on your account and delivers log files to your Amazon S3 bucket
    - Monitors and logs AWS API calls

### [EFS - Elastic File System](https://aws.amazon.com/efs/)
- [FAQs](https://aws.amazon.com/efs/faq/)
- EFS provides a file system interface and file system access semantics (such as strong consistency and file locking)
- Supports thousands of connections simultaneously
- Automatically scale from GB to PB
- Supports NFS v4 protocol
- Read after write consistency
- Pay for what you use
    - no pre-provision required like 8 GB of EBS
- [Data Protection and Availability](https://aws.amazon.com/efs/faq/#Data_protection_and_availability)
    - Data is stored across multiple availability zones within a region
    - File system can be accessed concurrently from all AZs in the region where it is located -- application can failover from one AZ to other AZs in the region for high availability
- EFS offers the ability to encrypt data at rest and in transit
    - Data encrypted at rest is transparently encrypted while being written, and transparently decrypted while being read
        - Encryption keys are managed by KMS
    - Data encryption in transit uses TLS 1.2 to encrypt data sent between your clients and EFS file systems

### [FSx](https://aws.amazon.com/fsx/)
- FSx for Windows
    - [FAQs](https://aws.amazon.com/fsx/windows/faqs/)
    - Provides fully managed, highly reliable file storage that is accessible over the industry-standard Service Message Block (SMB) protocol
    - Used when you need cetralized storage for Windows-based apps such as SharedPoint, SQL Server, IIS Web Server (SMB storage)
- FSx for Lustre
    - [FAQs](https://aws.amazon.com/fsx/lustre/faqs/)
    - When you need high-speed, high-capacity distributed storage (HPC, financial modelling, etc)
    - Designed for applications that require fast storage – where you want your storage to keep up with your compute
    - FSx for Lustre Integrates with S3
        - When linked to an S3 bucket, an FSx for Lustre file system transparently presents S3 objects as files and allows you to write changed data back to S3

### [Placement Groups](https://docs.aws.amazon.com/AWSEC2/latest/WindowsGuide/placement-groups.html)
- When you launch a new EC2 instance, the EC2 service attempts to place the instance in such a way that all of your instances are spread out across underlying hardware to minimize correlated failures
    - You can use placement groups to influence the placement of a group of interdependent instances to meet the needs of your workload
- Types
    - [Clustered](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/placement-groups.html#placement-groups-cluster)
        - Low network latency
        - High network throughput
        - Cannot span multiple AZs
        - AWS recommends homogeneous instances in the same clustered placement group
    - [Spread](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/placement-groups.html#placement-groups-spread)
        - Individual critical EC2 instances
        - Can span multiple AZs
        - ![](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/images/placement-group-spread.png)
    - [Partitioned](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/placement-groups.html#placement-groups-partition)
        - Multiple critical EC2 instances
        - Can span multiple AZs
        - Can have a maximum of 7 partitions per AZ
        - ![](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/images/placement-group-partition.png)
- [Rules and Limitations](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/placement-groups.html#concepts-placement-groups)
    - Cannot merge placement groups
    - Can move *stopped* intances to placement groups
        - move/removal must be done via CLI or SDK

### [WAF - Web Application Firewall](https://aws.amazon.com/waf/)
- [FAQs](https://aws.amazon.com/waf/faqs/)
- Web application firewall that helps protect web applications from attacks by allowing you to configure rules that allow, block, or monitor (count) web requests based on conditions that you define:
    - IP addresses
        - Countries that requests originate from
    - HTTP headers
        - Values in headers
    - HTTP body
        - Length of requests
    - URI strings
    - SQL injection
    - Cross-site scripting
- How does AWS WAF block or allow traffic?
    - CDN, ALB or API Gateway receives requests for your web site, those requests are forwarded to AWS WAF for inspection against your rule
    - If a request meets a condition defined in your rules, AWS WAF instructs the underlying service to either block or allow the request based on the action you define
- Layer 7

## DNS
- [Limitations](https://docs.aws.amazon.com/Route53/latest/DeveloperGuide/DNSLimitations.html#limits-api-entities-domains)
- ELBs do not have a pre-defined IPv4 address
    - You can resolve to them using a DNS name
- [Common DNS types](https://docs.aws.amazon.com/Route53/latest/DeveloperGuide/ResourceRecordTypes.html)
    - SOA, NS, A, CNAME, MX, PTR
- ALIAS record set is different from CNAME records
    - CNAME (canonical name): resolve one domain to another
    - CNAME cannot be used for naked domain names (zone apex record)
        - Cannot have a CNAME for http://acloud.guru (it'd have to be an ALIAS record)
    - *Always choose ALIAS over CNAME records in exam questions*
    - Alias Records have special functions that are not present in other DNS servers. Their main function is to provide special functionality and integration into AWS services. Unlike CNAME records, they can also be used at the Zone Apex, where CNAME records cannot. Alias Records can also point to AWS Resources that are hosted in other accounts by manually entering the ARN Further information: https://docs.aws.amazon.com/Route53/latest/DeveloperGuide/ResourceRecordTypes.htmlhttps://docs.aws.amazon.com/Route53/latest/DeveloperGuide/resource-record-sets-choosing-alias-non-alias.htmlhttps://tools.ietf.org/html/rfc2181https://tools.ietf.org/html/rfc1034
- You can buy domain names directly from AWS

### Routing Policies
- Simple
    - Does not take into account the state of the resources
- Weighted
- Latency-based
- [Failover](https://docs.aws.amazon.com/Route53/latest/DeveloperGuide/routing-policy.html)
    - ACTIVE/PASSIVE endpoints (if *active* record set fails health check, R53 falls back to *passive* record)
- [Geolocation](https://docs.aws.amazon.com/Route53/latest/DeveloperGuide/routing-policy.html#routing-policy-geo)
- [Geoproximity (traffic flow only)](https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/georestrictions.html)
- [Multi-value answer](https://docs.aws.amazon.com/Route53/latest/DeveloperGuide/routing-policy.html#routing-policy-multivalue)
    - Multivalue answer routing lets you configure Amazon Route 53 to return multiple values, such as IP addresses for your web servers, in response to DNS queries. Route 53 responds to DNS queries with up to eight healthy records and gives different answers to different DNS resolvers. The choice of which to use is left to the requesting service effectively creating a form or randomisation
- You can set health checks on individual record sets
    - If record set fails a health check, it'll be removed from R53 until it passes again the health check
    - YOu can set SNS notification to alert you if health check has failed

## VPC
- Logical data center in AWS (logically isolated from other resources in the cloud)
- Consists of:
    - Subnets
    - Internet Gateway
    - Route Tables
    - Network ACLs
    - Security Groups
- When you create a VPC, AWS creates by default Network ACL, RT and SG
- 1 Subnet = 1 AZ
- Security Groups are stateful
- Network ACLs are stateless
- No transitive peering
- /16 is the largest CIDR you can attach to a VPC, whereas /28 is the smallest
- You can only have 1 internet gateway per VPC

### NAT Instances x NAT Gateways
- NAT allow instances in privte subnets to talk to the Internet without being public
- NAT Instances
    - When creating a NAT instance, you must disable source/destination check on the instance
    - NAT instances must be in the public subnet
    - There must be a route out of the private subnet to the NAT instance (in order for this to work)
    - The amount of traffic that NAT instances can support depends on the instance size (*if you're bottlenecking, increase the instance size*)
    - Associated with Security Groups
- NAT Gateway
    - Redundant in the availability zone
    - Not associated with Security Groups
    - Automatically assigned a public IP address
    - Remember to update your Route Tables
    - No need to disable source/destination checks
    - For high availability, multiple NAT gateways in multiple zones are required

### Network ACL
- VPCs automatically come with a default network ACL, and by default it allows ALL inbound and outbound traffic
- Custom Netowork ACL
    - Each custom network ACL denies ALL inbound and outbound traffic by default
    - Each subnet must be associated with a network ACL
        - By default, subnets are associated with the default network ACL
- Allows you to block IP addresses (you cannot do that using Security Groups)
- 1 network ACL can be associated with many subnets
- 1 subnet can be associated with only one network ACL
- Network ACLs contain a numbered list of rules that's evaluated in order, starting with the lowest numbered rule
- Have separate inbound and outbound rules and each rule can either allow or deny traffic
- Stateless: responses to alllowed inbound traffic are subject to the rules for outbound traffic (and vice-versa)

### VPC Flow Logs
- Cannot enable flow logs for VPCs that are peered with your VPC unless the peer VPC is in your account
- Can tag flow logs
- Once a flow log is created, it cannot be changed
- Not all IP traffic is monitored
    - Traffic generated by instances when they contact AWS DNS (if you use your own DNS server, then the traffic is logged)
    - Traffic generated by a Windows instance for AWS license activation
    - Traffic to and from 169.254.169.254
    - DHCP traffic
    - Traffic to the default VPC router

### Bastion Hosts
- Used to securely administer EC2 instances (SSH or RDP)
- Cannot use NAT gateway as a bastion host
    - NAT gateway or NAT instances are used to provide internet traffic to EC2 instances in private subnets

### Connections to AWS
2 popular ways to connect your on premise equipment to the AWS Cloud.

#### VPN Connections

It's usually the most used. It is the  "cheapest" option, fast to implement and secure way. 

It creates a secure and encrypted tunnel from the cloud to your on premise hardware, throughout a Public Network. I've seen that on the Onboarding process journey to the Cloud, it is usually preferred because is fast to implement, and with no visible additional cost. 

You only need any VPN Terminal (Router/Firewall) with Internet connection.   

There is drawback, it depends on the Internet Connection and if fails also the backend of your cloud applications. (believe me, it happens a lot) That could impact on your business results.

#### Direct Connect

Directly connects your data center to AWS. It's the best one: secure, private and robust. Useful for high throughput.

It is about connecting a dedicated fiber cable the nearest point of presence of your **Cloud Partner**, and he will make the link to his cloud equipment in order to transport your date privately to your cloud. 

It's like having a dedicated link to the cloud. No Internet failure could affect your solution.

##### Steps Required to create a direct connect connection
[![Steps to create DC connection to AWS](https://img.youtube.com/vi/dhpTTT6V1So/0.jpg)](https://www.youtube.com/watch?v=dhpTTT6V1So)

[Create Connection](https://docs.aws.amazon.com/directconnect/latest/UserGuide/create-connection.html)

### Global Accelerator
- Service in which you create accelerators to improve availability and performance of your applications for local and global users
- Get 2 static IPs (you can also bring your own ips)
- Control traffic using traffic dials (done within the endpoint group)

### VPC Endpoints
- A [VPC endpoint](https://docs.aws.amazon.com/vpc/latest/userguide/vpc-endpoints.html) enables you to privately connect your VPC to supported AWS services and VPC endpoint services powered by AWS PrivateLink without requiring an internet gateway, NAT device, VPN connection, or AWS Direct Connect connection
- Instances in your VPC do not require public IP addresses to communicate with resources in the service
- Traffic between your VPC and the other service does not leave AWS network
- Endpoints are virtual devices. They are horizontally scaled, redundant, and highly available VPC components
- There are two types of VPC endpoints:
    - Interface endpoints
        - Interface endpoint is an elastic network interface with a private IP address from the IP address range of your subnet that serves as an entry point for traffic destined to a supported service
    - Gateway endpoints
        - Gateway that you specify as a target for a route in your route table for traffic destined to a supported AWS services: S3 and DynamoDB

## High Availability Architecture
- Design for failure
- Use multiple zones and regions where you can
- Know the difference between multi-AZ and read replicas for RDS
- Know the difference between scaling out and scaling up
- Know the different S3 storage classes
- Always consider the cost element $$$

### Load Balancer
- Classic (layer 4), Network (layer 4) and Application (layer 7)
- 504 error: app not responding within the idle timeout period (gateway timeout)
- *X-Forwarded-For* header: IPv4 of your end user
- [ELB FAQs](https://aws.amazon.com/elasticloadbalancing/faqs/)
- Sticky Sessions
    - Enable users to stick to the same servers
- Cross Zone Load Balancing
    - Enables you to load balance accross multiple AZs
- Path Patterns
    - Allows you to direct traffic to different servers based on the URL contained in the request (useful for microservices)

### Auto Scaling and Launch Configuration

### Cloud Formation
- Scripting your cloud environment
- Quick Start
    - Cloud Formation templates built by AWS Solutions Architects

### Elastic Beanstalk
- Quickly deploy and manage applications in AWS
    - no worry about the application infrastrcture
- Upload your app and EB automatically handles the details of capacity provisioning, LB, scaling and health monitoring

## Databases

### [RDS](https://aws.amazon.com/rds/)
- [FAQs](https://aws.amazon.com/rds/faqs/)
- RDS is a managed service that makes it easy to set up, operate, and scale a relational DB in the cloud
    - MySQL, MariaDB, Oracle, SQL Server, or PostgreSQL
    - You cannot log in to these OSs
    - Patching RDS OS and DB is AWS' responsability
    - OLTP
- Pricing
    - No up-front investments required, and you pay only for the resources you use
    - No charge is incurred when replicating data from your primary RDS instance to your secondary RDS instance
- Database Instances
    - RDS runs on VMs (it's not **not serverless**)
    - You can think of a DB instance as a DB environment in the cloud with the compute and storage resources you specify
    - You can create and delete DB instances, define/refine infrastructure attributes of your DB instance(s), and control access and security via Console, APIs, and CLI
    - Maintenance Window 
        - You also have the ability to change your DB instance’s backup retention policy, preferred backup window, and scheduled maintenance window
        - If you do not specify a preferred weekly maintenance window when creating your DB instance, a 30 minute default value is assigned
        - Maintenance events that require RDS to take your DB instance offline are 1) scale compute operations (which generally take only a few minutes from start-to-finish), 2) database engine version upgrades, and 3) required software patching
        - Running your DB instance as a Multi-AZ deployment can further reduce the impact of a maintenance event
- Security Groups
    - Technically a destination port number is needed, however with a DB security group the RDS instance port number is automatically applied to the RDS DB Security Group. Further information: http://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/Overview.RDSSecurityGroups.html
- Storage
    - https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/CHAP_Storage.html#USER_PIOPS
- Provisioned IOPS over standard storage
    - Provisioned IOPS becomes important when you are running production environments requiring rapid responses, such as those which run e-commerce websites. Without high performant responses from an RDS instance page loads of the website could suffer resulting in loss of business. If your workloads are not latency sensitive or you are running a test environment the additional cost of provisioned IOPS will not be cost beneficial to your project. Further information: https://aws.amazon.com/ebs/features/
- Troubleshooting
    - https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/APITroubleshooting.html
    - If you want your application to check RDS for an error, have it look for an error node in the response from the Amazon RDS API

#### RDS Backups, Multi-AZ and Read Replicas
- RDS can automatically back up your database and keep your database software up to date with the latest version
- Backups
    - Automated backups are enabled by default
    - DB snapshots
    - I/O may be briefly suspended while the backup process initializes (typically under a few seconds), and you may experience a brief period of elevated latency. Further information: http://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/USER_CreateSnapshot.htmlhttp://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/Overview.BackingUpAndRestoringAmazonRDSInstances.html
- Read Replicas
    - Can be multi-AZ
    - Used to increase performance
    - Must have backups turned on
    - Can be in different regions
    - Can be MySQL, PSQL, Maria DB, Oracle and Aurora
    - Can be promoted to master (*but that breaks read replicas*)
- Multi-AZ
    - Used for disaster recovery
    - Can force a failover from one AZ to another by rebooting the RDS instance

#### Encryption
- Supported by all DBs
- Done via KMS
- Once the RDS instance is encrypted, the data stored at rest in the underlying storage is encrypted, as re its automated backups, read replicates and snapshots

#### FAQs
https://aws.amazon.com/rds/faqs/#reserved-instances

### Dynamo DB
- [FAQs](https://aws.amazon.com/dynamodb/faqs/)
- AWS managed NoSQL database service
- Stored on SSD storage
- Distributed across 3 geographically different data centers by default
- [Read Consistency](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/HowItWorks.ReadConsistency.html)
    - Eventual consistent reads (1s), by default
    - Strong consistent reads

https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/HowItWorks.ProvisionedThroughput.html
https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/CapacityUnitCalculations.html

#### [Pricing](https://aws.amazon.com/dynamodb/pricing/)
- There will always be a charge for provisioning read and write capacity and the storage of data within DynamoDB
- There is no charge for the transfer of data into DynamoDB, providing you stay within a single region (if you cross regions, you will be charged at both ends of the transfer.) 
- There is no charge for the actual number of tables you can create in DynamoDB, providing the RCU and WCU are set to 0, however in practice you cannot set this to anything less than 1 so there always be a nominal fee associated with each table. Further information: 

### Readshift
- OLAP
- Data Warehouse used for BI
- Backups
    - Enabled by default (1-day retention period)
    - Max retention period is 35 days
    - Redshift attemps to maintain at least 3 copies of your data (original node, replica node and a backup in S3)
- Redshift can also replicate snapshots *synchronously* to S3 in another region for disaster recovery

### Aurora
- AWS proprietary DB
- MySQL and PSQL compatible
- Serverless
    - Simple, cost-effective option for infrequent, intermittent, or unpredictable workload
- 2 copies of data per AZ with minimum of 3 AZs = 6 copies
- 3 types of replica available:
    - Aurora
    - MySQL
    - PSQL
- Automated failover is only available with Aurora replicas

### Elasticache
- Increase DB and Web application performance
- Memcached (simple) x Redis (more robust)
- Redis is multi-AZ
- You can do backups and restores of Redis

## Serveless
https://forums.aws.amazon.com/thread.jspa?messageID=708378
https://aws.amazon.com/blogs/compute/parallel-processing-in-python-with-aws-lambda/

https://aws.amazon.com/lambda/pricing/

https://aws.amazon.com/architecture/well-architected/

https://docs.aws.amazon.com/lambda/latest/dg/lambda-permissions.html

https://aws.amazon.com/serverless/

https://aws.amazon.com/blogs/architecture/understanding-the-different-ways-to-invoke-lambda-functions/

https://docs.aws.amazon.com/lambda/latest/dg/invocation-scaling.html

https://aws.amazon.com/xray/

## Applications

### SQS
### SWF
### SNS
### Elastic Transcoder
### API Gateway
### Kinesis
### Web Identity Federator (Cognito)