# What Is AWS
On-premises and cloud computing
# AWS Region considerations
1. Compliance 灵活性
2. Latency 延迟
3. Pricing
4. Service availability
# Interacting with AWS
AWS products or services

# What’s the big deal about auth?
authentication: When you create your AWS account, you use the combination of an email address and a password to verify your identity. If a user types in the correct email and password, the system assumes the user is allowed to enter and grants them access. This is the process of authentication.
authorization: Once you’re authenticated and in your AWS account, you might be curious about what actions you can take. This is where authorization comes in. Authorization is the process of giving users permission to access AWS resources and services. Authorization determines whether a user can perform certain actions, such as read, edit, delete, or create resources. Authorization answers the question, “What actions can you perform?” 

## Multi-factor authentication (MFA)
MFA requires two or more authentication methods to verify an identity. MFA pulls from the following three categories of information:

1. Something you know, such as a user name and password, or pin number
2. Something you have, such as a one-time passcode from a hardware device or mobile app
3. Something you are, such as fingerprint or face scanning technology

Using a combination of this information enables systems to provide a layered approach to account access. So even if the first method of authentication, like Bob’s password, is cracked by a malicious actor, the second method of authentication, such as a fingerprint, provides another level of security. This extra layer of security can help protect your most important accounts, which is why you should enable MFA on your AWS root user.
## MFA on AWS
If you enable MFA on your root user, you must present a piece of identifying information from both the something you know category and the something you have category. The first piece of identifying information the user enters is an email and password combination. The second piece of information is a temporary numeric code provided by an MFA device.

Enabling MFA adds an additional layer of security because it requires users to use a supported MFA mechanism in addition to their regular sign-in credentials. Enabling MFA on the AWS root user account is an AWS best practice.
# IAM
AWS Identity and Access Management (IAM) is an AWS service that helps you manage access to your AWS account and resources. It also provides a centralized view of who and what are allowed inside your AWS account (authentication), and who and what have permissions to use and work with your AWS resources (authorization).
## IAM user

An IAM user represents a person or service that interacts with AWS. You define the user in your AWS account. Any activity done by that user is billed to your account. Once you create a user, that user can sign in to gain access to the AWS resources inside your account.

You can also add more users to your account as needed. For example, for your cat photo application, you could create individual users in your AWS account that correspond to the people who are working on your application. Each person should have their own login credentials. Providing users with their own login credentials prevents sharing of credentials.

## Consider the following examples:

A new developer joins your AWS account to help with your application. You create a new user and add them to the developer group, without thinking about which permissions they need.
A developer changes jobs and becomes a security engineer. Instead of editing the user’s permissions directly, you remove them from the old group and add them to the new group that already has the correct level of access.
## Keep in mind the following features of groups:

Groups can have many users.
Users can belong to many groups.
Groups cannot belong to groups.
```{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "cloudwatch:DescribeAlarms*",
                "ec2:Describe*",
                "ec2:StartInstances",
                "ec2:StopInstances",
            ],
            "Resource": "*"
        }
    ]
}
```
# EC2 = Elastic Compute Cloud = Infrastructure as a Service
## Create Instance
> User data script:
```
#!/bin/bash
#Use this for your user data (script from top to bottom)
#install httpd (Linux 2 version)
yum update -y
yum install -y httpd
systemctl start httpd
systemctl enable httpd
echo "<h1>Hello World from $(hostname -f)<h1>/var/www/html/index.html
```
只选SSH和http.
1. It mainly consists in the capability of:
2. Renting vitual machines(EC2)
3. Storing data on virtual drives(EBS)
4. Scaling the services using an auto-scaling group(ASG)

> public IP port may change when you restart the instance

## EC2 User Data
It is possible to bootstrap our instances using an EC2 User Data Script.
Bootstrapping means launching commands when a machine starts.
The script is only run once at the instance first start
#### EC2 user data is used to automate boot task such as:
* Installing updates
* Installing software
* Downloading common files from the Internet
* Anything you can think of.
> The EC2 User Data Script runs with the root user.
Keypair use for SSH
## EC2 Instance Types
* m5.2xlarge
* m:instance class
* 5:generation(AWS improves them over time)
* 2xlarge:size within the instance class
Balance between:
* Compute
* Memory
* Networking
EC2 instance comparison
https://instances.vantage.sh/
## Classic Ports to know
* 22 = SSH(Secure Shell) - log into a Linux instance
* 21 = FTP(File Transfer Protocol) - upload files into a file share
* 22 = SFTP(Secure File Transfer Protocol) - upload files using SSH
* 80 = HTTP - access unsecured websites
* 443 = HTTPS - access secured websites
* 3389 = RDP(Remote Desktop Protocol) - log into a Windows instance
# SSH
|               | SSH | Putty | EC2 instance connect |
|:-------------:|:---:|:-----:|:--------------------:|
| Mac           | Y   |       | Y                    |
| Linux         | Y   |       | Y                    |
| Windows >= 10 | Y   | Y     | Y                    |
| Windows < 10  |     | Y     | Y                    |

## How to SSH into your EC2 Instance using Linux / Mac OS X
![Screenshot 2023-02-06 at 12.09.20 PM](assets/Screenshot%202023-02-06%20at%2012.09.20%20PM.png)
![Screenshot 2023-02-06 at 12.09.36 PM](assets/Screenshot%202023-02-06%20at%2012.09.36%20PM.png)
# EC2 Instances Purchasing Options
1. On-Demand Instances - short workload, predictable pricing, pay by second
2. Reserved(1&3 years)
*     Reserved Instances - long workloads
*     Convertible Reserved Instances - long workloads with flexible instan
3. Saving Plans(1&3 years) -commitment to an amount of usage, long workload
4. Spot Instances - short workloads, cheap, can lose instances(less reliable)
5. Dedicated Hosts - book an entire physical server, control instance placement
6. Dedicated Instances - no other customers will share your hardware
7. Capacity Reservations - reserve capacity in a specific AZ for any duration
# Elastic IP
* With an Elastic IP address, you can mask the failure of an instance or software by rapidly remapping the address to another instance in your account.
* You can only have 5 Elastic IP in your account(you can ask AWS to increase that)
* Overall, try to avoid using Elastic IP, they often reflect poor architectural decisions. Instead, use a random public IP and register a DNS name to it. Or, use a Load Balancer and don't use a public IP.
# ENI Elastic Network Interfaces
Logical component in a VPC that represents a virtual network card
The ENI can have the following attributes:
    Primary private IPv4, one or more secondary IPv4
    One Elastic IP(IPv4) per private IPv4
    One Public IPv4
    One or more security groups
    A MAC address
You can create ENI independently and attach them on the fly (move them) on EC2 instances for failover.
Bound to a specific availability zone (AZ)
# EC2 Hibernate
1. Introducing EC2 Hibernate:
    The in-memory(RAM) state is preserved
    The instance boot is much faster!(the OS is not stopped / restarted)
    Under the hood: the RAM state is written to a file in the root EBS volume
    The root EBS volume must be encrypted
2. Use cases:
    Long-running processing
    Saving the RAM state
    Services that take time to initialize
3. Good to know
    Supported Instance Families - C3,C4,C5,I3,M3,M4,R3,R4,T2,T3....
    Instance RAM Size - must be less than 150GB
    Instance Size - not supported for bare metal instances.
    AMI - Amazon Linux 2, Linux AMI, Ubuntu, RHEL, CentOS & Windows....
    Root Volume - must be EBS, encrypted, not instance store, and large.
    Available for On-Demand, Reserved and Spot Instances
    An instance can NOT be hibernated more than 60 days.
# EC2 Instance Storage Section
## EBS Volume
An EBS (Elastic Block Store) Volume is a network drive you can attach to your instances while they run
It allows your instances to persist data, even after their termination.
They can only be mounted to one instance at a time (at the CCP level)
> Note: CCP - Certified Cloud Practitioner - one EBS can be only mounted to one EC2 instance
> Associate Level (Solutions Architect, Developer, SysOps): "multi-attach" feature for some EBS
They are bound to a specific availability zone
Analogy: Think of them as a "network USB stick"
# Solution Architecture
# S3 Encryption
## SSE-S3
encryption using keys handled & managed by Amazon S3
Object is encrypted server side
AES-256 encryption type
Must set header: "x-amz-server-side-encryption":"AES256"
## SSE-KMS
encryption using keys handled & managed by KMS
KMS advantages: user control + audit trail
Object is encrypted server side
Must set header: "x-amz-server-side-encryption":"aws:kms"
## SSE-C
server-side encryption using data keys fully managed by the customer outside of AWS
Amazon S3 does not store the encryption key you provide
HTTPS must be used
Encryption key must provided in HTTP headers, for every HTTP request made
## Client Side Encryption
Client library such as the Amazon S3 Encryption Client
Clients must encrypt data themselves before sending to S3
Clients must decrypt data themselves when retrieving from S3
Customer fully manages the keys and encryption cycle
# Encryption in transit (SSL/TLS)
Amazon S3 exposes:
    HTTP endpoint: non encrypted
    HTTPS endpoint: encryption in flight
You're free to use the endpoint you want, but HTTPS is recommended
Most clients would use the HTTPS endpoints by default
HTTPS is mandatory for SSE-C
Encryption in flight is also called SSL/TLS

Note: an IAM principal can access an S3 object is
    the user IAM permissions allow it OR the resource policy ALLOWS it.
    AND there's no explicit DENY. 
# CORS
An origin is a scheme (protocol), host(domain) and port
    implied port is 443 for HTTPS, 80 from HTTP
CORS means Cross-Origin Resource Sharing
Web Browser based mechanism to allow requests to other origins while visiting the main origin 

IAM Policy Simulator
https://policysim.aws.amazon.com/home/index.jsp?#roles/MyFirstEC2Role
# AWS EC2 Instance Metadata
It is powerful but one of the least known features to developers
It allows AWS EC2 instance to "learn about themselves" without using an IAM Role for that purpose
The URL is http://169.254.169.254/latest/meta-data
You can retrieve the IAM Role name from the metadata, but you CANNOT retrieve the IAM Policy.
Metadata = Info about the EC2 instance
Userdata = launch script of the EC2 instance
# AWS SDK
We have to use the AWS SDK when coding against AWS Services such as DynamoDB
# Advanced S3
## S3 MFA-Delete
MFA (multi factor authentication) forces user to generate a code on a device (usually a mobile phone or hardware) before doing important operations on S3.
To use MFA-Delete, enable Versioning on the S3 bucket.
You will need MFA to
    permanently delete an object version
    suspend versioning on the bucket
