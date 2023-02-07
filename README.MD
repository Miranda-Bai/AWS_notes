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










