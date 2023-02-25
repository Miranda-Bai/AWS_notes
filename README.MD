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


 