You won't need MFA for
    enabling versioning
    listing deleted versions
Only the bucket owner (root account) can enable/disable MFA-Delete
MFA-Delete currently can only be enabled using the CLI
## S3 Access Logs: Warning
Do not set your logging bucket to be the monitored bucket
It will create a logging loop, and your bucket will grow in size exponentially
## S3 Byte-Range Fetches
Parallelize GETs by requesting specific byte ranges
Better resilience in case of failure
Can be used to speed up downloads
Can be used to retrieve only partial data (for example the head of a file)
# CloudFront
Content Delivery Network (CDN)
Improves read performance, content is cached at the edge
216 Point of Presence globally (edge locations)
DDoS protection, integration with Shielf, AWS Web Application Firewall
Can expose external HTTPS and can talk to internal HTTPS backends
## AWS Global accelerator
Unicast IP: one server holds one IP address
Anycast IP: all servers hold the same IP address and the client is routed to the nearest one
# AWS Snow Family
Highly-secure, portable devices to collect and process data at the edge, and migrate data into and out of AWS
Data migration: Snowcone, Snowball Edge, Snowmobile
Edge computing: Snowcone, Snowball Edge
## Sloution Architecture: Snowball into Glacier
Snowball cannot import to Glacier directly
You must use Amazon S3 first, in combination with an S3 lifecycle policy
# Amazon FSx
Launch 3rd party high-performance file systems on AWS
Fully managed service
FSx for Lustre, FSx for NetApp ONTAP, FSx for Windows File Server, FSx for OpenZFS
## Hybrid Cloud for Storage
AWS is pushing for "hybrid cloud"
    Part of your infrastructure is on the cloud
    Part of your infrastructure is on-premises
This can be due to
    Long cloud migrations
    Security requirements
    Compliance requirements
    IT strategy
S3 is a proprietary storage technology (unlike EFS / NFS), so how do you expose the S3 data on-premises?
    AWS storage gateway
## AWS Storage Cloud Native Options
Block: Amazon EBS, EC2 Instance Store
File: Amazon EFS, Amazon FSx
Object: Amazon S3, Amazon Glacier
## AWS Storage Gateway
Bridge between on-premises data and cloud data
Use cases:
    disaster recovery
    backup & restore
    tiered storage
    on-premises cache & low-latency files access
Types of Storage Gateway:
    S3 File Gateway
    FSx File Gateway
    Volume Gateway
    Tape Gateway
## AWS Transfer Family
A fully-managed service for file transfers into and out of Amazon S3 or Amazon EFS using the FTP protocol
Support Protocols
    AWS Transfer for FTP (File Transfer Protocol (FTP))
    AWS Transfer for FTPS (File Transfer Protocol over SSL (FTPS))
    AWS Transfer for SFTP (Secure File Transfer Protocol (SFTP))
Managed infrastructure, Scalable, Reliable, Highly Available (multi-AZ)
Pay per provisioned endpoint per hour + data transfers in GB
Store and manage users' credentials within the service

## AWS DataSync
Move large amount of data to and from
    on-premises/other cloud to AWS (NFS, SMB, HDFS, S3 API...)- needs agent
    AWS to AWS (different storage services) - no agent needed
Can synchronize to:
    Amazon S3 (any storage classes - including Glacier)
    Amazon EFS
    Amazon FSx (Windows, Lustre, NetApp, OpenZFS...)
Replication tasks can ebe scheduled hourly, daily, weekly
File permissions and metadata are preserved (NFS POSIX, SMB...)
One agent task can use 10Gbps, can setup a bandwidth limit
# AWS Integration and Messaging
Synchronous between applications can be problematic if there are sudden spikes of traffic
What if you need to suddenly encode 1000 videos but usually it's 10?
In that case, it's better to decouple your applications
    using SQS: queue model
    using SNS: pub/sub model
    using Kinesis: real-time streaming model
These Services can scale independently from our application
## Amazon SQS -  Standard Queue
Oldest offering (over 10 years old)
Fully managed service, used to decouple applications
Attributes:
    Unlimited throughput, unlimited number of messages in queue
    Default retention of messages: 4 days, maximum of 14 days
    Low latency (<10 ms on publish and receive)
    Limitation of 256KB per message sent
Can have duplicate messages (at least once delivery, occasionally)
Can have out of order messages (best effort ordering)
## SQS - Producing Messages
Produced to SQS using the SDK (SendMessage API)
The message is persisted in SQS until a consumer deletes it
Message retention: default 4 days, up to 14 days
Example: send an order to be processed
    Order id
    Customer id
    Any attributes you want
SQS standard: unlimited throughput(吞吐量)
## SQS - Consuming Messages
Consumers (running on EC2 instances, servers, or AWS Lambda)...
Poll(测验,调查) SQS for messages (receive up to 10 messages at a time)
## SQS - Multiple EC2 Instances Consumers
Consumers receive and process messages in parallel
At least once delivery
Best-effort message ordering
Consumers delete messages after processing them
We can scale consumers horizontally to improve throughput of processing
## Amazon SQS - Security
Encryption
    In-flight encryption using HTTPS API
    At-rest encryption using KMS keys
    Client-side encryption if the client wants to perform encryption/decryption iteself
Access Controls: IAM policies to regulate access to the SQS API
SQS Access Policies (similar to S3 bucket policies)
    Useful for cross-account access to SQS queues
    Useful for allowing other services (SNS,S3...) to write to an SQS queue
## SQS - Message Visibility Timeout
After a message is polled by a consumer, it becomes invisible to other consumers
By default, the "messge visibility timeout" is 30 seconds
That means the message has 30 seconds to be processed
After the message visibility timeout is over, the message is "visible" in SQS
## Amazon SQS - Long Polling
When a consumer requests messages from the queue, it can optionally "wait" for messages to arrive if there are none in the queue
This is called Long Polling
LongPolling decreases the number of API calls made to SQS while increasing the efficiency and latency of your application
The wait time can be between 1s to 20s (20s preferable)
Long Polling is preferable to Short Polling
Long polling can be enabled at the queue level or at the API level using WaitTimeSeconds.
## Amazon SQS - FIFO Queue
FIFO = First In First Out (ordering of messages in the queue)
Limited throughput: 300 msg/s without batching, 3000 msg/s with
Exactly-once send capability (by removing duplicates)
Messages are processed in order by the consumer
## SQS with Auto Scaling Group (ASG)
If the load is too big, some transactions may be lost
SQS as a buffer to database writes
## Amazon SNS - Simple Notification Service
The "event producer" only sends message to one SNS topic
As many "event receivers" (subscriptions) as we want to listen to the SNS topic notifications
Each subscriber to the topic will get all the message (note:new feature to filter messages)
Up to 12,500,000 subscriptions per topic
100,000 topics limit
## SNS integrates with a lot of AWS services
Many AWS services can send data directly to SNS for notification
## AWS SNS - How to publish
Topic Publish (using the SDK)
    Create a topic
    Create a subscription (or many)
    Publish to the topic
Direct Publish (for mobile apps SDK)
    Create a platform application
    Create a platform endpoint
    Publish to the platform endpoint
    Works with Google GCM, Apple APNS, Amazon ADM...
## Amazon SNS - Security
Encryption
    In-flight encryption using HTTPS API
    At-rest encryption using KMS keys
    Client-side encryption if the client wants to perform encryption/decryption iteself
    
Access Controls: IAM policies to regulate access to the SNS API

SQS Access Policies (similar to S3 bucket policies)
    Useful for cross-account access to SNS topics
    Useful for allowing other services (S3...) to write to an SNS topic
## SNS + SQS: Fan Out
Push once in SNS, receive in all SQS queues that are subscribers
Fully decoupled, no data loss
SQS allows for: data persistence, delayed processing and retries of work
Ability to add more SQS subscribers over time
Make suer your SQS queue access policy allows for SNS to write
## Application: S3 Events to multiple queues
For the same combination of: event type (e.g. object create) and prefix(e.g.images/) you can only have one S3 Event rule
If you want to send the same S3 event to many SQS queues, use fan-out
## Application: SNS to Amazon S3 through Kinesis Data Firehose
SNS can send to Kinesis and thereforee we can have the following solutions architecture:
Buying Service --> SNS Topic --> Kinesis Data Firehose --> Amazon S3
## Amazon SNS - FIFO Topic
Similar features as SQS FIFO
Can only have SQS FIFO queues as subscribers
Limited throughput (same throughput as SQS FIFO)
In case you need fan out + ordering + deduplication(重复数据删除)
## SNS - Message Filtering
JSON policy used to filter messages send to SNS topic's subscriptions
If a subscription doesn't have a filter policy, it receives every message

# Kinesis
Makes it easy to collect, process, and analyze streaming data in real-time
Ingest real-time data such as: Application logs, Metrics, Website clickstreams, IoT telemetry data...
Kinesis Data Streams: capture, process, and store data streams
Kinesis Data Firehose: load data streams into AWS data stores
Kinesis Data Analytics: analyze data streams with SQL or Apahe Flink
Kinesis Video Streams: capture, process, and store video streams
## Kinesis Data Streams
Retention(保留) between 1 day to 365 days
Ability to reprocess (replay) data
Once data is inserted in Kinesis, it can't be deleted (immutability)
Data that shares the same partition goes to the same shard (ordering)
Producers: AWS SDK, Kinesis Producer Library(KPL), Kinesis Agent
Consumers:
    Write your own: Kinesis Client Library(KCL), AWS SDK
    Managed: AWS Lambda, Kinesis Data Firehose, Kinesis Data Analytics
## Kinesis Data Streams - Capacity Modes
Provisioned mode:
    You choose the number of shards provisioned, scale manually or using API
    Each shard gets 1 MB/s in (or 1000 records per second)
    Each shard gets 2 MB/s out (classic or enhanced fan-out consumer)
    You pay per shard provisioned per hour
On-demand mode:
    No need to provision or manage the capacity
    Default capacity provisioned (4 MB/s in or 4000 records per second)
    Scales automatically based on observed throughput peak during the last 30 days
    Pay per stream per hour & data in/out per GB
