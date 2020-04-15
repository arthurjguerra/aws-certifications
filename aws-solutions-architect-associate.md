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

## S3
[Simple Storage Service](https://aws.amazon.com/s3/)
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
