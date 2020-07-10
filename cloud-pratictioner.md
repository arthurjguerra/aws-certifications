# [Cloud Practioner](https://aws.amazon.com/certification/certified-cloud-practitioner/)

## Table of Contents

- [What do you need to know to pass the Certified Cloud Practitioner Exam?](#what-do-you-need-to-know-to-pass-the-certified-cloud-practitioner-exam)
- [Introduction to AWS Cloud](#introduction-to-aws-cloud)
    * [AWS Global Infrastructure](#aws-global-infrastructure)
- [AWS Services](#aws-services)
    * [S3](#s3)
    * [CloudFront](#cloudfront)
    * [EC2](#ec2)
    * [EBS](#ebs)
    * [VPC](#vpc)
    * [Load Balancer](#load-balancer)
    * [Auto Scaling](#auto-scaling)
    * [Route 53](#route-53)
    * [RDS](#rds)
    * [AWS Lambda](#aws-lambda)
    * [AWS Elastic Beanstalk](#aws-elastic-beanstalk)
    * [SNS](#sns)
    * [CloudWatch](#cloudwatch)
    * [CloudFormation](#cloudformation)
    * [AWS Services Deployed On Premise](#aws-services-deployed-on-premise)
- [Billing and Pricing](#billing-and-pricing)
- [Security in the Cloud](#security-in-the-cloud)
    * [AWS Compliance & AWS Artifact](#aws-compliance--aws-artifact)
    * [Shared Responsibility Model](#shared-responsibility-model)
    * [AWS WAF & AWS Shield](#aws-waf--aws-shield)
    * [AWS Inspector vs AWS Trusted Advisor vs CloudTrail](#aws-inspector-vs-aws-trusted-advisor-vs-cloudtrail)
    * [CloudWatch vs AWS Config](#cloudwatch-vs-aws-config)
    * [Athena Vs Macie](#athena-vs-macie)


## What do you need to know to pass the Certified Cloud Practitioner Exam?
- AWS Global Infrastructure
- Compute
- Storage
- Databases
- Security, Identity and Compliance
- AWS Cost Management

### 6 Advantages of the Cloud
- Go global in minutes
- Stop spending money running and maintaining data centers
- Increase speed and agility
- Stop guessing about capacity
- Benefit from massive economies of scale
- Trade capital expense for variable expense

## Introduction to AWS Cloud
- [AWS_Cloud_Best_Practices.pdf](https://d1.awsstatic.com/whitepapers/AWS_Cloud_Best_Practices.pdf)
- [AWS-Overview.pdf](https://d0.awsstatic.com/whitepapers/aws-overview.pdf)
- Cloud Computing
    - On demand delivery of resources and applications over the Internet
- Use services at your own pace
- Resize your resources when necessary
- AWS data centers spreaded across the globe
    - Cheaper than having your own data centers close to where all your customers are located
- AWS provides elastic infrastructure
    - Scale up and dow according to your needs
- 3 different ways to use AWS:
    - AWS Management Console (GUI)
    - AWS CLI 
        - Access to services via command line
    - AWS SDK
        - Ability to use AWS using programming languages such as Python, Java, Node.js, and Go

### AWS Support Plans
- [Support Plans](https://aws.amazon.com/premiumsupport/plans/)
    - Basic
        - AWS Forums
    - Developer
        - 12-24-hour response rates
    - Business
        - 24x7 support by phone and chat (1-hour response rate)
    - Enterprise
        - TAM
        - Full access to Trusted Advisor
        
### AWS Global Infrastructure
- https://aws.amazon.com/about-aws/global-infrastructure/
- Region: Composed of two or more availability zones
    - Not all services are available in all regions
- Availability Zone = data center
    - Physically and logically separated
- Edge Location
    - Endpoints for caching location (CDN)
- Fault Tolerance
    - System remains operational even if some components of that system fail
- High Availability
    - Ensures your system is always functioning and accessible
    - Max up time without manual intervention
- Security
    - Monitors IT resources continuously
    - Services meet the strictest requirements
    

## AWS Services

### S3
- [FAQs](https://aws.amazon.com/s3/faqs/)
- Fully managed storage service
- Unlimited number of objects
- Files can be from 0B to 5TB stored in Buckets
- Buckets have unique name globally
- S3 can be used to host static websites
- Objects consist of
    - **Key**: name of the object (usually defined as a file path)
    - **Value**: actual data
    - Version ID
    - Metadata
    - Sub-resources: ACLs and Torrent
- Data Consistency
    - **Read after write** for PUTS of new objects
    - **Eventual consistency** for overwrite PUTS and DELETES
- Access data anywhere:
    - AWS Console
    - CLI
    - SDKs
    - Direct Access to the bucket itself
        - URL-based access
        - https://<BUCKET_NAME>/<REGION_SPECIFIC_ENDPOINT>/<OBJECT_KEY>

#### S3 Storage Classes
- S3 Standard
- S3 Infrequently Accessed
- S3 One Zone Infrequently Accessed
- S3 Glacier
- S3 Glacier Deep Archive
- S3 Intelligent Tiering
    
#### S3 Pricing
- In S3, AWS charges you for:
    - *Storage*: Number and size of objects
    - *Requests*: different rates for GET requests
    - *Storage Management*: standard, standard-infrequent access, etc
    - *Data Transfer*: amount of data transferred out of S3
    - *Transfer Acceleration*: 
        - Objects are uploaded to Edge locations and then they are transferred over AWS backbone to the actual bucket

#### Restricting Bucket Access
- Bucket Policies
    - Applies across the whole bucket
- Object Policies
    - Applies to individual files
- IAM Policies to Users & Groups
    - Applies to individual users and groups
    

### CloudFront
- Content Deliver Network (CDN)
- Content will be cached on Edge Locations
    - You can read (CDN) and write (transfer acceleration) to Edge Locations
- Origin
    - This is the origin of all files that the CDN will distribute (S3, EC2, ALB, R53)
- Distribution
    - Name of the CDN
    - Consists of a collection of Edge Locations
    - Types
        - **Web Distribution**:  Static-asset caching
        - **RTMP**: Live and on-demand streaming
- Objects are cached for the TTL
    - Cache can be cleared but you will be charged
- Used to deliver both static and dynamic content
- Content NOT necessarily has to be in AWS

#### CloudFront Pricing
- Princing varies across geographic regions
- Pricing based on: requests and data transfer out

### EC2
- EC2 - Elastic Compute Cloud
    - Increase or decrease the # of instances your app needs
- Pay as you go
- Global hosting across AWS regions

#### EC2 Pricing
- Resources incur charges only when running
- Instance configuration
    - Pricing varies: region, OS, instance type, instance size
- On-demand instance
    - Pay for compute capacity by the hour and second (min of 60s)
- Reserved Instance
    - Capacity reservation with significant discount on the hourly charge (1-3-year contract)
    - Useful for applications with predictable usage
- [Spot Instances](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/how-spot-instances-work.html)
    - If your maximum price exceeds the current Spot price and capacity is available, your request is fulfilled 
        immediately
    - If the Spot price exceeds your maximum price, when the demand for Spot Instances rises, or when the supply 
        of Spot Instances decreases, EC2 can stop your instances
    - Cost-effective choice if you can have flexible start and end times
- Dedicated Instances
    - Physical EC2 servers dedicated for your use

### EBS
- Storage unit of EC2 instances
    - Can be HD or SSD devices
- Replicated in the same AZ
- Volumes are located in AZs and can only be attached to EC2 in the same AZ
- If a volume is detached from an instance, it'll stay in the *available* state
- Snapshots
    - You can recreate volumes from snapshots
- Encrypted Volumes
    - Encrypted in transit (inside AWS data center)

#### EBS Pricing
- Pay for GB per month
- Types
   - General purpose: included in price
   - Provisioned IOPS: charged by the amount you provision in IOPS
   - Magnetic: charged by the number of requests
- Snapshots
   - Added cost per GB per month of data stored
   - Inbound data transfer is free
   - Outbound data transfer are tiered

### VPC
- Private networking within AWS cloud
- Allows complete control of network configuration
- Offers several layers of security controls
    - Allow and deny specific internet and internal traffic
- Other AWS services that can be deployed into a VPC
    - Those services will inherent security built into network

#### VPC Features
- Builds upon Regions and AZs
    - Multiple VPCs per account
- Subnets
    - Divide a VPC
- Route Tables
    - Control traffic going out of the subnets
- Internet Gateway
    - Allows access to the internet from the VPC
- NAT gateway
    - Allows private subnet resources to access Internet
- Network Access Control List (NACL)
    - Control access to subnets; stateless

#### VPC Security Groups
- Filters traffic to your instances and other resources in a VPC via SG rules
- By default, all inbound traffic is denied and all outbound traffic is allowed

### Load Balancer
- 3 types: Application, Network and Classic
- ALB
    - Enhanced features when compared with Classic LB
        - Path and host-based routing
        - WAF integration
        - Dynamic Ports
        - Deletion, protection and request tracing
        - Native IPv6 Support
        - HTTP/2 and WebSockets support
    - Requires at least 2 AZs
    - Use Case
        - Ability to use containers for your microservices
        - ALBs allow you to distribuite traffic across a set of instances based on the request path
    - Key Terms
        - Listeners
            - Process that checks for connection requests using the protocol and port that you configure
        - Targets
            - Destination for target based on the listener rules
            - Targets can be member of multiple target groups
        - Target Groups
            - Routes requests to one or more registered targets using the protocol and port specified
    - How to create an ALB
        - Configure Load Balancer
            - name, schema (internet-facing or not), listeners - protocols and ports, VPC and AZs, and tags 
        - Configure Security Settings
        - Configure Security Groups
        - Configure Routing
            - Allows you to configure routing rules for the backend destination of the ALB
            - Where you define a target group and health check
        - Register Targets
            - Tell the load balancer what instance you want that port to be hit

### Auto Scaling
- Ensure that you have the correct number of EC2 instances to handle the load of your application
- Adjust capacity as needed
- Scaling Out and Scaling In
    - Out: add more boxes
    - In: terminate instances
- How do you Auto Scale?
    - Need a Launch Configuration
        - **What** will be launched? 
        - AMI, instance type, SGs, roles
    - Need an Auto Scaling Group
        - **Where** will it be deployed? 
        - VPC, ALB, max and minimum # of instances, and capacity
    - Auto Scaling Policy
        - **When** to launch or terminate instances
        - Scheduled, on-demand, Scale-out policy and scale-in policy
- CloudWatch Alarm for Auto Scaling
    - CPU utilization for a specified period of time
    - CPU is >= 80% for 10 minutes, CloudWatch then triggers auto scaling automatically to scale out the app

### Route 53
- Global, high available DNS service
- Offers different resolution strategies:
    - Geo-location
    - Failover
    - Weighted Round Robin
    - Latency-based
    - Multi-value answer

### RDS
- Cost-efficient and resizable capacity for your database: SQLServer, MySQL, PSQL,
- Multi-AZ deployments
    - Ideal for high availability
    - If master in a zone fails over to the other instance running on another zone
- Read Replicas
- Manages
    - OS installation and patching
    - DB SW installation and patching
    - DB backups
    - High Availability
    - Scaling
    - Server Maintenance (zero downtime)

#### RDS Pricing
- Resources incur charges when running
- Charges vary from engine, size, and memory class
- Purchase Types:
    - On-demand: charged by the hour
    - Reserved: upfront payment (discount)
- No additional for backup storage if DB instance is running
- Deployment types:
    - Single AZ
    - Multi-AZ

### AWS Lambda
- AWS managed service that allows you to run code without worring about any servers
- Auto-scaling, monitoring and logging
- Can respond to HTTP requests
- Can be triggered by events or other AWS Lambda functions

### AWS Elastic Beanstalk
- Deploy and manage apps in the cloud without worrying about the infrastructure
- Upload your code and Elastic Beanstalk automatically handles the of capacity provisioning,
  load balancing, scaling and app health monitoring

### SNS
- Pub/sub messaging
- Mobile Notification

### CloudWatch
- Monitoring Performance
    - Monitor events every 5 minutes by default (detailed monitoring: 1 min intervals)
- Can create alarms which trigger notifications
- Metrics, Alarms and Actions
    - CW Watches a single metric (e.g. CPU utilization > 60% for 5+ minutes)
    - CW Performs one or more actions
        - Based on the value of the metric relative to a threshold over a # of time periods
    - An action can be
        - EC2 action: start, stop or terminate instances
        - Auto Scaling action
        - Notification
- CW logs
    - Monitor and troubleshoot systems and apps using existing log files
    - Monitor logs from EC2 in real-time
    - Monitor CloudTrail logged events
    - Archive log data (S3)
- CW Dashboards
    - Customizable home pages in CW console to monitor your resources in a single view
    - Customized view of the metrics and alarms for your AWS resources

### CloudFormation
- Automate resource provisioning
- Create, update and delete resources in stacks (sets of resources)
- Template File -> CloudFormation -> Stack
    - Template
        - Resources to provision
        - Text file (JSON or YAML)
        - Infra as code
    - Stack
        - Unit of deployment
        - Where resources are generated
        - CRUD

### AWS Services Deployed On Premise
- Snowball
- Snowball Edge (allows lambda deployments)
- Storage Gateway
    - Caching files in your data center and replicate them to S3
- Code Deploy
    - Used to deploy apps on premises
- OpsWorks
    - Deploy app code to the cloud and on premise using Chef
- IoT Greengrass

### AWS Systems Manager
- Manage fleets of EC2 instances
- Can be both inside AWS and on premise
- Run Command is used to install, patch and uninstall software
- Integrates with CloudWatch to give you a dashboard

## Billing and Pricing
- [AWS Pricing Overview](https://d0.awsstatic.com/whitepapers/aws_pricing_overview.pdf)
- [The Total Cost of (Non) Ownership of
   Web Applications in the Cloud](https://media.amazonwebservices.com/AWS_TCO_Web_Applications.pdf)
- Pay for: compute capacity, storage, outbound data transfer
- No charge for inbound data transfer

### AWS Organizations

## Security in the Cloud

### AWS Compliance & AWS Artifact
### Shared Responsibility Model
### AWS WAF & AWS Shield
### AWS Inspector vs AWS Trusted Advisor vs CloudTrail
### CloudWatch vs AWS Config
### Athena Vs Macie

### IAM
- Global
- Group
    - Place to store users
    - Users will inherit all permissions that the group has
- To set the permissions in a group you need to apply a policy to that group
    - Policies consist of JSON
- Root Account
    - Email address used to set up your AWS account
- Role
    - Changes take place immediately

### Amazon Inspector

### Security Compliance

### AWS Trusted Advisor
- Keep track of your AWS resources
- 4 Checks:
    - Cost Optimizations
    - Performance
    - Security
    - Fault Tolerance
    
### The AWS Well-Architected Framework
- 5 pillars and design principles
    - Security
    - Reliability
    - Performance Efficiency
    - Cost Optmization
    - Operational Excellence
- Security Pillar
    - IAM
        - Ensure authorized and authenticated users are able to access your resources
    - Detective Controls
        - Identity potential security incident
    - Infrastructure protection
        - Systems and services are protected against unintended and unauthorized access
    - Data Protection
        - Encryption, backup, replication and recovery
    - Incident response
        - Response and mitigate any potential security incidents
    - Design Principles
        - Implement securities at all layers
        - Enable traceability: logging and auditing for all actions
        - Apply principle of least privilege
        - Focus on securing your system: shared responsibility model
        - Automate
- Reliability Pillar
    - Recover from failures
    - Apply best practices on:
        - Foundation
        - Change Mgmt
        - Failure Mgmt
    - Anticipate, respond and prevent failures
    - Design Principles
        - Test recovery procedures
        - Automatically recover
        - Scale Horizontally
        - Stop guessing capacity 
            - You can monitor demand and system utilization
            - Automation the addition or removal of resources
        - Manage change in automation
- Performance Efficiency Pillar
    - Select customizable solutions
    - Review to continually innovate
    - Monitor services
    - Consider the trade-offs
    - Design Principles
        - Democratize advance technologies: consume them as a service
        - Go global in minutes
        - Serverless
        - Experiment more often
        - Have mechanical sympathy
- Cost Optimization Pillar
    - Use cost-effective resources: Build and operation cost-aware systems
    - Matching supply with demand: elasticity of resouces (auto-scale)
    - Increase expenditure awareness: plan future changes
    - Optimize over time: mesure, monitor and apply changes based on the data you collected from your resources
    - Design Principles
        - Adopt a consumption model: pay for what you use
        - Measure overall efficiency: measure the costs of your infra
        - Reduce spending on data center operations
        - Analyze and attribute expenditure: simpler and easier to accurately identify the usage and cost of systems
        - Use managed services
    - Operational Excellence Pillar
        - Manage and automate changes
        - Respond to events
        - Define the standards

### Fault Tolerance and High Availability Architectures
- Fault Tolerance
    - Ability for a system to remain function after a failure
    - Service Tools
        - SQS: distributed messaging system
        - S3: highly durable data storage
        - RDS: setup and scale relational DBs (automated backups, snaphots and multi-zone deployments)
- High Availability
    - Systems are generally up and running
    - On AWS:
        - Multiple Servers
        - Zones
        - Regions
        - Fault-tolerant Services
    - Service Tools
        - ELBs
            - Distributes incoming traffic among your instances
            - Can be customized
        - Elastic IP
            - Static IP addresses
            - Designed for dynamic cloud computing
        - Route 53
            - Authorative DNS service
            - Supports:
                - Simple routing
                - Latency-based routing
                - Health Checks
                - DNS failovers
                - Geolocation routing
        - Auto Scaling
            - Terminates and launches instances on demand automatically
            - Scale in or out depending on your policies
        - CloudWatch
            - Distributed Statistics gathering system
            - Tracks metrics of your resources