## Kinesis Data Streams Security
Control access / authorization using IAM policies
Encryption in flight using HTTPS endpoints
Encryption at rest using KMS
You can implement encryption/decryption of data on client side(harder)
VPC Endpoints available for Kinesis to access within VPC
Monitor API calls using CloudTrail(云轨)
# Container
## Docker
Docker is a software development platform to deploy apps
Apps are packaged in containers that can be run on any OS
Apps run the same, regardless of where they're run
    Any machine
    No compatibility issues
    Predictable behavior
    Less work
    Easier to maintain and deploy
    Works with any language, any OS, any technology
Use cases: microservices architecture, lift-and-shift apps from on-premises to the AWS cloud...
## Docker versus Virtual Machines
Docker is "sort of" a virtualization technology, but not exactly
Resources are shared with the host => many containers on one server
## Docker Containers Management on AWS
Amazon Elastic Container Service (Amazon ECS)
    Amazon's own container platform
Amazon Elastic Kubernetes Service (Amazon EKS)
    Amazon's managed Kubernetes (open source)
## Amazon ECS - EC2 Launch Type
ECS = Elastic Container Service
Launch Docker containers on AWS = Launch ECS Tasks on ECS Clusters
EC2 Launch Type: you must provision & maintain the infrastructure (the EC2 instances)
Each EC2 Instance must run the ECS Agent to register in the ECS Cluster
AWS takes care of starting/stopping containers
## Amazon ECS - Fargate Launch Type
Launch Docker containers on AWS
You do not provision the infrastructure (no EC2 instances to manage)
It's all Serverless
You just create task definitions
AWS just runs ECS Tasks for you based on the CPU / RAM you need
To scale, just increase the number of tasks. Simple - no more EC2 instances
Amazon ECS - IAM Roles for ECS
EC2 instance Profile (EC2 Launch Type only):
    Used by the ECS angent
    Makes API calls to ECS service
    Send container logs to CloudWatch Logs
    Pull Docker image from ECR
    Reference sensitive data in Secret Manager or SSM Parameter Store
ECS Task Role:
    Allows each task to have a specific role
    Use different roles for the different ECS Services you run
    Task Role is defined in the task definition
## Amazon ECS - Load Balancer Integrations
* Application Load Balancer supported and works for most use cases
* Network Load Balancer recommended only for high throughput / high performance use cases, or to pair it with AWS Private Link
* Elastic Load Balancer supported but not recommended (no advanced features - no Fargate)
## Amazon ECS -  Data Volumes (EFS)
Mount EFS file systems onto ECS tasks
Works for both EC2 and Fargate launch types
Tasks running in any AZ will share the same data in the EFS file system
Fargate + EFS = Serverless
Use cases: persistent multi-AZ shared storage for your containers
Note:
    Amazon S3 cannot be mounted as a file system
## ECS Service Auto Scaling
Automatically increase/decrease the desired number of ECS tasks
Amazon ECS Auto Scaling uses AWS Application Auto Scaling
    ECS Service Average CPU Utilization
    ECS Service Average Memory Utilization - Scale on RAM
    ALB Request Count Per Target - metric coming from the ALB
Target Tracking - scale based on target value for a specific CloudWatch metric
Step Scaling - scale based on a specified CloudWatch Alarm
Scheduled Scaling - scale based on a specified date/time (predictable changes)

ECS Service Auto Scaling (task level) ≠ EC2 Auto Scaling (EC2 instance level)
Fargate Auto Scaling is much easier to setup (because Serverless)
## EC2 Launch Type - Auto Scaling EC2 Instances
Accommodate ECS Service Scaling by adding underlying EC2 Instances

Auto Scaling Group Scaling
    Scale your ASG based on CPU Utilization
    Add EC2 instances over time

ECS Cluster Capacity Provider
    Used to automatically provision and scale the infrastructure for your ECS Tasks
    Capacity Provider paired with an Auto Scaling Group
    Add EC2 Instances when you're missing capacity (CPU, RAM...)

# Serverless
Initially... Serverless == Faas(Function as a Service)
Serverless was pioneered by AWS Lambda but now also includes anything that's managed: "database, messaging, storage, etc."

Serverless does not mean there are no servers...
it means you just don't manage / provision / see them
# Serverless in AWS
AWS Lambda
DynamoDB
AWS Cognito
AWS API Gateway
Amazon S3
AWS SNS & SQS
AWS Kinesis Data Firehose
Aurora Serverless
Step Functions
Fargate
## AWS Lambda
Virtual functions - no servers to manage
Limited by time - short executions
Run on-demand(按需)
Scaling is automated
## Benefits of AWS Lambda
Easy Pricing:
    Pay per request and compute time
    Free tier of 1,000,000 AWS Lambda requests and 400,000 GBs of compute time
Integrated with the whole AWS suite of services
Integrated with many programming languages
Easy monitoring through AWS CloudWatch
Easy to get more resources per functions (up to 10GB of RAM)
Increasing RAM will also improve CPU and network
## AWS Lambda language support
Node.js (Javascript)
Python
Java (Java 8 compatible)
C# (.Net Core)
Golang
C# / Powershell
Ruby
Custom Runtime API (community supported, example Rust)

Lambda Container Image
    The container image must implement the Lambda Runtime API
    ECS / Fargate is preferred for running arbitrary Docker images
## AWS Lambda Integrations
Main ones
API Gateway, Kinesis, DynamoDB, S3, CloudFront, CloudWatch Events EventBridge, CloudWatch Logs, SNS, SQS, Cognito
## AWS Lambda Limits to Know - per region
Execution:
    Memory allocation: 128MB - 10GB (1MB increments)
    Maximum execution time: 900 seconds (15 minutes)
    Environment variable (4KB)
    Disk capacity in the "function container"(in/tmp):512MB to 10GB
    Concurrency executions:1000 (can be increased)
Deployment:
    Lambda function deployment size (compressed.zip): 50MB
    Size of uncompressed deployment (code + dependencies): 250MB
    Can use the /tmp directory to load other files at startup
    Size of environment variable: 4KB
## Lambda by default
By default, your Lambda function is launched outside your own VPC (in an AWS-owned VPC)
Therefore, it cannot access resources in your VPC (RDS, ElasticCache, internal ELB...)

**Lambda in VPC**
You must define the VPC ID, the Subnets and the Security Groups
Lambda will create an ENI (Elastic Network Interface) in your subnets

**Lambda with RDS Proxy**
If Lambda functions directly access your database, they may open too many connections under high load

**RDS Proxy**
    Improve scalability by pooling and sharing DB connections
    Improve availability by reducing by 66% the failover time and preserving connections
    Improve security by enforcing IAM authentication and storing credentials in Secrets Manager
> The Lambda function must be deployed in your VPC, because RDS Proxy is never publicly accessible

## Amazon DynamoDB
Fully managed, highly available with replication across multiple AZs
NoSQL database - not a relational database - with transaction support
Scales to massive workloads, distributed database
Millions of requests per second, trillions of row, 100s of TB of storage
Fast and consistent in performance (single-digit millisecond)
Integrated with IAM for security, authorization and administration
Low cost and auto-scaling capabilities
No maintenance(维护) or patching, always available
Standard & Infrequent Access (IA) Table Class
## DynamoDB - Basics
DynamoDB is made of Tables
Each table has a Primary Key (must be decided at creation time)
Each table can have an infinite number of items (=rows)
Each items has attributes (can be added over time - can be null)
Maximum size of an item is 400KB
Data types supported are:
    Scalar Types - String, Number, Binary, Boolean, Null
    Document Types - List, Map
    Set Types - String Set, Number Set, Binary Set
> Therefore, in DynamoDB you can rapidly evolve schemas
# Database
## Choosing the Right Database
We have a lot of managed databases on AWS to choose from
Questions to choose the right database based on your architecture:
    Read-heavy, write-heavy, or balanced workload? Throughput needs? Will it change, does it need to scale or fluctuate during the day?
    How much data to store and for how long? Will it grow? Average object size? How are they accessed?
    Data durability? Source of truth for the data?
    Latency requirements? Concurrent users?
    Data model? How will you query the data? Joins? Structured? Semi-Structured?
    Strong schema? More flexibility? Reporting? Search? RDBMS / NoSQL?
    License costs? Switch to Cloud Native DB such as Aurora?
## Database Types
* RDBMS (=SQL/OLTP): RDS, Aurora - great for joins
* NoSQL database - no joins, no SQL: DynamoDB(~JSON), ElastCache(key/value pairs), Neptune(graphs), DocumentDB(for MongoDB), Keyspaces(for Apache Cassandra)
* Object Store: S3(for big objects) / Glacier(for backups / archives)
* Data Warehouse (= SQL Analytics / BI): Redshift(OLAP), Athena, EMR
* Search: OpenSearch (JSON) - free text, unstructured searches
* Graphs: Amazon Neptune - displays relationships between data
* Ledger: Amazon Quantum Ledger Database
* Time series: Amazon Timestream
> Note: some databases are being discussed in the Data & Analytics section
## Amazon RDS - Summary
Managed PostgreSQL / MySQL / Oracle / SQL Server / MariaDB / Custom
Provisioned RDS Instance Size and EBS Volume Type & Size
Auto-scaling capability for Storage
Support for Read Replicas and Multi AZ
Security through IAM, Security Groups, KMS, SSL in transit
Automated Backup with Point in time restore feature (up to 35 days)
Manual DB Snapshot for longer-term recovery
Managed and Scheduled maintenance (with downtime)
Support for IAM Authentication, integration with Secrets Manager
RDS Custom for access to and customize the underlying instance (Oracle & SQL Server)
**Use case:** Store relational datasets (RDBMS / OLTP), perform SQL queries, transactions
## Amazon Aurora - Summary
Compatible API for PostgreSQL / MySQL, separation of storage and compute
Storage: data is stored in 6 replicas, across 3 AZ - highly available, self-healing, auto-scaling
Compute: Cluster of DB Instance across multiple AZ, auto-scaling of Read Replicas
Cluster: Custom endpoints for writer and reader DB instances
Same security / monitoring / maintenance features as RDS
Know the backup & restore options for Aurora
**Aurora Serverless** - for unpredictable / intermittent workloads, no capacity planning
**Aurora Multi-Master** - for continuous writes failover (high write availability)
**Aurora Global**: up to 16 GB Read Instances in each region, < 1 second storage replication
**Aurora Machine Learning:** perform ML using SageMaker & Comprehend on Aurora
**Aurora Database Cloning:** new cluster from existing one, faster then restoring a snapshot
**Use case:** same as RDS, but with less maintenance / more flexibility / more performance / more features
## Amazon ElastiCache - Summary
Managed Redis / Memcached (similar offering as RDS, but for caches)
In-memory data store, sub-millisecond latency
Must provision an EC2 instance type
Support for Clustering (Redis) and Multi AZ, Read Replicas (sharding)
Security through IAM, Security Groups, KMS, Redis Auth
Backup / Snapshot / Point in time restore feature
Managed and Scheduled maintenance
**Requires some application code changes to be leveraged**
**Use Case:** Key/Value store, Frequent reads, less writes, cache results for DB queries, store session data for websites, cannot use SQL
## Amazon DynamoDB - Summary
AWS proprietary technology, managed serverless NoSQL database, millisecond latency
Capacity modes: provisioned capacity with optional auto-scaling or on-demand capacity
Can replace ElatiCache as a key/value store (storing session data for example, using TTL feature)
Highly Available, Multi AZ by default, Read and Writes are decoupled, transation capability
DAX cluster for read cache, microsecond read latency
Security, authentication and authorization is done through IAM
Event Processing: DynamoDB Streams to integrate with AWS Lambda, or Kinesis Data Streams
Global Table feature: active-active setup
Automated backups up to 35 days with PITR (restore to new table), or on-demand backups
Export to S3 without using RCU within the PITR window, import from S3 without using WCU
**Great to rapidly evolve schemas**
**Use Case:** Serverless applications development (small documents 100s KB), distributed serverless cache, doesn't have SQL query language available
## Amazon S3 - Summary
S3 is a ... key/value store for objects
Great for bigger objects, not so great for many small objects
Serverless, scales infinitely, max object size is 5 TB, versioning capability
Tiers: S3 Standard, S3 Infrequent Access, S3 Intelligent, S3 Glacier + lifecycle policy
Features: Versioning, Encryption, Replication, MFA-Delete, Access Logs...
Security: IAM, Bucket Policies, ACL, Access Points, Object Lambda, CORS, Object/Vault Lock
Encryption: SSE-S3, SSE-KMS, SSE-C, client-side, TLS in transit, defualt encryption
Batch operations on objects using S3 Batch, listing files using S3 Inventory
Performance: Multi-part upload, S3 Transfer Acceleration, S3 Select
Automation: S3 Event Notifications (SNS, SQS, Lambda, EventBridge)

