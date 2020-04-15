# AWS Solutions Architect Associate

## Reference
[Do Your Homework: 7 AWS Certified Solutions Architect Exam Tips (Toptal)](https://www.toptal.com/aws-cloud-engineers/aws-certified-solutions-architect-exam-tips?utm_campaign=Toptal%20Engineering%20Blog&utm_source=hs_email&utm_medium=email&utm_content=82924000&_hsenc=p2ANqtz-9e8pBmq7n-oAcI1KJysb6eOjefvmT1pyAikVEgPso5N_p9HFPVsj2xKEao5RfwkluPNyub0hFcKkHCTQAsbC_TUsD0zQ&_hsmi=82924000)

## 10,000 Foot Overview
- AWS Global Infrastructure
    - Availability Zone =  data center
    - Region: Composed of two or more availability zones
    - Edge Location: endpoints for caching location (CDN)
- Support Plan
    - Basic (Free), Developer and Business
    - Enterprise: if you need an AWS TAM

## IAM
[Identity and Access Management](https://aws.amazon.com/iam/)
- Manage users and their level of access to AWS
- Centralized control
- Granular Permissions
- Identify Federation (Faceebok, LinkedIn, etc)
- [MFA](https://aws.amazon.com/iam/features/mfa/)
- You can implement your own password rotation policy
- Groups > Users
- Policy: documents in JSON used to grant permissions
- Role: a way to allow a resource the ability to talk to another resource
- IAM is global (not tied to specific regions)
- Root Account: account used when 1st setup your AWS account
- IAM is a feature of your AWS account offered at no additional charge
    - You will be charged only for use of other AWS services by your users
- [IAM Best Practices](http://docs.aws.amazon.com/IAM/latest/UserGuide/IAMBestPractices.html)

## S3
[Simple Storage Service](https://aws.amazon.com/s3/)

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

### EBS
*Elastic Block Store*

- Like a virtual HD on the cloud
- Snapshots exist on S3
- Snapshots are point in time copies of volume
- Snapshots are incremental (deltas)
    - For more information, check out [How Snapshots Work](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/EBSSnapshots.html#how_snapshots_work)
- Stop the instance before taking a snapshot of the root volume
- [EBS Deleting Snapshot](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ebs-deleting-snapshot.html)

### AMI Types (EBS x Instance Store)
- Instance Store Volumes
    - Called Ephemeral Storage (underlying host fails, data is lost)
    - Cannot be stopped (reboot/terminate)
- EBS can be stopped
- Both Root volumes are deleted on termination

### Volumes & Snapshots
- How to create an **encrypted** volumes from an **unencrypted** one?
    - Take a snapshot of the encrypted volume
    - Copy that snapshot and select the encryption method
    - Create an AMI from that copied snapshot
    - Create an instance from that AMI
        - Its root volume must be encrypted too
- Snapshots of encrypted evolumes are encrypted automatically
- Volumes restored from encrypted snapshots are encrypted automatically
- Only **unencrypted** snapshots can be shared
    - Can be shared with other AWS accounts and made public

### ENI x EN x EFA
- *Elastic Network Interface*
    - Basic networking
    - Perhaps you need a separate management network to your PROD network on a separate logging network at low cost
- *Enhanced Network*
    - Speeds between 10 and 100 Gbps (reliable, high throughput)
- *Elastic Fabric Adaptor*
    - Accelerate HPC and ML apps

### Cloud Watch
*Monitor service to monitor AWS resources and applications*
- Monitor performance
- CW with EC2 will monitor events every 5 minutes by default
    - You can have 1-minute intervals by turning on detailed monitoring
- Can create CW alarms which trigger notifications
- CloudTrail is all about auditing
    - monitors and logs AWS API calls

### EFS
*Elastic File System*
- Supports NFS v4 protocol
- Pay for what you use
    - no pre-provision required like 8 GB of EBS
- Can scale up to PB
- Supports thousands of NFS connections
- Data is stored across multiple availability zones within a region
- Read after write consistency

### FSx
- FSx for Windows
    - When you need cetralized storage for Windows-based apps such as SharedPoint, SQL Server, IIS Web Server (SMB storage)
- FSx for Lustre
    - When you need high-speed, high-capacity distributed storage (HPC, financial modelling, etc)
    - FSx for Lustre can store data on S3

### Placement Groups
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

### WAF
[Web Application Firewall](https://aws.amazon.com/waf/)
- Layer 7
- Monitors HTTP(S) requests to CDN, ALB or API Gateway
- Controls access to your content
- Extra protection using conditions you specify
    - IP addresses
    - Countries that requests originate from
    - Values in headers
    - Length of requests
    - Malicious code: SQL injection, script injection
    - Strings that appear in requests (regex)
