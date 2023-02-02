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
# EC2 
## Create Instance
> User data
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