Use Cases: static files, key value store for big files, website hosting
## DocumentDB
Aurora is an "AWS-implementation" of PostgreSQL / MySQL
DocumentDB is the same for MongoDB (which is a NoSQL database)

MongoDB is used to store, query, and index JSON data
Similar "deployment concepts" as Aurora
Fully managed, highly available with replication across 3 AZ
Aurora storage automatically grows in increments of 10GB, up to 64TB

Automatically scales to workloads with millions of requests per seconds
## Amazon Neptune
Fully managed graph database
A popular graph dataset would be a social network
    Users have friends
    Posts have comments
    Comments have likes from users
    Users share and like posts...
Highly available across 3 AZ, with up to 15 read replicas
Build and run applications working with highly connected datasets - optimized for these complex and hard queries
Can store up to billions of relations and query the graph with milliseconds latency
Highly avaiable with replications across multiple AZs
Great for knowledge graphs (Wikipedia), fraud detection, recommendation engines, social networking
## Amazon Keyspaces (for Apache Cassandra)
Apache Cassandra is an open-source NoSQL distributed database
A managed Apache Cassandra - compatible database service
Serverless, Scalable, highly available, fully managed by AWS
Automatically scale talbes up/down based on the application's traffic
Tables are replicated 3 times across multiple AZ
Using the Cassandra Query Language (CQL)
Single-digit millisecond latency at any scale, 1000s of requests per second
Capacity: On-demand mode or provisioned mode with auto-scaling
Encryption, backup, Point-In-Time Recovery (PITR) up to 35 days

Use cases: store IoT devices info, time-series data,...
## Amazon QLDB
QLDB stands for "Quantum Ledger Database"
A ledger is a book recording financial transactions
Fully managed, Serverless, High available, Replication across 3 AZ
Used to **review history of all the changes made to your application data** over time
Immutable system: no entry can be removed or modified, cryptographically verifiable
2-3x better performance than common ledger blockchain frameworks, manipulate data using SQL
Difference with Amazon Managed Blockchain: **no decentralization component**, in accordance with financial regulation rules
## Amazon Timestream
Fully managed, fast, scalable, serverless time series database
Automatically scales up/down to adjust capacity
Store and analyze trillions of events per day
1000s times faster & 1/10th the cost of relational database
Scheduled queries, multi-measure records, SQL compatibility
Data storage tiering: recent data kept in memory and historical data kept in a cost-optimized storage
Build-in time series analytics functions (helps you identify patterns in your data in near real-time)
Encryption in transit and at rest

Use cases: IoT apps, operational applications, real-time analytics,...
# Amazon Athena
Serverless query service to analyze data stored in Amazon S3
Uses standard SQL language to query the files (built on Presto)
Supports CSV, JSON, ORC, Avro, and Parquet
Pricing: $5.00 per TB of data scanned
Commonly used with Amazon Quicksight for reporting/dashboards
**Use cases**: Business intelligence / analytics / reporting, analyze & query VPC Flow Logs, ELB Logs, **CloudTrail trails**, etc....

## Amazon Athena - Performance Improvement
Use columnar data for cost-savings (less scan)
    Apache Parquet or ORC is recommended
    Huge performance improvement
    Use Glue to convert your data your Parquet or ORC
Compress data for smaller retrievals (bzip2, gzip, lz4, snappy, zlip, zstd...)
Partition datasets in S3 for easy querying on virtual columns
Use larger files (> 128MB) to minimize overhead
# Redshift Overview
Redshift is based on PostgreSQL, but it's not used for OLTP
It's OLAP - online anlytical processing (Analytics and data warehousing)
10x better performance than other data warehouses, scale to PBs of data
Columnar storage of data (instead of row based) & parallel query engine
Pay as you go based on the instances provisoned
Has a SQL interface for performing the queries
Bl tools such as Amazon Quicksight or Tableau integrate with it
**vs Athena**: faster queries / joins / aggregations thanks to indexes
## Redshift Cluster
Leader node: for query planning, results aggregation
Compute node: for performing the queries, send results to leader
You provison the node size in advance
You can used Reserved Instances for cost savings
## Redshift - Snapshots & DR
Redshift has no "Multi-AZ" mode
Snapshots are point-in-time backups of a cluster, stored internally in S3
Snapshots are incremental (only what has changed is saved)
You can restore a snapshot into a new cluster
Automated: every 8 hours, every 5GB, or on a schedule. Set retention
Manual: snapshot is retained until you delete it
You can configure Amazon Redshift to automatically copy snapshots (automated or manual) of a cluster to another AWS Region
## Redshift Spectrum
Query data that is already in S3 without loading it
Must have a Redshift cluster available to start the query
The query is then submitted to thousands of Redshift Spectrum nodes
# Amazon OpenSearch Service
Amazon OpenSearch is successor to Amazon ElasticSearch
In DynamoDB, queries only exist by primary key or indexes...
With OpenSearch, you can search any field, even partially matches
It's common to use OpenSearch as a complement to another database
OpenSearch requires a cluster of instances (not serverless)
Does not support SQL (it has its own query language)
Ingestion from Kinesis Data Firehose, AWS IoT, and CloudWatch Logs
Security through Cognito & IAM, KMS encryption, TLS
Comes with OpenSearch Dashboards (visualization)
# Amazon EMR
EMR stands for Elastic MapReduce(分布式计算系统)
EMR helps creating Hadoop clusters (Big Data) to analyze and process vast amount of data
The clusters can be made of hundreds of EC2 instances
EMR comes bundled with Apache Spark, HBase, Presto, Flink...
EMR takes care of all the provisioning and configuration
Auto-scaling and integrated with Spot instances

**Use cases:** data processing, machines learning, web indexing, big data...
## Amazon EMR- Node types & purchasing
Master Node: Manage the cluster, coordinate, manage health - long running
Core Node: Run tasks and store data - long running
Task Node (optional): Just to run tasks - usually Spot
Purchasing options:
    On-demand: reliable, predictable, won't be terminated
    Reserved (min 1 year): cost savings (EMR will automatically use if available)
    Spot Instances: cheaper, can be terminated, less reliable
Can have long-running cluster, or transient (temporary) cluster
# Amazon QuickSight
Serverless machine learning-powered business intelligence service to create interactive dashboards
Fast, automatically scalable, embeddable, with per-session pricing
Use cases:
    Business analytics
    Building visualizations
    Perform ad-hoc analysis
    Get business insights using data
Integrated with RDS, Aurora, Athena, Redshift, S3...
In-memory computation using SPICE engine if data is imported into QuickSight
Enterprise edition: Possibility to setup Column-Level security(CLS)
## QuickSight - Dashboard & Analysis
Define Users (standard version) and Groups (enterprise version)
    These users & groups only exist within QuickSight, not IAM
A dashboard...
    is a read-only snapshot of an analysis that you can share
    preserves the configuration of the analysis (filtering, parameters, controls, sort)

You can share the analysis or the dashboard with Users or Groups
To share a dashboard, you must first publish it
Users who see the dashboard can also see the underlying data
# AWS Glue
Managed extract, transform, and load (ETL) service
Useful to prepare and transform data for analytics
Fully serverless service
# Kinesis Data Analytics for SQL applications
Real-time analytics on Kinesis Data Streams & Firehose using SQL
Add reference data from Amazon S3 to enrich streaming data
Fully managed, no servers to provision
Automatic scaling
Pay for actual consumption rate
Output:
    Kinesis Data Stream: create streams out of the real-time analytics queries
    Kinesis Data Firehose: send analytics query results to destinations
Use cases:
    Time-series analytics
    Real-time dashboards
    Real-time metrics
## Kinesis Data Analytics for Apache Flink
Use Flink (Java, Scala or SQL) to process and analyze streaming data
Run any Apache Flink application on a managed cluster on AWS
    provisioning compute resources, parallel computation, automatic scaling
    application backups (implemented as checkpoints and snapshots)
    Use any Apache Flink programming features
    Flink does not read from Firehose (use Kinesis Analytics for SQL instead)
# Amazon Managed Streaming for Apache Kafka (Amazon MSK)
Alternative to Amazon Kinesis
Fully managed Apache Kafka on AWS
    Allow you to create, update, delete clusters
    MSK creates & manages Kafka brokers nodes & Zookeeper nodes for you
    Deploy the MSK cluster in your VPC, multi-AZ (up to 3 for HA)
    Automatic recovery from common Apache Kafka failures
    Data is stored on EBS volumes for as long as you want
    
