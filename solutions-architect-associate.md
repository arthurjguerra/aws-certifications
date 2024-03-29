# AWS Solutions Architect Associate (SAA-C02)

## Exam Tips
- [Do Your Homework: 7 AWS Certified Solutions Architect Exam Tips (Toptal)](https://www.toptal.com/aws-cloud-engineers/aws-certified-solutions-architect-exam-tips?utm_campaign=Toptal%20Engineering%20Blog&utm_source=hs_email&utm_medium=email&utm_content=82924000&_hsenc=p2ANqtz-9e8pBmq7n-oAcI1KJysb6eOjefvmT1pyAikVEgPso5N_p9HFPVsj2xKEao5RfwkluPNyub0hFcKkHCTQAsbC_TUsD0zQ&_hsmi=82924000)
- [SAA-C02 Exam Tips AWS Certified Solutions Architect Associate](https://hakeem-me.cdn.ampproject.org/c/s/hakeem.me/2020/09/25/saa-c02-exam-tips-aws-certified-solutions-architect-associate/amp/)

## Exam Guides
- [Associate SAA-C02 Exam Guide - NEW VERSION](https://d1.awsstatic.com/training-and-certification/docs-sa-assoc/AWS-Certified-Solutions-Architect-Associate_Exam-Guide.pdf)
- [Passing the AWS Certified Solutions Architect Associate (SAA-C02) Exam](https://cantrill.io/2020/05/24/Passing-the-AWS-certified-solutions-architect-associate-saa-c02-certification.html)
- [AWS SAA C02 Course Exam Notes](https://github.com/alozano-77/AWS-SAA-C02-Course)
- [AWS Whitepapers](https://aws.amazon.com/whitepapers/)
- [FreeCodeCamp YouTube Series](https://www.youtube.com/watch?v=Ia-UEYYR44s)

## Exam Topics
- AWS Direct Connect: direct, private connection from your on-premises data center to AWS, letting you establish a 1-gigabit or 10-gigabit dedicated network connection using Ethernet fiber-optic cable
- VPC Endpoints: The important concept that you have to understand in this scenario is that your VPC and your S3 bucket are located within the larger AWS network. However, the traffic coming from your VPC to your S3 bucket is traversing the public Internet by default. To better protect your data in transit, you can set up a VPC endpoint so the incoming traffic from your VPC will not pass through the public Internet, but instead through the private AWS network. A VPC endpoint enables you to privately connect your VPC to supported AWS services and VPC endpoint services powered by PrivateLink without requiring an Internet gateway, NAT device, VPN connection, or AWS Direct Connect connection. Instances in your VPC do not require public IP addresses to communicate with resources in the service. Traffic between your VPC and the other services does not leave the Amazon network. Endpoints are virtual devices. They are horizontally scaled, redundant, and highly available VPC components that allow communication between instances in your VPC and services without imposing availability risks or bandwidth constraints on your network traffic. **There are two types of VPC endpoints: interface endpoints and gateway endpoints. You should create the type of VPC endpoint required by the supported service. As a rule of thumb, most AWS services use VPC Interface Endpoint except for S3 and DynamoDB, which use VPC Gateway Endpoint.** VPC Gateway Endpoint supports a private connection to S3.
- Aurora Failover: best-effort scenario in case the primary fails
- SQS metrics for ASG
- WAF: how it works? Allow/Deny requests from specific locations

## S3
[FAQs](https://aws.amazon.com/s3/faqs/)

### General
Amazon S3 is object storage built to store and retrieve any amount of data from anywhere on the Internet. Store any type and amount of data that you want Since Amazon S3 is highly scalable and you only pay for what you use, you can start small and grow your application as you wish, with no compromise on performance or reliability. 

One advantage of using Amazon S3 over on-premises storage solutions is that it enables any developer to leverage Amazon’s own benefits of massive scale with no up-front investment or performance compromises.

Amazon S3 is a simple key-based object store. When you store data, you assign a unique object key that can later be used to retrieve the data. Keys can be any string, and they can be constructed to mimic hierarchical attributes. 

Alternatively, you can use S3 Object Tagging to organize your data across all of your S3 buckets and/or prefixes.

To interact with S3, Amazon provides a simple, standards-based REST web services interface that is designed to work with any Internet-development toolkit. 

Amazon S3 delivers **strong read-after-write consistency** automatically. **After a successful write of a new object or an overwrite of an existing object, any subsequent read request immediately receives the latest version of the object**. 

S3 also provides strong consistency for list operations, so after a write, you can immediately perform a listing of the objects in a bucket with any changes reflected.

Strong read-after-write consistency helps when you need to immediately read an object after a write. For example, strong read-after-write consistency when you often read and list immediately after writing objects. 

High-performance computing workloads also benefit in that when an object is overwritten and then read many times simultaneously, strong read-after-write consistency provides assurance that the latest write is read across all reads. 

These applications automatically and immediately benefit from strong read-after-write consistency. S3 strong consistency also reduces costs by removing the need for extra infrastructure to provide strong consistency.  

Individual Amazon S3 objects can range in size from a minimum of 0 bytes to a maximum of 5 terabytes. The largest object that can be uploaded in a single PUT is 5 gigabytes. For objects larger than 100 megabytes, customers should consider using the Multipart Upload capability.

There are 4 different URLs styles that it can be used to access content in S3
- The Virtual Hosted Style URL: `https://<BUCKET_NAME>.s3.<AWS_REGION>.amazonaws.com/<OBJECT_KEY_NAME>`
- The Path-Style Access URL: `https://s3.<AWS_REGION>.amazonaws.com/<BUCKET_NAME>/<OBJECT_KEY_NAME>`
- The Static web site URL: `https://<BUCKET_NAME>.s3-website-<AWS_REGION>.amazonaws.com` or `http://<BUCKET_NAME>.s3-website.<AWS_REGION>.amazonaws.com`
- The Legacy Global Endpoint URL: `https://s3.amazonaws.com/<BUCKET_NAME>/<OBJECT_KEY_NAME>` or `https://s3-us-east-2.amazonaws.com/<BUCKET_NAME>/<OBJECT_KEY_NAME>`

### Storage Classes

Amazon S3 offers a range of storage classes designed for different use cases:

#### S3 Standard
Used for general-purpose storage of frequently accessed data (99.99% availability,); 

#### S3 Intelligent-Tiering

Used for data with unknown or changing access patterns;

It is designed to optimize storage costs by automatically moving data to the most cost-effective tier. It monitors access patterns and moves objects that have not been accessed in 30 consecutive days to the Infrequent Access tier. Then, S3 Intelligent-Tiering automatically moves these objects that have not been accessed for 90 consecutive days to the Archive tier and finally after 180 consecutive days of no access to the Deep Archive Access tier. If the objects are accessed later, they are moved back to the Frequent Access Tier.

New object -> Frequent Access Tier (30 days) -> Infrequent Access Tier (90 days) -> Archive Tier (180 days) -> Deep Archive Tier

11 9's durability

S3 Intelligent-Tiering charges you for monthly storage, requests, and data transfer, and charges a small monthly fee for monitoring and automation per object. The S3 Intelligent-Tiering storage class stores objects in four storage access tiers: an Frequent Access tier priced at S3 Standard storage rates, an Infrequent Access tier priced at S3 Standard-Infrequent Access storage rates, an Archive Access tier priced at S3 Glacier storage rates, and a Deep Archive Access tier priced at S3 Glacier Deep Archive storage rates.

To access an object in the Archive or Deep Archive Access tiers, you need to issue a Restore request and the object will begin moving back to the Frequent Access tier, all within the S3 Intelligent-Tiering storage class. Objects in the Archive Access Tier are moved to the Frequent Access tier in 3-5 hours, objects in the Deep Archive Access tier are moved to the Frequent Access tier within 12 hours. Once the object is in the Frequent Access tier, you can issue a GET request to retrieve the object.

Objects smaller than 128KB are not eligible for auto-tiering and will always be stored at the frequent access tier rate. For each object archived to the archive access tier or deep archive access tier in S3 Intelligent-Tiering, Amazon S3 uses 8 KB of storage for the name of the object and other metadata (billed at S3 Standard storage rates) and 32 KB of storage for index and related metadata (billed at S3 Glacier and S3 Glacier Deep Archive storage rates).

#### S3 Standard-Infrequent Access (S3 Standard-IA) 
Used for data that is accessed less frequently but requires reapid access when needed

Combination of low cost and high performance = ideal for long-term storage, backups, and data store for disaster recovery

Set at the object level and can exist in the same bucket as the S3 Standard of S3 One-Zone-IA classes, allowing you to use S3 lifecyly policies to automatically transition objects between storage classes without any application changes

11 9's durability, 99.9% availability and 99.5% availability

There are 2 ways to push data into S3-Standard IA: 1) directly PUT them specifying the `STANDARD_IA` in the `x-amz-storage-class` header or 2) set lifecycle policies to transition objects from S3 standard to S3 Standard IA

#### S3 One Zone-Infrequent Access (S3 One Zone-IA)
Objects are stored in a single availability zone

S3 One Zone-IA storage redundantly stores data within that single Availability Zone to deliver storage at 20% less cost than geographically redundant S3 Standard-IA storage, which stores data redundantly across multiple geographically separate Availability Zones

S3 One Zone-IA offers a 99% available SLA and is also designed for 11 9’s of durability within the Availability Zone. But, unlike the S3 Standard and S3 Standard-IA storage classes, data stored in the S3 One Zone-IA storage class will be lost in the event of Availability Zone destruction

S3 One Zone-IA is best suited for easily re-creatable data such as backup and disaster recovery copies 

Like S3 Standard-IA, S3 One Zone-IA charges for the amount of storage per month, bandwidth, requests, early delete and small object fees, and a data retrieval fee. Amazon S3 One Zone-IA storage is 20% cheaper than Amazon S3 Standard-IA for storage by month, and shares the same pricing for bandwidth, requests, early delete and small object fees, and the data retrieval fee

As with S3 Standard-Infrequent Access, if you delete a S3 One Zone-IA object within 30 days of creating it, you will incur an early delete charge. For example, if you PUT an object and then delete it 10 days later, you are still charged for 30 days of storage

Like S3 Standard-IA, S3 One Zone-IA storage class has a minimum object size of 128KB. Objects smaller than 128KB in size will incur storage charges as if the object were 128KB

#### Amazon S3 Glacier (S3 Glacier)
Extremely low-cost storage service for data archival. Amazon S3 Glacier stores data for as little as $0.004 per gigabyte per month.

To keep costs low yet suitable for varying retrieval needs, Amazon S3 Glacier provides three options for access to archives, ranging from a few minutes to several hours

You can use Lifecycle rules to automatically archive sets of Amazon S3 objects to S3 Glacier based on object age. Also, S3 PUT to Glacier allows you to use S3 APIs to upload to the S3 Glacier storage class on an object-by-object basis, directly

To retrieve Amazon S3 data stored in the S3 Glacier storage class, initiate a retrieval request using the Amazon S3 APIs or Console. The retrieval request creates a temporary copy of your data in the S3 RRS or S3 Standard-IA storage class while leaving the archived data intact in S3 Glacier. You can specify the amount of time in days for which the temporary copy is stored in S3. You can then access your temporary copy from S3 through an Amazon S3 GET request on the archived object.

With restore notifications, you can now be notified with an S3 Event Notification when an object has successfully restored from S3 Glacier and the temporary copy is made available to you. The bucket owner (or others, as permitted by an IAM policy) can arrange for notifications to be issued to Amazon Simple Queue Service (SQS) or Amazon Simple Notification Service (SNS). Notifications can also be delivered to AWS Lambda for processing by a Lambda function.

To restore objects, Amazon S3 first retrieves the requested data from S3 Glacier, and then creates a temporary copy of the requested data in S3 (which typically takes a few minutes). The access time of your request depends on the retrieval option you choose: Expedited, Standard, or Bulk retrievals. 

For all but the largest objects (250MB+), data accessed using Expedited retrievals are typically made available within 1-5 minutes. Objects retrieved using Standard retrievals typically complete between 3-5 hours. Bulk retrievals typically complete within 5-12 hours.

You can also upgrade an in-progress request to a faster restore speed. S3 Restore Speed Upgrade is an override of an in-progress restore to a faster restore tier if access to the data becomes urgent. 

You can use S3 Restore Speed Upgrade by issuing another restore request to the same object with a new (faster) "tier" job parameter. You pay for each restore request and the per-GB retrieval charge for the faster restore tier. For example, if you issued a Bulk tier restore and then issued an S3 Restore Speed Upgrade request at the Expedited tier to override the in-progress Bulk tier restore, you would be charged for two requests and the per-GB retrieval charge for the Expedited tier.

Amazon S3 Glacier storage class is priced based on monthly storage capacity and the number of  Lifecycle transition requests into Amazon S3 Glacier. Objects that are archived to Amazon S3 Glacier have a minimum of 90 days of storage, and objects deleted before 90 days incur a pro-rated charge equal to the storage charge for the remaining days.

Amazon S3 Glacier is designed for use cases where data is retained for months, years, or decades. Deleting data that is archived to Amazon S3 Glacier is free if the objects being deleted have been archived in Amazon S3 Glacier for 90 days or longer. If an object archived in Amazon S3 Glacier is deleted or overwritten within 90 days of being archived, there will be an early deletion fee. This fee is prorated. If you delete 1GB of data 30 days after uploading it, you will be charged an early deletion fee for 60 days of Amazon S3 Glacier storage. If you delete 1 GB of data after 60 days, you will be charged for 30 days of Amazon S3 Glacier storage.

There are three ways to restore data from Amazon S3 Glacier – Expedited, Standard, and Bulk Retrievals - and each has a different per-GB retrieval fee and per-archive request fee (i.e. requesting one archive counts as one request).

#### Amazon S3 Glacier Deep Archive (S3 Glacier Deep Archive)
S3 Glacier Deep Archive is a new storage class that provides secure and durable object storage for long-term retention of data that is accessed once or twice in a year. 

From just $0.00099 per GB-month (less than one-tenth of one cent, or about $1 per TB-month), S3 Glacier Deep Archive offers the lowest cost storage in the cloud, at prices significantly lower than storing and maintaining data in on-premises magnetic tape libraries or archiving data off-site.

S3 Glacier Deep Archive is an ideal storage class to provide offline protection of your company’s most important data assets, or when long-term data retention is required for corporate policy, contractual, or regulatory compliance requirements.

S3 Glacier Deep Archive vs S3 Glacier: Choose S3 Glacier when you need to retrieve archived data typically in 1-5 minutes using Expedited retrievals. S3 Glacier Deep Archive, in contrast, is designed for colder data that is very unlikely to be accessed, but still requires long-term, durable storage. S3 Glacier Deep Archive is up to 75% less expensive than S3 Glacier and provides retrieval within 12 hours using the Standard retrieval speed. You may also reduce retrieval costs by selecting Bulk retrieval, which will return data within 48 hours.

11 9's durability

The easiest way to store data in S3 Glacier Deep Archive is to use the S3 API to upload data directly. Just specify "S3 Glacier Deep Archive" as the storage class.

You can also begin using S3 Glacier Deep Archive by creating policies to migrate data using S3 Lifecycle, which provides the ability to define the lifecycle of your object and reduce your cost of storage. These policies can be set to migrate objects to S3 Glacier Deep Archive based on the age of the object. You can specify the policy for an S3 bucket, or for specific prefixes. 

To retrieve data stored in S3 Glacier Deep Archive, initiate a “Restore” request using the Amazon S3 APIs or the Amazon S3 Management Console. The Restore creates a temporary copy of your data in the S3 One Zone-IA storage class while leaving the archived data intact in S3 Glacier Deep Archive. You can specify the amount of time in days for which the temporary copy is stored in S3. You can then access your temporary copy from S3 through an Amazon S3 GET request on the archived object.

When restoring an archived object, you can specify one of the following options in the Tier element of the request body: Standard is the default tier and lets you access any of your archived objects within 12 hours, and Bulk lets you retrieve large amounts, even petabytes of data inexpensively and typically completes within 48 hours.

S3 Glacier Deep Archive storage is priced based on the amount of data you store in GBs, the number of PUT/lifecycle transition requests, retrievals in GBs, and number of restore requests. This pricing model is similar to S3 Glacier.

S3 Glacier Deep Archive is designed for long-lived but rarely accessed data that is retained for 7-10 years or more. Objects that are archived to S3 Glacier Deep Archive have a minimum of 180 days of storage, and objects deleted before 180 days incur a pro-rated charge equal to the storage charge for the remaining days. 

S3 Glacier Deep Archive has a minimum billable object storage size of 40KB. Objects smaller than 40KB in size may be stored but will be charged for 40KB of storage.

#### S3 Outposts
Delivers object storage in your on-premises environment, using the S3 APIs and capabilities that you use in AWS today. AWS Outposts is a fully managed service that extends AWS infrastructure.

For S3 on Outposts, your data is stored in your Outpost on-premises environment, unless you manually choose to transfer it to an AWS Region.

### Pricing

With Amazon S3, you pay only for what you use. There is no minimum fee. You can estimate your monthly bill using the AWS Pricing Calculator.

Billing prices are based on:
- The location of your bucket
- Data Tranfers
- Storage Used
- Data Requests
- Data Retrieval

Prices vary depending on which Amazon S3 Region you choose. AWS charges less where our costs are less. For example, our costs are lower in the US East (Northern Virginia) Region than in the US West (Northern California) Region.

**There is no Data Transfer charge for data transferred within an Amazon S3 Region via a COPY request. Data transferred via a COPY request between AWS Regions is charged.**

There is no Data Transfer charge for data transferred between Amazon EC2 and Amazon S3 within the same region, for example, data transferred within the US East (Northern Virginia) Region. However, data transferred between Amazon EC2 and Amazon S3 across all other regions is charged at rates specified on the Amazon S3 pricing page, for example, data transferred between Amazon EC2 US East (Northern Virginia) and Amazon S3 US West (Northern California).

You pay for all bandwidth into and out of Amazon S3, except for the following:
- Data transferred in from the Internet
- Data transferred out to an Amazon Elastic Compute Cloud (Amazon EC2) instance, when the instance is in the same AWS Region as the S3 bucket (including to a different account in the same AWS region)
- Data transferred out to Amazon CloudFront (CloudFront)

Amazon S3 Data Transfer Out pricing applies whenever data is read from any of your buckets from a location **outside of the given Amazon S3 Region**. Data Transfer Out pricing rate tiers take into account your aggregate Data Transfer Out from a given region to the Internet across Amazon EC2, Amazon S3, Amazon RDS, Amazon SimpleDB, Amazon SQS, Amazon SNS and Amazon VPC.

For example, assume you transfer 1TB of data out of Amazon S3 from the US East (Northern Virginia) Region to the Internet every day for a given 31-day month. Assume you also transfer 1TB of data out of an Amazon EC2 instance from the same region to the Internet over the same 31-day month.

Your aggregate Data Transfer would be 62 TB (31 TB from Amazon S3 and 31 TB from Amazon EC2). This equates to 63,488 GB (62 TB * 1024 GB/TB).

This usage volume crosses three different volume tiers. The monthly Data Transfer Out fee is calculated below assuming the Data Transfer occurs in the US East (Northern Virginia) Region:
10 TB Tier: 10,239 GB (10×1024 GB/TB – 1 (free)) x $0.09 = $921.51
10 TB to 50 TB Tier: 40,960 GB (40×1024) x $0.085 = $3,481.60
50 TB to 150 TB Tier: 12,288 GB (remainder) x $0.070 = $860.16

Total Data Transfer Out Fee = $921.51+ $3,481.60 + $860.16= $5,263.27

The volume of storage billed in a month is based on the average storage used throughout the month. This includes all object data and metadata stored in buckets that you created under your AWS account. We measure your storage usage in “TimedStorage-ByteHrs,” which are added up at the end of the month to generate your monthly charges.

For example, Assume you store 100GB (107,374,182,400 bytes) of data in Amazon S3 Standard in your bucket for 15 days in March, and 100TB (109,951,162,777,600 bytes) of data in Amazon S3 Standard for the final 16 days in March.

At the end of March, you would have the following usage in Byte-Hours: Total Byte-Hour usage = [107,374,182,400 bytes x 15 days x (24 hours / day)] + [109,951,162,777,600 bytes x 16 days x (24 hours / day)] = 42,259,901,212,262,400 Byte-Hours.

Let's convert this to GB-Months: 42,259,901,212,262,400 Byte-Hours / 1,073,741,824 bytes per GB / 744 hours per month = 52,900 GB-Months

This usage volume crosses two different volume tiers. The monthly storage price is calculated below assuming the data is stored in the US East (Northern Virginia) Region: 50 TB Tier: 51,200 GB x $0.023 = $1,177.60 50 TB to 450 TB Tier: 1,700 GB x $0.022 = $37.40

Total Storage Fee = $1,177.60 + $37.40 = $1,215.00

For data requests, let's go through this example. Assume you transfer 10,000 files into Amazon S3 and transfer 20,000 files out of Amazon S3 each day during the month of March. Then, you delete 5,000 files on March 31st.
Total PUT requests = 10,000 requests x 31 days = 310,000 requests
Total GET requests = 20,000 requests x 31 days = 620,000 requests
Total DELETE requests = 5,000×1 day = 5,000 requests

Assuming your bucket is in the US East (Northern Virginia) Region, the Request fees are calculated below:
310,000 PUT Requests: 310,000 requests x $0.005/1,000 = $1.55
620,000 GET Requests: 620,000 requests x $0.004/10,000 = $0.25
5,000 DELETE requests = 5,000 requests x $0.00 (no charge) = $0.00

Amazon S3 data retrieval pricing applies for the S3 Standard-Infrequent Access (S3 Standard-IA) and S3 One Zone-IA storage classes.

For example, assume in one month you retrieve 300GB of S3 Standard-IA, with 100GB going out to the Internet, 100GB going to EC2 in the same AWS region, and 100GB going to CloudFront in the same AWS Region.

Your data retrieval fees for the month would be calculated as 300GB x $0.01/GB = $3.00. Note that you would also pay network data transfer fees for the portion that went out to the Internet.

In addition, when version is enabled on your bucket, you pay for every object version stored in S3. Rates apply for every version of an object stored or requested. For example:

1) Day 1 of the month: You perform a PUT of 4 GB (4,294,967,296 bytes) on your bucket.
2) Day 16 of the month: You perform a PUT of 5 GB (5,368,709,120 bytes) within the same bucket using the same key as the original PUT on Day 1.

When analyzing the storage costs of the above operations, please note that the 4 GB object from Day 1 is not deleted from the bucket when the 5 GB object is written on Day 15. Instead, the 4 GB object is preserved as an older version and the 5 GB object becomes the most recently written version of the object within your bucket. At the end of the month:

Total Byte-Hour usage
[4,294,967,296 bytes x 31 days x (24 hours / day)] + [5,368,709,120 bytes x 16 days x (24 hours / day)] = 5,257,039,970,304 Byte-Hours.

Conversion to Total GB-Months
5,257,039,970,304 Byte-Hours x (1 GB / 1,073,741,824 bytes) x (1 month / 744 hours) = 6.581 GB-Month

Normal Amazon S3 pricing applies when your storage is accessed by another AWS Account. Alternatively, you may choose to configure your bucket as a Requester Pays bucket, in which case the requester will pay the cost of requests and downloads of your Amazon S3 data.

You can get started with IPv6 on Amazon S3 by pointing your application to Amazon S3’s new "dual-stack" endpoint, which supports access over both IPv4 and IPv6. In most cases, no further configuration is required for access over IPv6, because most network clients prefer IPv6 addresses by default.

### Amazon S3 Transfer Acceleration

Amazon S3 Transfer Acceleration enables fast, easy, and secure transfers of files over long distances between your client and your Amazon S3 bucket. S3 Transfer Acceleration leverages Amazon CloudFront’s globally distributed AWS Edge Locations. As data arrives at an AWS Edge Location, data is routed to your Amazon S3 bucket over an optimized network path.

After S3 Transfer Acceleration is enabled, you can point your Amazon S3 PUT and GET requests to the s3-accelerate endpoint domain name: *.s3-accelerate.amazonaws.com* or *.s3-accelerate.dualstack.amazonaws.com*. If you want to use standard data transfer, you can continue to use the regular endpoints.

Generally, you will see more acceleration when the source is farther from the destination, when there is more available bandwidth, and/or when the object size is bigger. However, the amount of acceleration primarily depends on your available bandwidth, the distance between the source and destination, and packet loss rates on the network path.

Each time you use S3 Transfer Acceleration to upload an object, we will check whether S3 Transfer Acceleration is likely to be faster than a regular Amazon S3 transfer. If we determine that S3 Transfer Acceleration is not likely to be faster than a regular Amazon S3 transfer of the same object to the same destination AWS Region, we will not charge for the use of S3 Transfer Acceleration for that transfer, and we may bypass the S3 Transfer Acceleration system for that upload.

AWS Direct Connect is a good choice for customers who have a private networking requirement or who have access to AWS Direct Connect exchanges. S3 Transfer Acceleration is best for submitting data from distributed client locations over the public Internet, or where variable network conditions make throughput poor. Some AWS Direct Connect customers use S3 Transfer Acceleration to help with remote office transfers, where they may suffer from poor Internet performance.

### Security
Amazon S3 is secure by default. Upon creation, only the resource owners have access to Amazon S3 resources they create. Amazon S3 supports user authentication to control access to data. You can use access control mechanisms such as bucket policies and Access Control Lists (ACLs) to selectively grant permissions to users and groups of users. 

The Amazon S3 console highlights your publicly accessible buckets, indicates the source of public accessibility, and also warns you if changes to your bucket policies or bucket ACLs would make your bucket publicly accessible. You should enable Block Public Access for all accounts and buckets that you do not want publicly accessible.

You can securely upload/download your data to Amazon S3 via SSL endpoints using the HTTPS protocol. If you need extra security you can use the **Server-Side Encryption (SSE)** option to encrypt data stored at rest. You can configure your Amazon S3 buckets to automatically encrypt objects before storing them if the incoming storage requests do not have any encryption information. Alternatively, you can use your own encryption libraries to encrypt data before storing it in Amazon S3.

Customers may use four mechanisms for controlling access to Amazon S3 resources: 
- Identity and Access Management (IAM) policies
- Bucket policies
- Access Control Lists (ACLs)
- Query String Authentication

IAM enables organizations with multiple employees to create and manage multiple users under a single AWS account. With IAM policies, customers can grant IAM users fine-grained control to their Amazon S3 bucket or objects while also retaining full control over everything the users do. 

With bucket policies, customers can define rules which apply broadly across all requests to their Amazon S3 resources, such as granting write privileges to a subset of Amazon S3 resources. Customers can also restrict access based on an aspect of the request, such as HTTP referrer and IP address. 

With ACLs, customers can grant specific permissions (i.e. READ, WRITE, FULL_CONTROL) to specific users for an individual bucket or object. 

With Query String Authentication, customers can create a URL to an Amazon S3 object which is only valid for a limited time.

You have the following options to store sensitive data encrypted at rest in Amazon S3:
- SSE-S3
- SSE-C
- SSE-KMS
- Client library such as the Amazon S3 Encryption Client

**SSE-S3** provides an integrated solution where Amazon handles key management and key protection using multiple layers of security. You should choose SSE-S3 if you prefer to have Amazon manage your keys.

**SSE-C** enables you to leverage Amazon S3 to perform the encryption and decryption of your objects while retaining control of the keys used to encrypt objects. With SSE-C, you don’t need to implement or use a client-side library to perform the encryption and decryption of objects you store in Amazon S3, but you do need to manage the keys that you send to Amazon S3 to encrypt and decrypt objects. Use SSE-C if you want to maintain your own encryption keys, but don’t want to implement or leverage a client-side encryption library.

**SSE-KMS** enables you to use AWS Key Management Service (AWS KMS) to manage your encryption keys. Using AWS KMS to manage your keys provides several additional benefits. With AWS KMS, there are separate permissions for the use of the master key, providing an additional layer of control as well as protection against unauthorized access to your objects stored in Amazon S3. AWS KMS provides an audit trail so you can see who used your key to access which object and when, as well as view failed attempts to access data from users without permission to decrypt the data. Also, AWS KMS provides additional security controls to support customer efforts to comply with PCI-DSS, HIPAA/HITECH, and FedRAMP industry requirements.

Using an **encryption client library**, such as the Amazon S3 Encryption Client, you retain control of the keys and complete the encryption and decryption of objects client-side using an encryption library of your choice. Some customers prefer full end-to-end control of the encryption and decryption of objects; that way, only encrypted objects are transmitted over the Internet to Amazon S3. Use a client-side library if you want to maintain control of your encryption keys, are able to implement or use a client-side encryption library, and need to have your objects encrypted before they are sent to Amazon S3 for storage.

Instead of accessing buckets over the Internet, you can access it using the Amazon Global network via VPC Endpoints. An Amazon VPC Endpoint for Amazon S3 is a logical entity within a VPC that allows connectivity to S3 over the Amazon global network. 

There are two types of VPC endpoints for S3 – gateway VPC endpoints and interface VPC endpoints. Gateway endpoints are a gateway that you specify in your route table to access S3 from your VPC over the Amazon network. Interface endpoints extend the functionality of gateway endpoints by using private IPs to route requests to S3 from within your VPC, on-premises, or from a different AWS Region.

You can limit access to your bucket from a specific Amazon VPC Endpoint or a set of endpoints using Amazon S3 bucket policies. S3 bucket policies now support a condition, aws:sourceVpce, that you can use to restrict access.

AWS PrivateLink for S3 provides private connectivity between Amazon S3 and on-premises. You can provision interface VPC endpoints for S3 in your VPC to connect your on-premises applications directly to S3 over AWS Direct Connect or AWS VPN. You no longer need to use public IPs, change firewall rules, or configure an internet gateway to access S3 from on-premises.

Interface VPC endpoints provision an Elastic Network Interface (ENI) in your VPC. An ENI is a logical networking component through which you can route requests to S3 over the Amazon network. You can create an interface VPC endpoint in one or more Availability Zones, spanning one or more subnets. 

In each subnet that you specify, an ENI will be set up with an IP address from your private IP address pool. Requests to S3 are resolved to the private IPs assigned to the ENIs. Addressing S3 through private IP addresses in this way makes S3 directly reachable from on-premises hosts that are connected to AWS over AWS Direct Connect or AWS Virtual Private Network (VPN) via Private Virtual Interface.

VPC endpoints versus AWS PrivateLink-based interface VPC endpoints: AWS recommends that you use interface VPC endpoints to access S3 from on-premises or from a VPC in another AWS Region. For resources that are accessing S3 from VPC in the same AWS Region as S3, AWS recommends using gateway VPC endpoints as they are not billed.

Amazon Macie is an AI-powered security service that helps you prevent data loss by automatically discovering, classifying, and protecting sensitive data stored in Amazon S3. Amazon Macie uses machine learning to recognize sensitive data such as personally identifiable information (PII) or intellectual property, assigns a business value, and provides visibility into where this data is stored and how it is being used in your organization. Amazon Macie continuously monitors data access activity for anomalies, and delivers alerts when it detects risk of unauthorized access or inadvertent data leaks.

Access Analyzer for S3 is a feature that monitors your access policies, ensuring that the policies provide only the intended access to your S3 resources. Access Analyzer for S3 evaluates your bucket access policies and enables you to discover and swiftly remediate buckets with potentially unintended access.

Access Analyzer for S3 alerts you when you have a bucket that is configured to allow access to anyone on the internet or that is shared with other AWS accounts. You receive insights or ‘findings’ into the source and level of public or shared access. For example, Access Analyzer for S3 will proactively inform you if read or write access were unintendedly provided through an access control list (ACL) or bucket policy. With these insights, you can immediately set or restore the intended access policy.

### Durability & Data Protection
Amazon S3 Standard, S3 Standard–IA, S3 One Zone-IA, S3 Glacier, and S3 Glacier Deep Archive are all designed to provide 99.999999999% durability of objects over a given year.

As with any environment, the best practice is to have a backup and to put in place safeguards against malicious or accidental deletion. For S3 data, that best practice includes secure access permissions, Cross-Region Replication, versioning, and a functioning, regularly tested backup.

Amazon S3 Standard, S3 Standard-IA, and S3 Glacier storage classes redundantly store your objects on multiple devices across a minimum of three Availability Zones (AZs) in an Amazon S3 Region before returning SUCCESS. The S3 One Zone-IA storage class stores data redundantly across multiple devices within a single AZ. These services are designed to sustain concurrent device failures by quickly detecting and repairing any lost redundancy, and they also regularly verify the integrity of your data using checksums. 

### Storage Analytics and Insights
S3 Storage Lens provides recommendations contextually with storage metrics in a dashboard, so you can take action to optimize your storage based on the metrics.

### Queries In Place
Amazon S3 allows customers to run sophisticated queries against data stored without the need to move data into a separate analytics platform.

S3 offers multiple query in place options, including S3 Select, Amazon Athena, and Amazon Redshift Spectrum, allowing you to choose one that best fits your use case. 

S3 Select provides a new way to retrieve specific data using SQL statements from the contents of an object stored in Amazon S3 without having to retrieve the entire object. S3 Select simplifies and improves the performance of scanning and filtering the contents of objects into a smaller, targeted dataset.

Amazon Athena is an interactive query service that makes it easy to analyze data in Amazon S3 using standard SQL queries.

Amazon Redshift Spectrum is a feature of Amazon Redshift that enables you to run queries against exabytes of unstructured data in Amazon S3 with no loading or ETL required. 

### Replication
Amazon S3 Replication enables automatic, asynchronous copying of objects across Amazon S3 buckets. Buckets that are configured for object replication can be owned by the same AWS account or by different accounts. 

You can copy objects to one or more destination buckets between different AWS Regions (S3 Cross-Region Replication), or within the same AWS Region (S3 Same-Region Replication).

Versioning must be enabled for both the source and destination buckets to enable replication.

### S3 Object Lock
- S3 Object Lock
    - Store objects using a Write Once, Read Many (WORM) model
    - Can be applied to individual objects or accross the entire bucket
    - Two Modes:
        - **Governance Mode**: users cannot overwrite or delete an object version or alter lock settings (unless they have special permissions)
        - **Compliance Mode**: objects cannot be overwritten or deleted by **any** user (including root)
    

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
- [FAQs](https://aws.amazon.com/storagegateway/faqs/?nc=sn&loc=6)
- Hybrid cloud storage service that gives you on-premises access to virtually unlimited cloud storage
- Storage Gateway supports three key hybrid cloud use cases:
    1. Move backups and archives to the cloud
    1. Reduce on-premises storage with cloud-backed file shares
    1. Provide on-premises applications low latency access to data stored in AWS
-  Storage Gateway provides 3 types of storage interfaces:
    - File Gateway enables you to store and retrieve objects in Amazon S3 using file protocols such as Network File System (NFS) and Server Message Block (SMB)
        - File-based interface to Amazon S3, which appears as a network file share
    - Volume Gateway provides block storage to your on-premises applications using iSCSI connectivity. Data on the volumes is stored in Amazon S3 and you can take point in time copies of volumes which are stored in AWS as Amazon EBS snapshots. You can also take copies of volumes and manage their retention using AWS Backup. You can restore EBS snapshots to a Volume Gateway volume or an EBS volume
        - Provides an iSCSI target, which enables you to create block storage volumes and mount them as iSCSI devices from your on-premises or EC2 application servers. The Volume Gateway runs in either a cached or stored mode
        - In the **cached mode**, your primary data is written to S3, while retaining your frequently accessed data locally in a cache for low-latency access.
        - In the **stored mode**, your primary data is stored locally and your entire dataset is available for low-latency access while asynchronously backed up to AWS.
    - Tape Gateway provides your backup application with an iSCSI virtual tape library (VTL) interface, consisting of a virtual media changer, virtual tape drives, and virtual tapes. Virtual tapes are stored in Amazon S3 and can be archived to Amazon S3 Glacier or Amazon S3 Glacier Deep Archive
        - Cloud-based Virtual Tape Library (VTL)


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
    - Spot instances save up to 90% of the cost of On-demand Instances
    - You can block Spot instances from terminating by using Spot Block
    - Spot Fleet
        - Collection of Spot Instances and, optionally, On-demand instances
    - Spot Instance interruptions
        - Demand for Spot Instances can vary significantly from moment to moment, and the availability of Spot Instances can also vary significantly depending on how many unused EC2 instances are available
        - It is always possible that your Spot Instance might be interrupted
        - Possible reasons that Amazon EC2 might interrupt your Spot Instances: **Price** (the Spot price is greater than your maximum price), **Capacity** (if there are not enough unused EC2 instances to meet the demand for On-Demand Instances)
        - When Amazon EC2 is going to interrupt your Spot Instance, **it emits an event two minutes prior to the actual interruption** (except for hibernation, which gets the interruption notice, but not two minutes in advance, because hibernation begins immediately). This event can be detected by Amazon CloudWatch Events
        - You can specify that Amazon EC2 should do one of the following when it interrupts a Spot Instance: **Stop**, **Hibernate**, or **Terminate**

Constraints – If your request includes a constraint such as a launch group or an Availability Zone group, these Spot Instances are terminated as a group when the constraint can no longer be met.
- Dedicated Hosts

### Launching EC2 Instances
- Termination protection is turned off by default
- Root Volumes
    - By default, deleted when the instance is terminated
    - Can now be encrypted
- Additional volumes can be encrypted

### Autoscaling And Proactive Scaling
- Proactive scaling 
    - Either **cyclic (weekly / daily / monthly)** or **event based (black friday sale)** 
    - It's when you know there's going to be an increase in traffic so you set your instances to scale
    - Setup by you, whereby you change, for example, the number of your minimum instances from say 1 to maybe 5 for a period as you know there will be high demand (black friday)
- Autoscaling is demand based, it will wait until there is a trigger based on metrics before scaling up and then down again when demand drops
    - Pre-configured by you to automatically create more instances as, for example, your instance cpu usage exceeds 80%
    - It will create new instances up to the maximum you specified (example: minimum 1 instance, maximum 10 instances)
    - Also you can set a trigger whereby if the load decreases, which can be seem by your cpu usage dropping below 25% for an instance then it will automatically terminate instances

### EC2 Hibernate
- Preserves the in-memory RAM on persistent storage (EBS)
- Much faster to boot up because you do not need to reload the OS
- Instance RAM must be < 150 GB
- Available for Windows, Amazon Linux 2 and Ubuntu
- Instances cannot be hibernated for more than 60 days
- Available for On-Demand and Reserved Instances

### HPC on AWS
- Achieve HPC on AWS through:
    - Data Transfer
        - Snowball/Snowmobile
        - Data Sync
        - Direct Connect
    - Compute & Networking
        - GPU/CPU optimized instances
        - EC2 Fleets (spot)
        - Placement Groups
        - Enhanced Networking Single Root IO Virtualization (SR-IOV)
        - Elastic Network Adapters and Intel Virtual Function (VF) interface
        - Elastic Fabric Adapters
    - Storage
        - Instance Attached Storage
            - EBS: 64,000 IOPS with Provisioned IOPS
            - Instance Store: millions IOPS; low latency
        - Network Storage
            - S3
            - EFS
            - FSx for Lustre
    - Orchestration and Automation
        - AWS Batch
        - AWS Parallel Cluster

### Security Groups
- All **inbound** traffic is **blocked** by default and all **outbound** traffic is **allowed** by default
- SG changes take effect immediately
- 1 SG * EC2 instances
- 1 EC2 instance 5 SGs
- You can specify allow rules but not deny rules
- You cannot block specific IP addresses
    - Use Network ACLs instead
- Security groups act like a firewall at the instance level, whereas Network ACL are an additional layer of security that act at the subnet level
- Security Groups support "allow" rules only 
- Security Groups evaluate all rules before deciding whether to allow traffic
- Security Groups operate at the instance level.

### EBS - Elastic Block Storage
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

### Cloud Watch
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

### EFS - Elastic File System
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

### FSx
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

### Placement Groups
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
        - Group of instances that are each placed on distinct underlying hardware
        - ![](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/images/placement-group-spread.png)
    - [Partitioned](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/placement-groups.html#placement-groups-partition)
        - Multiple critical EC2 instances
        - Can span multiple AZs
        - Can have a maximum of 7 partitions per AZ
        - ![](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/images/placement-group-partition.png)
- [Rules and Limitations](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/placement-groups.html#concepts-placement-groups)
    - Cannot merge placement groups
    - Can move only *stopped* intances to placement groups
        - move/removal must be done via CLI or SDK

### WAF - Web Application Firewall
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

### Dedicated Hosts
- [FAQs](https://aws.amazon.com/ec2/dedicated-hosts/faqs/)
- An Amazon EC2 Dedicated Host is a physical server with EC2 instance capacity fully dedicated for your use
- Allow you to use your eligible software licenses from vendors such as Microsoft and Oracle on Amazon EC2, so that you get the flexibility and cost effectiveness of using your own licenses, but with the resiliency, simplicity and elasticity of AWS
- Bring your own licenses (BYOL)
    - One-time onboarding set up in AWS License Manager
    - Step 1: Define licensing rules for software licenses, such as Windows Server and SQL Server, that you want bring to AWS
    - Step 2: Specify your preferences for managing the underlying hosts. For example, you can define how to allocate and release hosts within the group, which licensing rules to use, and what instance types are allowed.
    - Step 3: (optional) You can share the host management preferences and licensing rules across your organizational accounts
- Pricing
    - **On-Demand**, where you have the flexibility to scale up and down the number of Dedicated Hosts allocated to your account and only pay for the hours those Dedicated Hosts were allocated to your account
    - **Reservation**, where you can purchase and assign a Reservation to a Dedicated Host and benefit from a lower rate over the term compared to On-Demand. Reservations can save you up to 70% on your On-Demand costs over the term
    - **Savings Plans**, which is a new flexible pricing model that will help you lower your bill by making a commitment to a consistent amount of compute usage (measured in $/hour) instead of making commitments to specific hosts. Savings Plans offer significant savings over On Demand, just like Reservations, but automatically reduce your bills on compute usage across any AWS region, even as usage changes


## IAM - Identity and Access Management
- [FAQs](https://aws.amazon.com/iam/faqs/)
- Manage users, and their level of access to AWS
- Centralized control
    - IAM is global (not tied to specific regions)
- Granular Permissions
- Identify Federation (Facebook, LinkedIn, etc)
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
    
### AWS Directory Service
- AD Compatible
    - Managed Microsoft AD
    - AD Connector
    - Simple AD
- Not AD Compatible
    - Cloud Directory
    - Cognito user pools

### IAM Policies
- It contains Effect/Action/Resource
- Identity vs Resource Policies
    - Identity Policies define permissions for users and groups
    - Resource Policies define permissions for AWS services
- Not explicitly allowed = implicitly denied
- Explicit deny >>> everything else
- Only attached policies have effect
- AWS joins multiple policies when evaluating them
- AWS-manged vs customer-managed policies
- Permission Boundaries
    - Prevent privilege escalation or unnecessarily broad permissions
    - Controls maximum permissions an IAM policy can grant
    
### AWS Resource Access Manager (RAM)
- Allows resource sharing between accounts

### AWS SSO
- Centrally manage access to AWS accounts and business apps
- SAML 2.0

## Simple Queue Service - SQS
- SQS is pull-based, not pushed-based: you need to have an EC2 instance pulling the messages out of the queue
- Queue types:
    - Default: does not guarantee delivery order (no limit of requests per second)
    - FIFO: first message in the queue is the first one to come out (limite of 300 requests per second)
- Messages are 256KB in size
- Messages are delivered **at least** once
- Retention Period
    - Messages can be kept in the queue from 1 minute to 14 days (default value is 4 days)
- **Visibility Timeout**:
    - Amout of time that the message is invisible in the queue *after* a reader picks up that message
    - If message is processed before the visibility timeout expires, then the message is deleted from the queue
    - If message is not processed within the visibility timeout period, then the message will become visible again and another reader will process it -- *this could result in the same message being delivered twice*

### Long Polling vs Short Polling
- [SQS Short and Long Polling](https://docs.aws.amazon.com/AWSSimpleQueueService/latest/SQSDeveloperGuide/sqs-short-and-long-polling.html)
- With **short polling**, the `ReceiveMessage` request queries only a subset of the servers (based on a weighted random distribution) to find messages that are available to include in the response
    - Amazon SQS sends the response right away, even if the query found no messages
- **Short polling** occurs when the `WaitTimeSeconds` parameter of a `ReceiveMessage` request is set to 0 in one of two ways:
    - The `ReceiveMessage` call sets `WaitTimeSeconds` to 0
    - The `ReceiveMessage` call doesn't set `WaitTimeSeconds`, but the queue attribute `ReceiveMessageWaitTimeSeconds` is set to 0.
- With **long polling**, the `ReceiveMessage` request queries all of the servers for messages. Amazon SQS sends a response after it collects at least one available message, up to the maximum number of messages specified in the request
    - Amazon SQS sends an empty response only if the polling wait time expires

## DNS
- [Limitations](https://docs.aws.amazon.com/Route53/latest/DeveloperGuide/DNSLimitations.html#limits-api-entities-domains)
- ELBs do not have a pre-defined IPv4 address
    - You can resolve to them using a DNS name
- [Common DNS types](https://docs.aws.amazon.com/Route53/latest/DeveloperGuide/ResourceRecordTypes.html)
    - SOA, NS, A, CNAME, MX, PTR
- ALIAS vs CNAME records
    - Both point to another DNS record
    - Alias records let you route traffic to selected AWS resources, such as CloudFront distributions and Amazon S3 buckets. They also let you route traffic from one record in a hosted zone to another record
    - Unlike a CNAME record, you can create an alias record at the top node of a DNS namespace, also known as the **zone apex**
        - For example, if you register the DNS name `example.com`, the zone apex is `example.com`. You cannot create a CNAME record for `example.com`, but you can create an alias record for `example.com` that routes traffic to `www.example.com`

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
- When you [create a custom VPC](https://docs.aws.amazon.com/cli/latest/reference/ec2/create-vpc.html), a default Security Group, Access control List, and Route Table are created automatically
    - You must create your own subnets, Internet Gateway, and NAT Gateway (if you need one.) 
- 1 Subnet = 1 AZ
- Security Groups are stateful
    - When you allow inbound traffic for a specific IP range, outbound traffic to the same IP range is automatically allowed
- Network ACLs are stateless
    - You need to allow both inbound and outbound traffic explicitly
- No transitive peering
- /16 is the largest CIDR you can attach to a VPC (65536 addresses), whereas /28 is the smallest
- You can only have 1 internet gateway per VPC
    - However an IG is a fully-redundant component of your VPC provides Internet services to all of your public subnets, in all Availability Zones
- Having just created a new VPC and launching an instance into its Public Subnet, you realise that you have forgotten to assign a Public IP to the instance during creation. What is the simplest way to make your instance reachable from the outside world?
    - Create an Elastic IP address and associate it with your instance
    - Although creating a new NIC & associating an EIP also results in your instance being accessible from the internet, it leaves your instance with 2 NICs & 2 private IPs as well as the Public Address and is therefore not the simplest solution
    - By default, any user-created VPC subnet WILL NOT automatically assign Public IPv4 Addresses to instances – the only subnet that does this is the “Default” VPC subnets automatically created by AWS in your account
    - https://docs.aws.amazon.com/vpc/latest/userguide/vpc-ip-addressing.html#subnet-public-ip
- By default, instances in new subnets in a custom VPC can communicate with each other across Availability Zones
    - In a custom VPC with new subnets in each AZ, there is a Route that supports communication across all subnets/AZs
    - Plus, a Default SG with an allow rule 'All traffic, All protocols, All ports, from anything using this Default SG'
    - Further information: https://docs.aws.amazon.com/AmazonVPC/latest/UserGuide/default-vpc.html
    
### VPC and subnet sizing for IPv4
[The first four IP addresses and the last IP address in each subnet CIDR block cannot be used by you https://docs.aws.amazon.com/vpc/latest/userguide/VPC_Subnets.html).

For example, in a subnet with CIDR block 10.0.0.0/24, the following five IP addresses are reserved: 
- `10.0.0.0`: Network address
- `10.0.0.1`: Reserved by AWS for the VPC router
- `10.0.0.2`: Reserved by AWS. The IP address of the DNS server is the base of the VPC network range plus two. For VPCs with multiple CIDR blocks, the IP address of the DNS server is located in the primary CIDR. We also reserve the base of each subnet range plus two for all CIDR blocks in the VPC
- `10.0.0.3`: Reserved by AWS for future use
- `10.0.0.255`: Network broadcast address. We do not support broadcast in a VPC, therefore we reserve this address

### Internet Gateway
- [Egress Only Internet Gateway](https://docs.aws.amazon.com/vpc/latest/userguide/egress-only-internet-gateway.html)
    - An instance in your public subnet can connect to the Internet through the Internet gateway if it has a public IPv4 address or an IPv6 address
        - Similarly, resources on the Internet can initiate a connection to your instance using its public IPv4 address or its IPv6 address
    - IPv6 addresses are globally unique, and are therefore public by default. If you want your instance to be able to access the Internet, but you want to prevent resources on the Internet from initiating communication with your instance, you can use an egress-only Internet gateway
    - To do this, create an egress-only Internet gateway in your VPC, and then add a route to your route table that points all IPv6 traffic (::/0) or a specific range of IPv6 address to the egress-only Internet gateway. IPv6 traffic in the subnet that's associated with the route table is routed to the egress-only Internet gateway
    - An egress-only Internet gateway is stateful: it forwards traffic from the instances in the subnet to the Internet or other AWS services, and then sends the response back to the instances.
- You can only have 1 internet gateway per VPC
    - However, an IG is a fully-redundant component of your VPC provides Internet services to all of your public subnets, in all Availability Zones

### NAT Instances x NAT Gateways
- NAT allow instances in private subnets to talk to the Internet without being public
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
- Security groups act like a firewall at the instance level, whereas network ACLs are an additional layer of security that act at the subnet level

### VPC Flow Logs
- Once enabled for a particular VPC, VPC subnet, or Elastic Network Interface (ENI), relevant network traffic will be logged to CloudWatch Logs for storage and analysis by your own applications or third-party tools
- The information captured includes information about:
    - Allowed and denied traffic (based on security group and network ACL rules)
    - Source and destination IP addresses, ports, the IANA protocol number, packet and byte counts, a time interval during which the flow was observed, and an action (ACCEPT or REJECT)
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
- VPN Connections
    - It's usually the most used. It is the  "cheapest" option, fast to implement and secure way
    - It creates a secure and encrypted tunnel from the cloud to your on premise hardware, throughout a Public Network. I've seen that on the Onboarding process journey to the Cloud, it is usually preferred because is fast to implement, and with no visible additional cost
    - You only need any VPN Terminal (Router/Firewall) with Internet connection
    - There is drawback, it depends on the Internet Connection and if fails also the backend of your cloud applications
    - When connecting a VPN between AWS and a third party site, the Customer Gateway is created within AWS, but it contains information about the third party site e.g. the external IP address and type of routing. The Virtual Private Gateway has the information regarding the AWS side of the VPN and connects a specified VPC to the VPN
- Direct Connect
    - Directly connects your data center to AWS. It's the best one: secure, private and robust. Useful for high throughput
    - It is about connecting a dedicated fiber cable the nearest point of presence of your **Cloud Partner**, and he will make the link to his cloud equipment in order to transport your date privately to your cloud
    - It's like having a dedicated link to the cloud. No Internet failure could affect your solution
    - Data charges are still incurred whilst using Direct Connect
    - [![Steps to create DC connection to AWS](https://img.youtube.com/vi/dhpTTT6V1So/0.jpg)](https://www.youtube.com/watch?v=dhpTTT6V1So)
    - [Create Connection](https://docs.aws.amazon.com/directconnect/latest/UserGuide/create-connection.html)

### Global Accelerator
- [FAQs](https://aws.amazon.com/global-accelerator/faqs/)
- Improves performance for a wide range of applications over TCP or UDP by proxying packets at the edge to applications running in one or more AWS Regions
- Get 2 static IPs (you can also bring your own ips)
- Control traffic using traffic dials 
    - Done within the endpoint group
- High availability 
    - When you create an accelerator, you are allocated two IPv4 static IP addresses that are serviced by independent **network zones**
    - Similar to Availability Zones, these network zones are isolated units with their own physical infrastructure and serve static IP addresses from a unique IP subnet
        - If one static IP address becomes unavailable due to IP address blocking or unreachable networks, AWS Global Accelerator provides fault tolerance to client applications by rerouting to a healthy static IP address from the other isolated network zone

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

### [Penetration Testing]()
- Until recently customers were not permitted to conduct [Penetration Testing](https://aws.amazon.com/security/penetration-testing/) without AWS engagement. However that has changed. There are still conditions however

### AWS Private Link
- Expose service VPC to thousands of customer VPCs
- Does NOT required peering (no RTs, no NAT, no IGW)
- Requires a Network LB on the service VPC and ENI on the customer VPC
- ![](images/aws-private-link.png)

### AWS Transit Gateway
- Allows you to have transitive peering between thousands of VPCs and on-premises data centers
- Regional service but can have it across multiple regions
- You can use it across multiple AWS accounts via Resource Access Manager
- Works with Direct Connect and VPN
- Supports IP multicast

### AWS VPN CloudHub
- With multiple sites, each with its own VPN connection, you can use VPN CloudHub to connect those sites together
- Operates over the public Internet
- All traffic is encrypted

### AWS Network Costs
- Use private IP addresses over public IP to save on costs (AWS backbone network)
- To cut network costs, group all resources in a single AZ with private IPs

### On-Prem Strategies with AWS
- Database Migration Service
- Server Migration Service
- Application Discovery Service
- VM Import/Export
- Download Amazon Linux 2 as ISO

## High Availability Architecture
- Design for failure
- Use multiple zones and regions where you can
- Know the difference between multi-AZ and read replicas for RDS
- Know the difference between scaling out and scaling up
- Know the different S3 storage classes
- Always consider the cost element $$$

## Reliability x Availability x Resiliency
- https://www.quora.com/What-are-the-differences-between-reliability-availability-resiliency-and-fault-tolerance-in-IT-systems
- https://blog.westerndigital.com/data-availability-vs-durability/

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

### RDS
- [FAQs](https://aws.amazon.com/rds/faqs/)
- RDS is a managed service that makes it easy to set up, operate, and scale a relational DB in the cloud
    - MySQL, MariaDB, Oracle, SQL Server, or PostgreSQL
    - You cannot log in to these OSs
    - Patching RDS OS and DB is AWS' responsability
    - OLTP
- Pricing
    - No up-front investments required, and you pay only for the resources you use
    - No charge is incurred when replicating data from your primary RDS instance to your secondary RDS instance
    - Instance Hours
        - Based on region, instance type, DB engine and license
    - DB Storage
        - EBS x Aurora, Storage Type, Storage Allocation (GB)
    - Backup Storage
        - Size of backups stored in AWS
        - No charge for backup storage up to 100% of total DB storage
    - Data Transfer
        - Outgoing traffic only
        - Regional Data Transfer pricing
- Database Instances
    - RDS runs on VMs (it's not **not serverless**)
    - You can think of a DB instance as a DB environment in the cloud with compute and storage resources you specify
    - You can create and delete DB instances, define/refine infrastructure attributes of your DB instance(s), and control access and security via Console, APIs, and CLI
    - Maintenance Window 
        - You also have the ability to change your DB instance’s backup retention policy, preferred backup window, and scheduled maintenance window
        - If you do not specify a preferred weekly maintenance window when creating your DB instance, a 30 minute default value is assigned
        - Maintenance events that require RDS to take your DB instance offline are 
            1. Scale compute operations which generally take only a few minutes from start-to-finish 
            1. DB engine version upgrades 
            1. Required software patching
        - Running your DB instance as a Multi-AZ deployment can further reduce the impact of a maintenance event
- License Model (Oracle)
    - **On-demand**: License cost is included
    - Buy your own license and bring it to AWS
- DB instance class
    - Related to EC2 instance type:
        - T2/T3: Burstable instances, moderate networking performance 
        - M3/M4: General purpose, high networking performance
        - R3/R4: Memory Optimized, high networking performance
- DB Identifier
    - Identifies the database instance
    - Unique for your account across the region
- Credentials
    - Master user account
- Networking config
    - VPC
    - Subnet Groups
    - Is DB public or private?
    - AZs
    - Securiy Groups
- Storage Types
    - **GP2**: general purpose
    - **IO1**: I/O intensive workloads (recommended for prod)
    - **Magnetic**: supported for legacy DBs
- Troubleshooting
    - https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/APITroubleshooting.html
    - If you want your application to check RDS for an error, have it look for an error node in the response from the Amazon RDS API

#### Scaling your DB instance
- Scale Compute or Memory Vertically
    - In Multi-AZ, secondary resizes first, DNS points to secondary, then primary resizes
    - A New host is attached to existing volumes
    - You can modify the instance or change the instance class
        - Apply immediately
        - During maintenance window
- Scale EBS
    - No downtime
- Scale Horizontally
    - Scale read replicas

#### RDS Multi-AZ
- Leverages another AZ for a secondary host
    - Primary host is replicated to the secondary host before *ack'ed* to the client
    - DOES NOT improve performance (only failover scenarios)
- Writes to Multi-AZ DB instance are slightly slower
- Multi-AZ improves Backups
- Can also be used to minimize downtime when scaling up/down or during maintenance (software upgrades)
- When failovers occur?
    - AZ outage
    - Primary DB instance fails
    - DB instance class changes
    - Software patching
    - Manual Failover (instance is manually rebooted)
    - *Failovers* rely on DNS, so if you're caching DNS you might have issues

#### RDS Read Replicas
- Read-only DB instance
- Async data replication between Primary and Read Replica
    - Eventual Consistency
    - Lag dictate how eventual consistency is
- Read replicates can be in different regions
- Can be multi-AZ
- Must have backups turned on
- Can be MySQL, PSQL, Maria DB, Oracle and Aurora
- Can be promoted to master (*but that breaks read replicas*)
- When to use Read Replicas?
    - Scaling: 
        - Read-heavy apps
        - Increase performance
    - Primary DB Unavailable
    - Reporting or Data Warehouse
    - Disaster Recovery
    - Lower Latency (cross region deployments)
    

#### RDS Backups
- RDS can automatically back up your database and keep your database software up to date with the latest version
- Two Types
    - Automated backups
    - Manual snapshots

##### Automated Backups
- Multi-Region
    - Take a daily EBS snapshot of the secondary DB instance and push it to S3 (RDS-owned bucket)
    - Transaction logs are also pushed to S3 every 5 min (point-in-time recovery)
    - The snapshot is taken during maintenance window
    - Copy the snapshot to another region
- Single-Region
    - Similar to Multi-Region, except for daily Snapshots are taken from the Primary DB instance instead of Seconday
- Snapshots only store the difference
    - 1st snapshot has all the data
    - Subsequent snapshots are incremental
- Performance Impact
    - Single AZ
        - Brief IO suspension
    - Multi AZ
        - No IO suspension since snapshots are taken from secondary db instance
- Options:
    - Backup Window
    - Retention Period (1-35 days; 7 days is the default value)

##### Manual Snapshots
- Manually created via Console, SDK or CLI
- Kept until you delete them (no retention period)
- Restores to a saved snapshot
- Used for checkpoints before large changes
- Final copy before deleting a database

#### Restoring Backups
- Restoring a backup creates a new DB Instance
- Must retain Param Group and Sec group from the DB the snapshot was taken from

#### RDS Security and Monitoring
- Network Isolation via VPC
- Access Control
    - IAM
    - MFA
- Grant access to within the db
    - Do not use master creds
    - IAM
    - Active Directory for SQL Server
- Encryption at rest
    - Free
    - Encryption is replicated
    - Encryption at Rest (at the EBS volume level -  no impact on the app)
    - Encrypt once (cannot unencrypt data)
    - 2-tier Encryption (master key created by you, each RDS instance has its own data key)
- Supported by all DBs
- Done via KMS

#### RDS Monitoring
- Standard Monitoring
    - Metrics: CPU, Storage, Network Traffic, DB Connections, IOPS
    - Metrics are provided from outside the host - Hypervisor
- Enhanced Monitoring
    - OS/Host level metrics
    - 50 additional metrics in Cloud Watch
    - Granulity of 1 sec (default is 60s)
    - Metrics are provided from inside the host - Lightweight agent at the host
- Performance Insights
    - Provides insights into DB performance
    - Identifies bottlenecks
    - Analyze SQL queries, Hosts and users
    - Free feature not availabe on db.t2
- Other Monitoring Tools
    - RDS Events: subscribe to categories
    - AWS Config: records and evaluates changes to configuration
    - AWS CloudTrail: auditing on RDS API calls
    - AWS Trusted Advisor: $$$, Security, Fault Tolerance, and Performance improvement
    

#### Aurora
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

### Dynamo DB
- [FAQs](https://aws.amazon.com/dynamodb/faqs/)
- AWS managed NoSQL database service
- High Availability and Durability
     - DynamoDB automatically spreads the data and traffic for your tables over a sufficient number of servers to handle your throughput and storage requirements, while maintaining consistent and fast performance
     - Data is stored on solid-state disks (SSDs) and is automatically replicated across multiple Availability Zones in an AWS Region
     - You can use global tables to keep DynamoDB tables in sync across AWS Regions
- The maximum item size in DynamoDB is 400 KB, which includes both attribute name binary length (UTF-8 length) and attribute value lengths (again binary length). The attribute name counts towards the size limit
- Multi-Region Replication
    - Amazon DynamoDB global tables provide a fully managed solution for deploying a multiregion, multi-active database, without having to build and maintain your own replication solution
    - With global tables you can specify the AWS Regions where you want the table to be available
    - DynamoDB performs all of the necessary tasks to create identical tables in these Regions and propagate ongoing data changes to all of them
- [Read Consistency](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/HowItWorks.ReadConsistency.html)
    - Eventual consistent reads (1s), by default
    - Strong consistent reads
- Dynamo DB Accelerator (DAX)
    - Caching service
    - Sits in between the app and db
    - No code changes required
    - High available
    - Reduce calls from milliseconds to microseconds
- Transactions
    - All-or-nothing operations
    - 2 underlying reads and writes - prepare and commit
- On-demand Capacity
    - Balance cost and performance
    - Dynamo DB automatically provision more resource to serve all requests
    - Pay more per request when compared to provisioned capacity
- On-demand Backup & Restore
    - Full backups at anytime
    - Zero impact on table performance
    - Consistent within seconds
    - Operates in the same region as the source table
- Point in Time Recovery (PITR)
    - Restore to any point in the last 35 days
    - Incremental backups
    - Not enabled by default
    - Latest Restorable: 5 min in the past
- Streams
    - Time-ordered sequence of item-level changes in a table
    - Stored 24h
    - Inserts, Updates & Deletes
- Global Tables
    - Managed Multi-Master, Multi-Region Replication
    - Based on Dynamo DB Streams
    - Multi-region redundancy for DR and HA
    - Replication latency usually under 1 sec
- Security
    - Data is encrypted at rest with KMS
    - Control access with IAM policies and roles
    - Fine-grained access: IAM policies that allow access to parts of tables

#### Pricing
- There will always be a charge for provisioning read and write capacity and the storage of data within DynamoDB
- There is no charge for the transfer of data into DynamoDB, providing you stay within a single region (if you cross regions, you will be charged at both ends of the transfer.) 
- There is no charge for the actual number of tables you can create in DynamoDB, providing the RCU and WCU are set to 0, however in practice you cannot set this to anything less than 1 so there always be a nominal fee associated with each table. Further information: 

### Redshift
- OLAP
- Data Warehouse used for BI
- Backups
    - Enabled by default (1-day retention period)
    - Max retention period is 35 days
    - Redshift attemps to maintain at least 3 copies of your data (original node, replica node and a backup in S3)
- Redshift can also replicate snapshots *synchronously* to S3 in another region for disaster recovery

### Elasticache
- Increase DB and Web application performance
- Memcached (simple) x Redis (more robust)
- Redis is multi-AZ
- You can do backups and restores of Redis

### Database Migration Service (DMS) 
- Helps you migrated data from on-prem into AWS cloud, from AWS cloud to on-prem, and between on-premise DBs 
- Types of Migration:
    - Homogeneous: Oracle -> Oracle
    - Heterogeneous: SQL Server -> Aurora
- Schema-Conversion Tool (SCT)
    - Used in Heterogeneous migrations
![](images/dms.png)

### Caching Strategies in AWS
- CloudFront
- API GTW
- Elasticache - Memcached & Redis
- Dynamo DB Accelerator (DAX)

### Elastic MapReduce (EMR)
- Used for Big Data processing
- Consists of a Master, Core, and (optionally) a Task node
- By default, log data is stored in the master node
- You can configure replication to S3 on 5-minute intervals for all log data from the master node
    - This cannot be changed (need to configure that when creating the cluster)

## Serverless
https://forums.aws.amazon.com/thread.jspa?messageID=708378
https://aws.amazon.com/blogs/compute/parallel-processing-in-python-with-aws-lambda/

https://aws.amazon.com/lambda/pricing/

https://aws.amazon.com/architecture/well-architected/

https://docs.aws.amazon.com/lambda/latest/dg/lambda-permissions.html

https://aws.amazon.com/serverless/

https://aws.amazon.com/blogs/architecture/understanding-the-different-ways-to-invoke-lambda-functions/

https://docs.aws.amazon.com/lambda/latest/dg/invocation-scaling.html

https://aws.amazon.com/xray/

## AWS Data Sync
- Used to move large amounts of data from on-prem to AWS
- Used with NFS and SMB-compatible file systems
- Replication can be done hourly, daily, or weekly
- Need to isntall Data Sync agent
- Can be used to replicate EFS to EFS

## CloudFront
- AWS caching service
- Edge Location
    - Where content will be cached
    - Are NOT just read-only (can write to it too)
- Origin/Source of CDN caching objects: S3, EC2 instance, ELB or R53
- Distribution: CDN's name (collection of Edge locations)
    - Web Distribution: typically used for web sites
    - RTMP: Used for Media Streaming
- Objects are cached for the life of TTL
- You can clear objects yourself, but you'll be charged
- CloudFront Signed URLs and Cookies
    - Used when to want to secure content so that only the people you authorize are able to access it
    - A signed URL is for an individual file -> 1 file = 1 URL
    - A signed cookie is for multiple files -> 1 cookie = multiple files
    - Origin is EC2 -> Use CloudFront (if it's S3, use S3 signed URL)
    
## Event Processing Patterns
- Understand pub/sub pattern (facilitaded by SNS)
- DLQ - SNS, SQS, Lambda
- Fanout Pattern - SNS
- S3 Event Notifications - which events trigger; which services consume
    
## AWS Security
- KMS
    - Manager customer master keys (CMKs)
- CloudHSM
    - Regulatory Compliance Requirements
    - FIPS 140-2 Level 3
- Systems Manager Parameter Store
    - xxx
- Secrets Manager
    - Charge per secret stored and per 10,000 API calls
- Automatically rotate secrets
- Apply the new key/password in RDS for you
- Generate random secrets

### AWS Shield
- Protects against DDoS
- AWS Shield Standard
    - Automatically enable for all customers at no cost with WAF
    - Protects against common layer 3 and 4 attacks
        - SYN/UDP floods
        - Reflection attacks
- AWS Shield Advanced
    - USD 3,000 per month
    - Enhanced protection for EC2, ELB, CloudFront, Global Accelerator, R53
    - Business and Enterprise support customers get 24x7 access to DDoS Response Team (DRT)
    - DDoS cost protection
    
### AWS WAF
- Web app firewall that lets you monitor HTTPS requests to CloudFront, ALB and API Gateway
- Control access to content
- Configure filtering rules to allow/deny traffic
    - IP addresses
    - Query string params
    - SQL query injection
- Blocked traffic returns HTTP 403 forbidden

#### How does AWS WAF work
- Allow all traffic, except what you specify
- Deny all traffic, except what you specify
- Count requests that match the properties you specify
- Request properties:
    - Originating IP Address
    - Originating country
    - Request size
    - Values in request headers
    - Strings in request matching regex 
    - SQL code injection
    - Cross-site scripting
    
#### AWS Firewall Manager
- Centrally configure and manage firewall rules across an AWS Organization
- WAF rules:
    - ALB
    - API GTW
    - CloudFront distributions
- AWS Shield Advanced protections:
    - ALB
    - ELB Classic
    - EIP
    - CloudFront distributions
- Enable Sec Groups for EC2 and ENIs

### AWS ECS
- Managed container orchestration service

### AWS EKS
- Managed container orchestration service (Kubernetes)

