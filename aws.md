# AWS - Amazon Web Service

- [About](#about)
- [Advantages of cloud computing](#advantages-of-cloud-computing)
- [Regions and AZs](#regions-and-azs)
- [Access AWS Services](#access-aws-services)
- [Pricing](#pricing)
- [Security](#security)
- [IAM](#iam-identity-and-access-management)
- [Compute services](#compute-services)
- [VPCs](#vpcs)
- [Scaling and load balancing](#scaling-and-load-balancing)
- [EBS, EFS, FSx](#ebs-efs-fsx)
- [S3](#s3)
- [Databases](#databases)
- [Networking](#networking)
- [API Gateway](#api-gateway)
- [AppSync](#appsync)
- [Cognito](#cognito)
- [Amplify](#amplify)
- [Elastic Beanstalk](#elastic-beanstalk)
- [Lightsail](#lightsail)
- [App Runner](#app-runner)
- [Copilot](#copilot)
- [SQS](#sqs)
- [SNS](#sns)
- [EventBridge](#eventbridge)
- [SES](#ses)
- [Step Functions](#step-functions)
- [CloudWatch](#cloudwatch)
- [XRay](#xray)
- [CloudTrail](#cloudtrail)
- [GuardDuty](#guardduty)
- [WAF](#waf)
- [Shield](#shield)
- [KMS](#kms)
- [Secrets Manager](#secrets-manager)

## About

**AWS** is a **Cloud Computing Services Provider**.  

It uses **acronymes** of **3 letters** to label its services.  
e.g.: EC2 - Virtual Server inside Physical Server  

Zone - sous zone (subnet)

---

## Advantages of cloud computing

### Trade fixed expenses for variable expenses

Instead of having to invest heavily in data centers and servers before you know how you’re going to use them, you can pay only when you consume computing resources, and pay only for how much you consume.

### Benefit from massive economies of scale

By using cloud computing, you can achieve a lower variable cost than you can get on your own. Because usage from hundreds of thousands of customers is aggregated in the cloud, providers such as AWS can achieve higher economies of scale, which translates into lower pay as-you-go prices.

### Stop guessing capacity

 Eliminate guessing on your infrastructure capacity needs. When you make a capacity decision prior to deploying an application, you often end up either sitting on expensive idle resources or dealing with limited capacity. With cloud computing, these problems go away. You can access as much or as little capacity as you need, and scale up and down as required with only a few minutes’ notice.

### Increase speed and agility

In a cloud computing environment, new IT resources are only a click away, which means that you reduce the time to make those resources available to your developers from weeks to just minutes. This results in a dramatic increase in agility for the organization, since the cost and time it takes to experiment and develop is significantly lower.

### Stop spending money running and maintaining data centers

Focus on projects that differentiate your business, not the infrastructure. Cloud computing lets you focus on your own customers, rather than on the heavy lifting of racking, stacking, and powering servers.

### Go global in minutes

Easily deploy your application in multiple regions around the world with just a few clicks. This means you can provide lower latency and a better experience for your customers at minimal cost.

---

## Regions and AZs

**Regions** has at least 3 **Availability Zones**  
**Availability Zones** has at least 1 **Data Center**  

**Regions** => **Availability Zones** => **Data Centers**

---

## Access AWS Services

- Management Console
- AWS CLI (#<https://aws.amazon.com/cli/>)
- SDKs (write code that will run commands)

### AWS CLI

- [Install](#install)
- [Connect](#connect)

#### Install

```bash
curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
unzip awscliv2.zip
sudo ./aws/install
aws --version
```

#### Connect

<https://docs.aws.amazon.com/cli/latest/userguide/cli-configure-quickstart.html>

`aws configure`

---

## Pricing

Use the **AWS Pricing Calculator** to estimate the cost of the architecture solution.

### Billing

Access the **Billing Dashboard** to get details about the different costs and expenses of the services used.

### Budgets

Access **Bugets** from the **Pricing** console to manage the expenses.  
It's also possible to set up a threshold and get an alert email when that threshold is almost reached.

---

## Services

### Service quotas

It is possible to increase some services quotas by going to the **Services Quotas** page.

---

## Security

Shared Responsibility Model: AWS is responsible for the things we can't control. We are responsible for the account protection and the settings we implement.  

- Choose a strong password, change it frequently and don't share credentials.  
- Use Multi-Factor Authentication (MFA)

---

## IAM (Identity and Access Management)

The **root** account should never be used because it has unlimited rights. Create an **IAM** user instead and use it.  
The **programmatic access keys** should be **deleted**.

- Identities
- Access management

### Identities

By default, a newly created user doesn't have any rights.  

- **Users:** a list of users with specific permissions. All users can have totally different permissions.  
- **User groups:** a group will have certain permissions and any user/entity added to this group will have the same rights.  
- **Roles:** roles concern Services that can interact with some other services

### Access management

**Access management** are basically **permissions** (managed via **policies**) given to users, user groups and roles.

---

## Compute services

**Compute Services** allow the **execution of code** in the cloud (computing workload).  

- [EC2](#ec2)
- [ECS/EKS](#ecseks)
- [ECR](#ecr)
- [Fargate](#fargate)
- [Lamba](#lambda)
- [AMIs](#amis)

### EC2

**EC2** or **Elastic Compute Cloud**. Rent a virtual remote machine ("a computer in the cloud").  
**AWS Data Center**(Physical machine) => **Virtual Server** or **EC2 Instances**(virtual machine). Typically a virtual machine **using a slice** of a physical machine.

Those instances are fully configurable: choose **hardware profile**, **operating system** and install any **software**.

### ECS/EKS

**Elastic Container Service** / **Elastic Kubernetes Service**. Those services are an alternative to EC2.  
They are configured to host Containers.

### ECR

**Elastic Container Registry** is a **Managed Container Image Repository** where images can be managed and distributed.

### Fargate

**Serverless Container Execution Environment** used to host containers.

### Lambda

-[Code](#code)
-[Event](#event)
-[Configuration](#configuration)

A **Serverless Compute service** that allows to run code without configuring or controlling any servers.  
It's focused on event-driven code.  
Meant to execute code that stops after a few seconds, not meant to be used for long running tasks.

#### Code

We upload or define the code that should be executed by writing the code in the console, upload a ZIP file or Docker container.  
We choose a programming language and set up environment variables if needed.

#### Event

Choose a supported event source (e.g. a file was uploaded to S3).

#### Configuration

Configure how long the code should last, what file system to use, what are the roles attached and wheter a VPC is connected (if the lambda function needs to communicate with another AWS service)

### AMIs

**Amazon Machine Images** or **AMIs** define the base operating system, software and configuration of an EC2 instance. It will decide which type of server is running (Amazon Linux, Linux, Windows, Red Hat, etc...).  
The **instance type** of an EC2 instance defines its hardware profile and capabilities.  
The **security group** is a firewall attached to an EC2 instance, controlling network access (SSH, https, http...)

---

## VPCs

- [About VPCs](#about-vpcs)
- [Subnets](#subnets)
- [CIDR IP](#cidr-ip)
- [Elastic IPs](#elastic-ips)
- [VPCs peering and transit gateways](#vpcs-peering-and-transit-gateways)
- aws endpoint private links ???

### About VPCs

**Virtual Private Clouds**  contain **subnets** who in turn contain services like EC2.  
An EC2 instance is always part of a VPC.  
**VPCs** help in managing netork traffic between certain AWS services and the internet.

### Subnets

A single **Subnet** always belong to exactly one **AZs**.  
A **private** subnet has no direct connectivity with the internet.  
A **public** subnet has access to an **internet gateway**.

### CIDR IP

**Classless Inter-Domain Routing** is a notation for IP address ranges.  
CIDR blocks describe IP address ranges, typically used for defining available private IP addresses in private networks.  

An IP address is a 32-bit number:  
`172.31.0.0` => here each number is made of 8-bit (4x8-bit).  
`172.31.0.0/16` => the number **16** means that 16 bits are fixed. Which means that the range of that IP address is **171.31.0.0 <> 171.31.255.255**. Giving us **65.536 available addresses**.  
That notation is usefull for computers or EC2 instances to be able to communicate with each other inside a private network.  
=> IP addresses starting with _172_ are usually Private addresses.  
**Public addresses are rare**. They are not enough available IP addresses for all the devices in the world. But not all devices need a public access.  

### Elastic IPs

**Elastic IPs** are static / fixed public IP addresses which you can request from AWS & assign to EC2 isntances. Usused Elastics Ips will be charged as IPs are rare.

### NACLs

**NACLs** control access to an entire subnet, as opposed to security groups that control access to a single instance.  

### VPCs peering and transit gateways

**VPC peering** helps with connecting muliple VPCs.

---

## Scaling and load balancing

- [Auto Scaling](#auto-scaling)
- [ELB](#elb)

### Auto Scaling

**Scaling** refers to adjusting the amount of availbale IT resources (e.g. to keep up with increasing traffic).  
**Auto Scaling** automatically adds or removes EC2 insctances based on confitions / metrics previously defined by the user.  

### ELB

**Elastic Load Balancer** is a service that distributes loads evenly accross available instances:  

- **Application Load Balancer**: used for most HTTP apps and limited to HTTP(s) traffic.
- **Network Load Balancer**: use for non-HTTP services

## EBS, EFS, FSx

- [EBS](#ebs)
- [EFS](#efs)
- [FSx](#fsx)

### EBS

**Elastic Block Store** provides a virtual, unformatted hard drive that can be mounted to an EC2 instance.

### EFS

**Elastic File System** provides a scalable file system that can be used by many AWS services.  
It will scale automatically.  
Usefull for files that need to be accessed by many EC2 instances or services.

### FSx

Same as EFS but it focuses on high-performance scenarios & use-cases.

---

## S3

**Simple Storage Service** is an Object Storage service.  
Files are stored in **Buckets** and can be further organized via file prefixes. They can be **uploaded** from **any source** as long as the source has **appropriate permissions**. The same goes for downloading.  
The Public Access must explicitly be enabled by disabling **"Block Public"** and adding a **bucket policy**.

**S3 classes** help reduce costs by picking the right class for a given file access pattern.

**Glacier storage classes** are used for long-term data archiving.

**Lifecycle policies** help deal with canging file access patterns.

S3 can **host static websites**.  

Private ???
Public ???

---

## Databases

- [Self-hosted Databases](#self-hosted-databases)
- [Managed Databases](#managed-databases)
- [RDS](#rds)
- [RDS](#rds)
- [Caching Databases](#caching-databases)
- [Dynamo DB](#dynamo-db)
- [Other database services](#other-database-services)
- [AWS Backup](#aws-backup)

### Self-hosted Databases

When choosing this option the user has to install and operate the database softwares manually. It has full control but also full responsability: keep software patched, manage back-ups, etc...

### Managed Databases

In a **managed database**, AWS manages the setup and database operations. The user has less control but also less responsability.  
They are basically softwares running on top of EC2 instances that are hidden from the user.

### RDS

**Relational Database Service** is a managed SQL database.  
As a user we need to **choose** a **database engine** (MySQL, PostgreSQL), a **database version** and **setup the auto-update settings**.  
We need to choose the hardware and network configuration: instance class, VPC, subnet and security group.  
Set up the server configurations: login credential and port. Replication (for high availability and performance), monitoring settings, encryption and backup settings.  

### Aurora

**Aurora** is **Amazon's** own **Relational Database Engine**.  
It is **faster** than the other RDS services.  
It also has a **serverless** version that will only run when needed.

### Caching Databases

AwS offers **managed Caching databases** that can be used to cache some data and serve them faster to the client.  
Available options are **Redis** and **Memcached**.  
The setup is similar to a regular managed database service.

### Dynamo DB

**Dynamo DB** is a fully managed NoSQL key-value database.  
It differs from RDS or Aurora as we don't need to create a database. We only need to create **tables**.

### Other database services

- MemoryDB => persistent in-memory storage
- DocumentDB => document database
- Keyspaces => wide column database
- Neptune => graph database
- Timestream => time series database
- Quantum Ledger Database => immutable log of data changes

### AWS Backup

AWS Backup is a service for **Centralized Backup Management**. It helps in managing multiple database services.

---

## Networking

- [DNS](#dns)
- [Route 53](#route-53)
- [CDN](#cdn)
- [CloudFront](#cloudfront)
- [Local Zones](#local-zones)
- [Outposts](#outposts)
- [Wavelength Zones](#wavelength-zones)
- [Global Accelerator](#global-accelerator)
- [S3 Transfer Acceleration](#s3-transfer-acceleration)
- [ACM](#acm)

### DNS

A **Domain Name System** translates domain names to IP addresses.

### Route 53

**Managed DNS Service** that helps in managing **Domains** and **DNS**.  
We can register or transfer domains as well as create hosted zones (config containers).  
We can create routing records and manage failover or create complex routing rules.

### CDN

A **Content Delivery Network** is a network of servers that distributes content from an _origin_ server throughout the world by caching content close to where each end user is accessing the internet. The content they first request is first stored on the origin server and is then replicated and stored elsewhere as needed.

### CloudFront

Managed CDN that uses AWS' **Edge Locations**.  
It manages Content Origins by creating a distribution connected to content sources, defining distribution behaviors, logging, SSL and security settings.  
It delivers and caches content by creating caching policies(containing cache rules), connecting caching policies to distribution and using functions for request/response manipulation(advanced feature).

### Local Zones

Smaller AWS regions close to big metropolitan areas, perfect for achieving ultra-low latency.  
It has a limited set of supported services and can extends VPCs to local zones.

### Outposts

AWS-managed infrastructures to the company's on-premises.  
It also has a limited set of supported services and can extends VPCs to local zones.

### Wavelength Zones

AWS services embedded into 5G networks. Perfect for achieving lowest latency possible.  
It also has a limited set of supported services and can connect to other services running in a region.

### Global Accelerator

Improves user traffic performance via AWS network. Receives two unique, stable IP addresses.  
Useful for balancing multi-region traffic.

### S3 Transfer Acceleration

Improves data transfer speed via AWS edge network. Less network variability, more banwidth utilization.

### ACM

**Amazon Certificate Manager** is used to request **SSL Certificates**. They are issued by AWS and are free to use for AWS services.  
A custom certificate can also be imported.  
The SSL certificate can then be used with a variety of AWS services (ALB, NLB, CloudFront...).  
It handles SSL encryption on the requests and therefore encryts network traffic.

---

## API Gateway

**API Gateway** is a **Managed REST API Service**. It is used to build REST APIs.  
It can serve as a back-end service to communicate with systems running on EC2, AWS Lambda or any public addressable web service.

---

## AppSync

**Managed GraphQL API Service** that helps with **GrapQL APIs**.

---

## Cognito

**Managed App User Authentication Service** that manages app users.  
Uses **Managed User Pools** where credentials and authentication experience can be configured.  
It can integrate with any applications and assign temporary IAM permissions to users.  
It allows **Social Sign In** by creating federated identity pools and adding Google, Facebook, Apple etc authentication.

---

## Amplify

**Amplify** is an **Application Development Platform and Framework** that is focused on the product and front-end developers.  
It creates insfractructure on the user's behalf(other services). It also integrates with client-side code via SDK.  
The user can **build a Backend** or **Host an Application** by letting Amplify managing services and app data and using **Amplify Studio** to configure the backend and manage data.

---

## Elastic Beanstalk

Simplified and easy to use service for deploying and scaling web applications and services.  
The **configuration of a web app** is done in a **single space**.

---

## Lightsail

Simplified EC2 Management Console.

---

## App Runner

Service that simplifies the process of deploying containers and uses ECS and Fargate under the hood.

---

## Copilot

A CLI that simplifies container app creation and deployment.  
It is used to create cloud environments and deploy apps.

---

## SQS

**Simple Queue Service** is a managed message(data package) queue service. It pushes and reads messages to the queue.  
It is an **asynchronous process** and is an equivalent to **RabbitMQ**.

---

## SNS

**Simple Notification Service** is a managed push message(data package) service. It pushes messages directly to interested subscribers.  
It is a **synchronous** process.

---

## EventBridge

Service that listens to event and triggers actions. It can react to any kind of events and trigger other services.
It is a **synchronous** process.

---

## SES

**Simple Email Service** is used to send transcational or batch emails to users.  
It is **NOT** used for intra-service communication.

---

## Step Functions

**Reliable Pre-defined Execution Sequences**. Defines steps inputs, outputs and code to be executed.

---

## CloudWatch

**Managed Monitoring Service** that creates application logs by **collecing data**, analyses them and then acts accordingly(e.g. creating alerts). CloudWatch is where all the logs appear.

**Metrics** can be set up to trigger **Alarts**. Metrics can be the number of bytes stored in a bucket per day. It the number of bytes reach a certain quantity then an alert can be triggered and an email can be sent thanks to SNS.

### Dashboards

Dashboards can be created in CloudWatch to display different data of multiple selected services on the same page.

---

## XRay

XRay is an In-app Request Flow Tracing.

---

## AWS Batch

---

## CloudTrail

**Tracks User Activity** and **API Usage** by logging the account activity.

---

## GuardDuty

---

## WAF

Is a firewall that supports HTTP request content-based rules.

---

## Shield

Protecting against **DDos** (Distributed Denial of Service) attacks. A DDos attack happens when too many requests are made to a server, leading in that server crashing as it can't handle that many requests.

---

## KMS

Encrypting data at rest.

---

## Secrets Manager

Securely stores secret values. Secret values can be accessed from the application code (e.g. database credentials can be stored with Secrets Manager to avoid writing them directly into the code).

---

## Security Hub

---

## SSO

**Single Sign-On** allows signing into multiple accounts via one pair of credentials.

---

## Directory Service

Allows companies to connect their DA(Active Directory) workflows to AWS.

---

## Artifact

Allows you to access & download AWS compliance reports.

---

## Audit Manager

Proves your application / service usage compliance.

---

## Detective

Helps analyse incidents.

---

## Firewall Manager & Security Hub

Centralized Security Management services.

---

## Cloud9

Cloud-based IDE.

---

## CodeCommit

AWS's Git repository.

---

## CodeBuild

---

## CodeGuru & DevOpsGuru

Provide ML-based improvement suggestions.

---

## Well-Architected framework

---

## Acceptable Use Policy

---

## CodeDeploy

---

## Health Dashboard

Check this page to stay up to date with the states of the services used.  