MSK Serverless
    Run Apache Kafka on MSK without managing the capacity
    MSK automatically provisions resources and scales compute & storage
## Kinesis Data Streams VS Amazon MSK 
| Kinesis Data Streams      | Amazon MSK                                  |
|:--------------------------|:--------------------------------------------|
| 1MB message size limit    | 1MB default, configure for higher (ex:10MB) |
| Data Streams with Shards  | Kafka Topics with Partitions                |
| Shard Splitting & Merging | Can only add partitions to a topic          |
| TLS In-flight encryption  | PLAINTEXT or TLS In-flight Encryption       |
| KMS at-rest encryption    | KMS at-rest encryption                      |

# Big Data Ingestion Pipeline
We want the ingestion pipeline to be fully serverless
We want to collect data in real time
We want to transform the data
We want to query the transformed data using SQL
The reports created using the queries should be in S3
We want to load that data into a warehouse and create dashboards
## Big Data Ingestion Pipeline
IoT devices -->Kinesis data streams-->kinesis data firehose --> S3....
# Amazon Rekognition
Find objects, people, text, scenes in images and videos using ML
Facial analysis and facial search to do user verification, people counting
Create a database of "familiar faces" or compare against celebrities
Use cases:
    Labeling
    Content Moderation
    Text Detection
    Face Detection and Analysis (gender, age, range, emotions...)
    Face Search and Verification
    Celebrity Recognition
    Pathing (ex: for sports game analysis)
# Amazon Transcribe
Automativally convert speech to text
Uses a deep learning process called automatic speech recognition (ASR) to convert speech to text quickly and accurately
Automatically remove Personally Identifiable Information (PII) using Redaction
Use cases:
    transcribe customer service calls
    automate closed captioning and subtitling
    generate metadata for media assets to create a fully searchable archive
# Amazon Polly
Turn text into lifelike speech using deep learning
Allowing you to create applications that talk
## Amazon Polly - Lexicon & SSML
Customize the pronunciation of words with Pronunciation lexicons
    Stylized words: St3ph4ne => "Stephane"
    Acronyms: AWS => "Amazon Web Services"
Upload the lexicons and use them in the SynthesizeSpeech operation

Generate speech from plain text or from documents marked up with Speech
Synthesis Markup Language (SSML) - enables more customization
    emphasizing specific words or phrases
    using phonetic pronunciation
    including breathing sounds, whispering
    using the Newscaster speaking style
# Amazon Translate
Natural and accurate language translation
Amazon Translate allows you to localize content - such as websites and applications - for international users, and to easily translate large volumes of text efficiently.
# Amazon Lex & Connect
Amazon Lex: (same technology that powers Alexa)
    Automatic Speech Recognition (ASR) to convert speech to text
    Natural Language Understanding to recognize the intent of text,callers
    Helps build chatbots, call center bots
Amazon Connect:
    Receive calls, create contact flows, cloud-based virtual contact center
    Can integrate with other CRM systems or AWS
    No upfront payments, 80% cheaper than tranditional contact center solutions

# Amazon Comprehend
For Natural Language Processing - NLP
Fully managed and serverless service
Uses machine learning to find insights and relationships in text
    Language of the text
    Extracts key phrases, places, people, brands, or events
    Understands how positive or negative the text is
    Analyzes text using tokenization and parts of speech
    Automatically organizes a collection of text files by topic
Sample use cases:
    analyze customer interactions (emails) to find what leads to a positive or negative experience
    Create and groups articles by topic that Comprehend will uncover
## Amazon Comprehend Medical
Amazon Comprehend Medical detects and returns useful information in unstructured clinical text:
    Physician's notes
    Discharge summaries
    Test results
    Case notes
Uses NLP to detect Protected Health Information (PHI) - *DetectPHI API*
Store your documents in Amazon S3, analyze real-time data with Kinesis Data Firehose, or use Amazon Transcribe to transcribe patient narratives into text that can be analyzed by Amazon Comprehend Medical.

# Amazon SageMaker
Fully managed service for developers / data scientists to build ML models
Typically difficult to do all the processes in one place + provision servers
Machine learning process (simplified): predicting your exam score

# Amazon Forecase
Fully managed service that uses ML to deliver highly accurate forecasts
Example: predict the future sales of a raincoat
50% more accurate than looking at the data itself
Reduce forecasting time from months to hours
Use cases: Product Demand Planning, Financial Planning, Resource Planning,...
# Amazon Kendra
Fully managed document search service powered by Machine Learning
Extract answers from within a document (text, pdf, HTML, PowerPoint, MS Word, FAQs...)
Natural language search capabilities
Learn from user interactions/feedback to promote preferred results (Incremental Learning)
Ability to manually fine-tune search results (importance of data, freshness, custom,...)
# Amazon Personalize
Fully managed ML-service to build apps with real-time personalized recommendations
Example: Personalized product recommendations/re-ranking, customized direct marketing
    Example:User bought gardening tools, provide recommendations on the next one to buy
Same technology used by Amazon.com
Integrates into existing websites, applications, SMS, email marketing systems,...
Implement in days, not months (you don't need to build, train, and deploy ML solutions)
Use cases: retial stores, media and entertainment...

# Amazon Textract
Automatically extracts text, handwriting, and data from any scanned documents using AI and ML

Extract data from forms and tables
Read and process any type of document (PDFs, images,...)
Use cases:
    Financial Services (e.g., invoices, financial reports)
    Healthcare (e.g., medical records, insurance claims)
    Public Sector (e.g., tax forms, ID documents, passports)

# AWS Monitoring
## Amazon CloudWatch Metrics(指标)
CloudWatch provides metrics for every services in AWS
Metric is a variable to monitor (CPU Utilization, NetworkIn...)
Metrics belong to namespaces
Dimension is an attribute of a metric (instance id, environment, etc...)
Up to 10 dimensions per metric
Metrics have timestamps
Can create CloudWatch dashboards of metrics
Can create CloudWatch Custom Metrics (for the RAM for example)

## CloudWatch Metric Streams
Continually stream CloudWatch metrics to a destination of your choice, with near-real-time delivery and low latency.
    Amazon Kinesis Data Firehose (and then its destinations)
    3rd party service provider: Datadog, Dynatrace, New Relic, Splunk, Sumo Logic...
    
Option to filter metrics to only stream a subset of them
## CloudWatch Logs
Log groups: arbitrary name, usually representing and application
Log stream: instances with application / log files / containers
Can define log expiration policies (never expire, 30 days, etc...)
CloudWatch Logs can send logs to:
    Amazon S3 (exports)
    Kinesis Data Streams
    Kinesis Data Firehose
    AWS Lambda
    ElasticSearch
## CloudWatch Logs - Sources
SDK, CloudWatch Logs Agent, CloudWatch Unified Agent
Elastic Beanstalk: collection of logs from application
ECS: collection from containers
AWS Lambda: collection from function logs
VPC Flow Logs: VPC specific logs
API Gateway
CloudTrail based on filter
Route53: Log DNS queries

## CloudWatch Logs Metrics Filter & Insights
CloudWatch Logs can use filter expressions
    For example, find a specific IP inside of a log
    Or count occurrences of "ERROR" in your logs
Metric filters can be used to trigger CloudWatch alarms
CloudWatch Logs Insights can be used to query logs and add queries to CloudWatch Dashboards
## CloudWatch Logs - S3 Export
Log data can take up to 12 hours to become available for export
The API call is CreateExportTask

Not near-real time or real-time... use Logs Subscriptions instead
## CloudWatch Log Subscriptions

## CloudWatch Logs for EC2
By default, no logs from your EC2 machine will go to CloudWatch
You need to run a CloudWatch agent on EC2 to push the log files you want
Make sure IAM permissions are correct
The CloudWatch log agent can be setup on-premises too

## CloudWatch Logs Agent & Unified Agent
For virtual servers (EC2 instances, on-premise servers...)
CloudWatch Logs Agent
    Old version of the agent
    Can only send to CloudWatch Logs

CloudWatch Unified Agent
    Collect additional system-level metrics such as RAM, processes, etc...
    Collect logs to send to CloudWatch Logs
    Centralized configuration using SSM Parameter Store
## CloudWatch Unified Agent - Metrics
Collect directly on your Linux server / EC2 instance

CPU (active, guest, idle, system, user, steal)
Disk metrics (free, used, total), Disk IO (writes, reads, bytes, iops)
RAM (free, inactive, used, total, cached)
Netstat (number of TCP and UDP connections, net packets, bytes)
Processes (total, dead, bloqued, idle, running, sleep)
Swap Space (free, used, used %)

Reminder: out-of-the box metrics for EC2 - disk, CPU, network (high level)

## CloudWatch Alarms
Alarms are used to trigger notifications for any metric
Various options (sampling, %, max, min, etc...)
Alarm States:
    OK
    INSUFFICIENT_DATA
    ALARM
Period:
    Length of time in seconds to evaluate the metric
    High resolution custom metrics: 10 sec, 30 sec or multiples of 60 sec

## CloudWatch Alarm Targets
Stop, Terminate, Reboot, or Recover an EC2 Instance
Trigger Auto Scaling Action
Send notification to SNS (from which you can do pretty much anything)

# EC2 Instance Recovery
Status Check:
    Instance status = check the EC2 VM
    System status = check the underlying hardware

Recovery: Same Private, Public, Elastic IP, metadata, placement group

## CloudWatch Alarm: good to know
Alarms can be created based on CloudWatch Logs Metrics Filters
To test alarms and notifications, set the alarm state to Alarm using CLI

aws cloudwatch set-alarm-state --alarm-name "myalarm" --state-value ALARM 
--state-reason "testing purpose"

## Amazon EventBridge (formerly CloudWatch Events)
Schedule: Cron jobs (scheduled scripts)
Event Pattern: Event rules to react to a service doing something
Trigger Lambda functions, send SQS/SNS messages...

## CloudWatch Container Insights
Collect, aggregate, summarize metrics and logs from containers
Available for containers on...
    Amazon Elastic Container Service (Amazon ECS)
    Amazon Elastic Kubernetes Services (Amazon EKS)
    Kubernetes platforms on EC2
    Fargate (both for ECS and EKS)
## CloudWatch Lambda Insights
Monitoring and troubleshooting solution for serverless applications running on AWS Lambda
Collect, aggregates, and summarizes system-level metrics including CPU time, memory, disk, and network
Collect, aggregates, and summarizes diagnostic information such as cold starts and Lambda worker shutdowns
Lambda Insight is provided as a Lambda Layer

## CloudWatch Contributor Insights
Analyze log data and create time series that display contributor data
    See metrics about the top-N contributors
    The total number of unique contributors, and their usage
This helps you find top talkers and understand who or what is impacting system performance
Works for any AWS-generated logs (VPC, DNS, etc...)
For example, you can find bad hosts, **identify the heaviest network users**, or find the URLs that generate the most errors.
You can build your rules from scratch, or you can also use sample rules that AWS has created - leverages your CloudWatch Logs
CloudWatch also provides built-in rules that you can use to analyze metrics from other AWS services

## CloudWatch Application Insights
Provides automated dashboards that show potential problems with monitored applications, to help isolate ongoing issues
You applications run on Amazon EC2 Instances with select technologies only (Java, .Net, Microsoft IIS Web Server, databases...)
And you can use other AWS resources such as Amazon EBS, RDS, ELB, ASG, Lambda, SQS, DynamoDB, S3 bucket, ECS, EKS, SNS, API Gateway...

Powered by SageMaker
Enhanced visibility into your application health to reduce the time it will take you to troubleshoot and repair your applications
Findings and alerts are sent to Amazon EventBridge and SSM OpsCenter

## CloudWatch Insights and Operational Visibility
CloudWatch Container Insights
    ECS, EKS, Kubernetes on EC2, Fargate, needs agent for Kubernetes
    Metrics and logs
    
CloudWatch Lambda Insights
    Detailed metrics to troubleshoot serverless applications

CloudWatch Contributors Insights
    Find "Top-N" Contributors through CloudWatch Logs

CloudWatch Application Insights
    Automatic dashboard to troubleshoot your application and related AWS services

## AWS CloudTrail
Provides governance, compliance and audit for your AWS Account
CloudTrail is enabled by default
Get an history of events / API calls made within your AWS Account by:
    Console
    SDK
    CLI
    AWS Services
Can put logs from CloudTrail into CloudWatch Logs or S3
A trail can be applied to All Regions (default) or a single Region.
**If a resource is deleted in AWS, investigate CloudTrail first!**

## CloudTrail Events
Management Events:
    Operations that are performed on resources in your AWS account
    Examples:
        Configuring security (IAM **AttachRolePolicy**)
        Configuring rules for routing data (Amazon EC2 **CreateSubnet**)
        Setting up logging (AWS CloudTrail **CreateTrail**)
    By default, trails are configured to log management events.
    Can separate Read Events (that don't modify resources) from Write Events (that may modify resources)

Data Events:
    By default, data events are not logged (because high volume operations)
    Amazon S3 object-level activity (ex: GetObject, DeleteObject, PutObject): can separate Read and Write Events
    AWS Lambda function execution activity (the Invoke API)

## CloudTrail Insight (Events)
Enable CloudTrail Insights to detect unusual activity in your account
    inaccurate resource provisioning
    hitting service limits
    Bursts of AWS IAM actions
    Gaps in periodic maintenance activity

CloudTrail Insights analyzes normal management events to create a baseline
And then continuously analyze write events to detect unusual patterns
    Anomalies(反常现象) appear in the CloudTrail console
    Event is sent to Amazon S3
    An EventBridge event is generated (for automation needs)
    
## CloudTrail Events Retention(保存)
Events are stored for 90 days in CloudTrail
To keep events beyond this period, log them to S3 and use Athena

## AWS Config
Helps with auditing and recording compliance of your AWS resources
Helps record configurations and changes over time
Questions that can be solved by AWS Config:
    Is there unrestricted SSH access to my security groups?
    Do my buckets have any public access?
    How has my ALB configuration changed over time?
You can receive alerts (SNS notifications) for any changes
AWS Config is a per-region service
Can be aggregated across regions and accounts
Possibility of storing the configuration data into S3 (analyzed by Athena)
## Config Rules
Can use AWS managed config rules (over 75)
Can make custom config rules (must be defined in AWS Lambda)
    Ex: evaluate if each EBS disk is of type gp2
    Ex: evaluate if each EC2 instance is t2.micro
Rules can be evaluated / triggered:
    For each config change
    And / or: at regular time intervals
AWS Config Rules does not prevent actions from happening (no deny)
Pricing: no free tier, 0.003 per configuration item recorded per region, 0.001 per config rule evaluation per region.

## AWS Config Resource
View compliance of a resource over time

View configuration of a resource over time

View CloudTrail API calls of a resource over time

## Config Rules - Remediations(改良剂)
Automate remediation of non-compliant resources using SSM Automation Documents
Use AWS-Managed Automation Documents or create custom Automation Documents
    Tip: you can create custom Automation Documents that invokes Lambda function
You can set Remediation Retries if the resource is still non-compliant after auto-remediation

## Config Rules - Notifications
Use EventBridge to trigger notifications when AWS resources are non-compliant

Ability to send configuration changes and compliance state notifications to SNS (all events - use SNS Filtering or filter at client-side)

# CloudWatch vs CloudTrail vs Config
CloudWatch
    Performance monitoring (metrics, CPU, network, etc...) & dashboards
    Events & Alerting
    Log Aggregation & Analysis
CouldTrail
    Record API calls made within your Account by everyone
    Can define trails for specific resources
    Global Service
Config
    Record configuration changes
    Evaluate resources against compliance rules
    Get timeline of changes and compliance
## For an Elastic Load Balancer
CloudWatch:
    Monitoring Incoming connections metric
    Visualize error codes as a % over time
    Make a dashboard to get an idea of your load balancer performance
Config:
    Track security group rules for the Load Balancer
    Track configuration changes for the Load Balancer
    Ensure an SSL certificate is always assigned to the Load Balancer (compliance)
    
CloudTrail:
    Track who made any changes to the Load Balancer with API calls
    
# AWS Organizations
Global service
Allows to manage multiple AWS accounts
The main account is the management account
Other accounts are member accounts
Member accounts can only be part of one organization
Consolidated(统一的) Billing across all accounts - single payment method
Pricing benefits from aggregated usage (volume discount for EC2, S3...)
Shared reserved instances and Saving Plans discounts across accounts
API is available to automate AWS creation

Root Organization Unit (OU)

## Advantages
Multi Account vs One Account MultiVPC
Use tagging standards for billing purposes
Enable CloudTrail on all accounts, send logs to central S3 account
Send CloudWatch Logs to central logging account
Establish Cross Account Roles for Admin purposes
## Security: Service Control Policies(SCP)
IAM policies applied to OU or Accounts to restrict Users and Roles
They do not apply to the management account (full admin power)
Must have an explicit(清楚明白的) allow (does not allow anything by default - like IAM)

Management Account:
    Can do anything
    (no SCP apply)
Account A:
    Can do anything
    EXCEPT access Redshift
    (explicit Deny from OU)
## SCP (Service Control Policies)
Blocklist and Allowlist strategies

# IAM Conditions
## IAM Roles vs Resource Based Policies
When you assume a role (user, application or service), you give up your original permissions and take the permissions assigned to the role

When using a resource based policy, the principal(负责人) doesn't have to give up his permissions

Example: User in account A needs to scan a DynamoDB table in Account A and dump it in an S3 bucket in Account B.

Supported by: Amazon S3 buckets, SNS topics, SQS queues

Cross account:
    attaching a resource-based policy to a resource (example: S3 bucket policy)
    OR using a role as a proxy

**Differences**  
When you assume a role (user, application or service), you give up your original permissions and take the permissions assigned to the role
When using a resource-based policy, the principal doesn't have to give up his permissions
<u>Example:</u> User in account A needs to scan a DynamoDB table in Account A and dump it in an S3 bucket in Account B.
Supported by: Amazon S3 buckets, SNS topics, SQS queues, etc...
## Amazon EventBridge - Security
When a rule runs, it needs permissions on the target
Resource-based policy: Lambda, SNS, SQS, CloudWatch Logs, API Gateway...
IAM role: Kinesis stream, Systems Manager Rum Command, ECS task...

# IAM Permission Boundaries
IAM Permission Boundaries are supported for users and roles (not groups)
Advanced feature to use a managed policy to set the maximum permissions an IAM entity can get.
> An IAM Permission Boundary is the only thing the user or role can do even though the user attached all permissions.

# AWS Single Sign-On (SSO)
Replaced by AWS IAM Identity Center
Centrally manage Single Sign-On to access multiple accounts and 3rd-party business applications
Integrated with AWS Organizations
Supports SAML 2.0 markup
Integration with on-premise Active Directory
Centralized permission management
Centralized auditing(审计) with CloudTrail

> Security Assertion Markup Language (SAML)(安全断言标记语言)

## SSO vs AssumeRoleWithSAML

# Amazon Cognito
Give users an identity to interact with our web or mobile application
Cognito User Pools:
    Sign in functionality for app users
    Integrate with API Gateway & Application Load Balancer
    
Cognito Identity Pools (Federated Identity):
    Provide AWS credentials to users so they can access AWS resources directly
    Integrate with Cognito User Pools as an identity provider

Cognito vs IAM: "hundreds of users", "mobile users", "authenticate with SAML"

## Cognito User Pools (CUP) - User Features
Create a serverless database of user for your web & mobile apps
Simple login: Username (or email) / password combination
Password reset
Email & Phone Number Verification
Multi-factor authentication (MFA)
Federated Identities: users from Facebook, Google, SAML...
## Cognito User Pools (CUP) - Integrations
CUP integrates with API Gateway and Application Load Balancer

# AWS Security & Encryption - Important
## Encryption in flight (SSL)
Data is encrypted before sending and decrypted after receiving
SSL certificates help with encryption (HTTPS)
Encryption in flight ensures no MITM (main in the middle attack) can happen

## Server side encryption at rest
Data is encrypted after being received by the server
Data is decrypted before being sent
It is stored in an encrypted form thanks to a key (usually a data key)
The encryption / decryption keys must be managed somewhere and the server must have access to it

## Client side encryption
Data is encrypted by the client and never decrypted by the server
Data will be decrypted by a receiving client
The server should not be able to decrypt the data
Could leverage(利用) Envelope Encryption

## AWS KMS (Key Management Service)
Anytime you hear "encryption" for an AWS service, it's most likely KMS
AWS manages encryption keys for us
Fully integrated with IAM for authorization
Easy way to control access to your data
Able to audit KMS Key usage using CloudTrail
Seamlessly(无缝地) integrated into most AWS services (EBS, S3, RDS, SSM...)
Never ever store your secrets in plaintext, especially in your code!
    KMS Key Encryption also available through API calls (SDK, CLI)
    Encrypted secrets can be stored in the code / environment variables
## KMS Keys Types
KMS Keys is the new name of KMS Customer Master Key
Symmetric(对称的) (AES-256 keys)
    Single encryption key that is used to Encrypt and Decrypt
    AWS services that are integrated with KMS use Symmetric CMKs
    You never get access to the KMS Key unencrypted (must call KMS API to use)

Asymmetric(不对称的) (RSA & ECC key pairs)
    Public (Encrypt) and Private Key (Decrypt) pair
    Used for Encrypt/Decrypt, or Sign/Verify operations
    The public key is downloadable, but you can't access the Private Key unencrypted
    Use case: encryption outside of AWS by users who can't call the KMS API

Three types of KMS Keys:
    AWS Managed Key: free (aws/service-name, example: aws/rds or aws/ebs)
    Customer Managed Keys (CMK) created in KMS: 1 dollar/month
    Customer Managed Keys imported (must be 256-bit symmetric key): 1 dollar/month + pay for API call to KMS (0.03 dollars / 10000 calls)
    
Automatic Key rotation:
    AWS-managed KMS Key: automatic every 1 year
    Customer-managed KMS Key: (must be enabled) automatic every 1 year
    Imported KMS Key: only manual rotation possible using alias
    
KMS Key Policies
Control access to KMS keys, "similar" to S3 bucket policies
Difference: you cannot control access without them

Default KMS Key Policy:
    Created if you don't provide a specific KMS Key Policy
    Complete access to the key to the root user = entire AWS account
Custom KMS Key Policy:
    Define users, roles that can access the KMS key
    Define who can administer the key
    Useful for cross-account access of your KMS key

Copying Snapshots across accounts
1. Create a Snapshot, encrypted with your own KMS Key (Customer Managed Key)
2. Attach a KMS Key Policy to authorize cross-account access
3. Share the encrypted snapshot
4. (in target) Create a copy of the Snapshot, encrypt it with a CMK in your account
5. Create a volume from the snapshot

## KMS Multi-Region Keys
Identical KMS keys in different AWS Regions that can be used interchangeably
Multi-Region keys have the same key ID, key material, automatic rotation...

Encrypt in one Region and decrypt in other Regions
No need to re-encrypt or making cross-Region API calls

KMS Multi-Region are NOT global (Primary + Replicas)
Each Multi-Region key is managed independently

Use cases: global client-side encryption, encryption on Global DynamoDB, Global Aurora

## DynamoDB Global Tables and KMS Multi-Region Keys Client-Side encryption
We can encrypt specific attributes client-side in our DynamoDB table using the Amazon DynamoDB Encryption Client

Combined with Global Tables, the client-side encrypted data is replicated to other regions

If we use a multi-region key, replicated in the same region as the DynamoDB Global table, then clients in these regions can use low-latency API calls to KMS in their region to decrypt the data client-side

Using client-side encryption we can protect specific fields and guarantee only decryption if the client has access to an API key

## Global Aurora and KMS Multi-Region Keys Client-Side encryption
We can encrypt specific attributes client-side in our Aurora table using the AWS Encryption SDK

Combined with Aurora Global Tables, the client-side encrypted data is replicated to other regions

If we use a multi-region key, replicated in the same region as the Global Aurora DB, then clients in these regions can use low-latency API calls to KMS in their region to decrypt the data client-side

Using client-side encryption we can protect specific fields and guarantee only decryption if the client has access to an API key, we can protect specific fields even from database admins

## S3 Replication Encryption Considerations
Unencrypted objects and objects encrypted with SSE-S3 are replicated(重复的) by default
Objects encrypted with SSE-C (customer provided key) are never replicated

For objects encrypted with SSE-KMS, you need to enable the option
    Specify which KMS Key to encrypt the objects within the target bucket
    Adapt the KMS Key Policy for the target key
    An IAM Role with kms:Decrypt for the source KMS Key and kms:Encrypt for the target KMS Key
    You might get KMS throttling(节流调节) errors, in which case you can ask for a Service Quotas increase

You can use multi-region AWS KMS Keys, but they are currently treated as independent keys by Amazon S3 (the object will still be decrypted and then encrypted)

## AMI Sharing Process Encrypted via KMS
1. AMI in Source Account is encrypted with KMS Key from Source Account
2. Must modify the image attribute to add a Launch Permission which corresponds to the specified target AWS account
3. Must share the KMS Keys used to encrypted the snapshot the AMI references with the target account / IAM Role
4. The IAM Role/User in the target account must have the permissions to DescribeKey, ReEncrypted, CreateGrant, Decrypt
5. When launching an EC2 instance from the AMI, optionally the target account can specify(明确指出) a new KMS key in its own account to re-encrypt the volumes

## SSM Parameter Store (AWS Systems Manager -> Parameter Store)
Secure storage for configuration and secrets
Optional Seamless Encryption using KMS
Serverless, scalable, durable, easy SDK
Version tracking of configurations / secrets
Configuration management using path & IAM
Notifications with CloudWatch Events
Integration with CloudFormation
```
aws ssm get-parameters --names /my-app/dev/db-url /my-app/dev/db-password 
--with-decryption

aws ssm get-parameters-by-path help
aws ssm get-parameters-by-path --path /my-app/dev/
aws ssm get-parameters-by-path --path /my-app/ --recursive(递归的)
```
## AWS Secrets Manager
Newer service, meant for storing secrets
Capability(能力) to force rotation(轮流) of secrets every X days
Automate generation of secrets on rotation (uses Lambda)
Integration with Amazon RDS (MySQL, PostgreSQL, Aurora)
Secrets are encrypted using KMS

Mostly meant for RDS integration

## AWS Certificate Manager (ACM)
Easily provision, manage, and deploy TLS(Transport Layer Security) Certificates
Provide in-flight encryption for websites (HTTPS)
Supports both public and private TLS certificates
Free of charge for public TLS certificates
Automatic TLS certificate renewal
Integrations with (load TLS certificates on)
    Elastic Load Balancers (CLB, ALB, NLB)
    CloudFront Distributions
    APIs on API Gateway
Cannot use ACM with EC2 (can't be extracted)

## ACM - Requesting Public Certificates
1. List domain names to be included in the certificate
    Fully Qualified Domain Name (FQDN):corp.example.com
    Wildcard Domain:*.example.com
2. Select Validation Method:DNS Validation or Email validation
    DNS Validation is preferred for automation purposes
    Email validation will send emails to contact addresses in the WHOIS database
    DNS Validation will leverage a CNAME record to DNS config (ex:Route 53)
3. It will take a few hours to get verified
4. The Public Certificate will be enrolled for automatic renewal
    ACM automatically renews ACM-generated certificates 60 days before expiry
## ACM - Importing Public Certificates
Option to generate the certificate outside of ACM and then import it
No automatic renewal, must import a new certificate before expiry
ACM sends daily expiration events starting 45 days prior to expiration
    The # of days can be configured
    Events are appearing in EventBridge
AWS Config has a managed rule named acm-certificate-expiration-check to check for expiring certificates
(configurable number of days)

## ACM - Integration with ALB (Application Load Balancer)
## API Gateway - Endpoint Types
Edge-Optimized (default): For global clients
    Requests are routed through the CloudFront Edge locations (improves latency)
    The API Gateway still lives in only one region
Regional:
    For clients within the same region
    Could manually combine with CloudFront (more control over the caching strategies and the distribution)
Private:
    Can only be accessed from your VPC using an interface VPC endpoint (ENI)
    Use a resource policy to define access

## ACM - Integration with API Gateway
Create a Custom Domain Name in API Gateway
Edge-Optimized (default): For global clients
    Requests are routed through the CloudFront Edge locations (improves latency)
    The API Gateway still lives in only one region
    The TLS Certificate must be in the same region as CloudFront, in use-west-2
    The setup CNAME or (better) A-Alias record in Route 53

Regional:
    For clients within the same region
    The TLS Certificate must be imported on API Gateway, in the same region as the API Stage
    Then setup CNAME or (better) A-Alias record in Route 53

## AWS WAF - Web Application Firewall
Protects your web applications from common web exploits (Layer 7)
Layer 7 is HTTP (vs Layer 4 is TCP/UDP)

Deploy(部署) on
    Application Load Balancer
    API Gateway
    CloudFront
    AppSync GraphQL API
    Cognito User Pool

Define Web ACL (Web Access Control List) Rules:
    IP Set: up to 10,000 IP addresses - use multiple Rules for more IPs
    HTTP headers, HTTP body, or URI strings Protects from common attack - SQL injection and Cross-Site Scripting (XSS)
    Size constraints, geo-match (block countries)
    Rate-based rules (to count occurrences of events) - for DDoS protection

Web ACL are Regional except for CloudFront
A rule group is a reusable set of rules that you can add to a web ACL

## WAF - Fixed IP while using WAF with a Load Balancer
WAF does not support the Network Load Balancer (Layer 4)
We can use Global Accelerator for fixed IP and WAF on the ALB

## AWS Shield: protect from DDoS attack
DDoS: Distributed Denial of Service - many requests at the same time
AWS Shield Standard:
    Free service that is activated for every AWS customer
    Provides protection from attacks such as SYN/UDP Floods, Reflection attacks and other layer 3/layer 4 attacks
AWS Shield Advanced:
    Optional DDoS mitigation service ($3,000 per month per organization)
    Protect against more sophisticated attack on Amazon EC2, Elastic Load Balancing(ELB), Amazon CloudFront, AWS Global Accelerator, and Route 53
    24/7 access to AWS DDoS response team (DRP)
    Protect against higher fees during usage spikes due to DDos
    Shield Advanced automatic application layer DDoS mitigation automatically creates, evaluates and deploys AWS WAF rules to mitigate layer 7 attacks

## AWS Firewall Manager
Manage rules in all accounts of an AWS Organization

Security policy: common set of security rules
    WAF rules (Application Load Balancer, API Gateways, CloudFront)
    AWS Shield Advanced (ALB, CLB, NLB, Elastic IP, CloudFront)
    Security Groups for EC2, Application Load Balancer and ENI resources in VPC
    AWS Network Firewall (VPC Level)
    Amazon Route 53 Resolver DNS Firewall
    Policies are created at the region level
    
Rules are applied to new resources as they are created (good for complicance) across all and future accounts in your Organization

## WAF vs Firewall Manager vs Shield
WAF, Shield and Firewall Manager are used together for comprehensive protection
Define your Web ACL rules in WAF
For granular(颗粒的) protection of your resources, WAF alone is the correct choice

If you want to use AWS WAF across accounts, accelerate WAF configuration, automate the protection of new resources, use Firewall Manager with AWS WAF

Shield Advanced adds additional features on top of AWS WAF, such as dedicated support from the Shield Response Team (SRT) and advanced reporting.

If you're prone to frequent DDoS attacks, consider purchasing Shield Advanced

## AWS Best Practices for DDoS Resiliency(弹性,跳回) Edge Location Mitigation(缓和) (BP1, BP3)
BPI - CloudFront
    Web Application delivery at the edge
    Protect from DDoS Common Attacks (SYN floods, UDP reflection...)
BPI - Global Accelerator
    Access your application from the edge
    Integration with Shield for DDoS protection
    Helpful if your backend is not compatible with CloudFront
BP3 - Route 53
    Domain Name Resolution at the edge
    DDoS Protection mechanism

Infrastructure layer defense (BP1, BP3, BP6)
    Protect Amazon EC2 against high traffic
    That includes using Global Accelerator, Route 53, CloudFront, Elastic Load Balancing

Amazon EC2 with Auto Scaling (BP7)
    Help scale in case of sudden traffic surges including a flash crowd or a DDoS attack

Elastic Load Balancing (BP6)
    Elastic Load Balancing scales with the traffic increases and will distribute the traffic to many EC2 instances
    
Detect and filter malicious(恶意的) web requests (BP1, BP2)
    CloudFront cache static content and serve it from edge locations, protecting your backend
    AWS WAF is used on top of CloudFront and Application Load Balancer to filter and block requests based on request signatures 
    WAF rate-based rules can automatically block the IPs of bad actors
    Use managed rules on WAF to block attacks based on IP reputation, or block anonymous lps
    CloudFront can block specific geographies

Shield Advanced (BP1, BP2, BP6)
    Shield Advanced automatic application layer DDoS mitigation automatically creates, evaluates and deploys AWS WAF rules to mitigate layer 7 attacks
    
**Attack surface reduction**
Obfuscating(弄暗淡) AWS resources (BP1, BP4, BP6)
    Using CloudFront, API Gateway, Elastic Load Balancing to hide your backend resources (Lambda functions, EC2 instances)
    
Security groups and Network ACLs (BP5)
    Use security groups and NACLs to filter traffic based on specific IP at the subnet or ENI-level
    Elastic IP are protected by AWS Shield Advanced

Protecting API endpoints (BP4)
    Hide EC2, Lambda, elsewhere
    Edge-optimized mode, or CloudFront + regional mode (more control for DDoS)
    WAF + API Gateway: burst limits, headers filtering, use API keys

## Amazon GuardDuty
Intelligent Threat discovery to Protect AWS Account
Uses Machine Learning algorithms, anomaly detection, 3rd party data
One click to enable (30 days trial), no need to install software
Input data includes:
    CloudTrail Events Logs - unusual API call, unauthorized deployments
        CloudTrail Management Events - create VPC subnet, create trail, ...
        CloudTrail S3 Data Events - get object, list objects, delete object,...
    VPC Flow Logs - unusual internal traffic, unusual IP address
    DNS Logs - compromised EC2 instances sending encoded data within DNS queries
    Kubernetes Audit Logs - suspicious activities and potential EKS cluster compromises

Can setup CloudWatch Event rules to be notified in case of findings
CloudWatch Events rules can target AWS Lambda or SNS
Can protect against CryptoCurrency attacks (has a dedicated "finding" for it)

![IMG_6999FE8F2AE1-1](assets/IMG_6999FE8F2AE1-1.jpeg)

## Amazon Inspector
Automated Security Assessments

For EC2 instances
    Leveraging the AWS System Manager (SSM) agent
    Analyze against unintended network accessibility
    Analyze the running OS against known vulnerabilities(缺陷)
For Containers push to Amazon ECR
    Assessment of containers as they are pushed
    
Reporting & integration with AWS Security Hub
Send findings to Amazon Event Bridge

## What does AWS Inspector evaluate?
Remember: only for EC2 instances and container infrastructure
Continuous scanning of the infrastructure, only when needed
Package vulnerabilities (EC2 & ECR) - database of CVE
Network reachability (EC2)
A risk score is associated with all vulnerabilities for prioritization

## Amazon Macie
Amazon Macie is a fully managed data security and data privacy service that uses machine learning and pattern matching to discover and protect your sensitive data in AWS.

Macie helps identify and alert you to sensitive data, such as personally identifiable information (PII)

# Virtual Private Cloud (VPC)
## Understanding CIDR (Classless Inter-Domain Routing) - IPv4
Classes Inter-Domain Routing -  a method for allocationg IP addresses
Used in Security Groups rules and AWS networking in general
They help to define an IP address range:

A CIDR consists of two components
Base IP
    Represents an IP contained in the range (XX.XX.XX.XX)
    Example: 10.0.0.0, 192.168.0.0, ...

Subnet Mask
    Defines how many bits can change in the IP
    Example:/0, /24, /32
    Can take two forms:
        /8 -> 255.0.0.0
        /16 -> 255.0.0.0
        /24 -> 255.255.255.0
        /32 -> 255.255.255.255
## Explain
192.168.0.0/16 表示32位的二进制地址中，前16位为网络前缀，后16位代表主机号。

在换算中，192.168.0.0/16 对应的二进制为：

1100 0000，1010 1000，*0000 0000，0000 0000*
其中红色为主机号，总共有16位。当这16位全为0时，取最小地址192.168.0.0，当这16位全为1时，取最大地址192.168.255.255。但请注意，在实际中，主机号全为0或者全为1的地址一般不使用，作为预留地址另有作用。所以第一个地址为：

1100 0000，1010 1000，0000 0000，0000 0001
即 192.168.0.1 最后一个地址为：

1100 0000，1010 1000，1111 1111，1111 1110
即 192.168.255.254。

因此，192.168.0.0/16 代表的IP段就是 192.168.0.1 ~ 192.168.255.254.

![IMG_0173](assets/IMG_0173.jpg)

## Default VPC Walkthrough
All new AWS accounts have a default VPC
New EC2 instances are launched into the default VPC if no subnet(子网络) is specified
Default VPC has Internet connectivity and all EC2 instances inside it have public IPv4 addresses
We also get a public and a private IPv4 DNS names

## VPC in AWS - IPv4
Max.CIDR per VPC is 5, for each CIDR:
    Min.size is /28 (16 IP addresses)
    Max.size is /16 (65536 IP addresses)
Because VPC is private, only the Private IPv4 ranges are allowed:
    10.0.0.0 - 10.255.255.255 (10.0.0.0/8)
    172.16.0.0 - 172.31.255.255 (172.16.0.0/12)
    192.168.0.0 - 192.168.255.255 (192.168.0.0/16)

Your VPC CIDR should NOT overlap with your other networks (e.g.,corporate)

## VPC - Subnet (IPv4)
AWS reserves 5 IP addresses (first 4 & last 1) in each subnet
These 5 IP addresses are not available for use and can't be assigned to an EC2 instance
Example: if CIDR block 10.0.0.0/24, then reserved IP addresses are:
    10.0.0.0 - Network Address
    10.0.0.1 - reserved by AWS for the VPC router
    10.0.0.2 - reserved by AWS for mapping(映射) to Amazon-provided DNS
    10.0.0.3 - reserved by AWS for future use
    10.0.0.255 - Network Broadcast Address. AWS does not support broadcast in a VPC, therefore the address is reserved

<u>Exam Tip</u>, if you need 29 IP addresses for EC2 instances:
    You can't choose a subnet of size /27 (32 IP addresses, 32-5=27 < 29)
    相当于2的5次方等于32，但是要减去5的保留的ip字段，所以实际可用的只有27个IP地址，不够29
    You need to choose a subnet of size /26 (64 IP addresses, 64-5=59 > 29)
    2的6次方等于64，减去5的保留IP，剩59个ip字段可用，满足有29个IP字段
    
![IMG_0174](assets/IMG_0174.PNG)
![IMG_0176](assets/IMG_0176.PNG)

## Bastion Hosts
运维安全中心（堡垒机）是阿里云提供的运维和安全审计管控平台，可集中管理运维权限，全程管控操作行为，实时还原运维场景，保障运维行为身份可鉴别、权限可管控、操作可审计，解决资产多难管理、运维职责权限不清晰以及运维事件难追溯等问题， 助力企业满足等保合规需求。
![IMG_0177](assets/IMG_0177.PNG)
## Bastion Host Hands On
![Screenshot 2023-03-06 at 7.13.54 PM](assets/Screenshot%202023-03-06%20at%207.13.54%20PM.png)
chmod 0400 EC2-tutorial.pem\  #change permission to the EC2-tutorial key pair
ssh ec2-user@10.0.23.221 -i EC2-tutorial.pem  #use key pair connect to private ip address through Bastion host

## NAT Instance (Network Address Translation)![IMG_0179](assets/IMG_0179.jpg)
![IMG_0180](assets/IMG_0180.jpg)
![IMG_0181](assets/IMG_0181.jpg)

## NAT Gateway![IMG_0182](assets/IMG_0182.jpg)

## Network Access Control List (NACL)
![IMG_0183](assets/IMG_0183.jpg)
![IMG_0184](assets/IMG_0184.jpg)
![IMG_0185](assets/IMG_0185.jpg)

## Ephemeral(短暂的,短命的) Ports
![IMG_0186](assets/IMG_0186.jpg)
![IMG_0187](assets/IMG_0187.jpg)
![IMG_0188](assets/IMG_0188.jpg)

## VPC Peering
![IMG_0189](assets/IMG_0189.jpg)
![IMG_0190](assets/IMG_0190.jpg)
![IMG_0191](assets/IMG_0191.jpg)

## VPC Endpoints
![IMG_0192](assets/IMG_0192.jpg)

## VPC Endpoints (AWS PrivateLink)
![IMG_0193](assets/IMG_0193.jpg)
![IMG_0194](assets/IMG_0194.jpg)

## Gateway or Interface Endpoint for S3?
![IMG_0195](assets/IMG_0195.jpg)

