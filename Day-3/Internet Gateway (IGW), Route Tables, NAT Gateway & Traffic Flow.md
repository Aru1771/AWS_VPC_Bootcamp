🌐 Internet Gateway (IGW), Route Tables, NAT Gateway & Traffic Flow
=====================================================================

🎯 Goal
---------
By the end of today, you'll understand:

What is an Internet Gateway (IGW)?
What is a Route Table?
What is a Public Subnet?
What is a Private Subnet?
What is an Elastic IP (EIP)?
What is a NAT Gateway?
Complete Internet Traffic Flow
Production Architecture
Hands-on Implementation

Step 1: We Already Have a VPC
---------------------------------
From Day 2, we created:

VPC
10.0.0.0/16

│
├── Public Subnet A
│      10.0.1.0/24
│
├── Public Subnet B
│      10.0.2.0/24
│
├── Private App A
│      10.0.11.0/24
│
├── Private App B
│      10.0.12.0/24
│
├── Private DB A
│      10.0.21.0/24
│
└── Private DB B
       10.0.22.0/24

Question:

Can these EC2 instances access the Internet?

Answer: ❌ No.

Because the VPC is isolated by default.

Step 2: What is an Internet Gateway (IGW)?
----------------------------------------------
An Internet Gateway is a managed AWS service that connects your VPC to the public Internet.

Think of it like the main gate of your house.

Your House

↓

Main Gate

↓

Road

↓

World

Similarly:

VPC

↓

Internet Gateway

↓

Internet

Without an IGW:

Internet

❌ Cannot Reach

VPC

Step 3: Attach IGW to VPC
----------------------------
Create an Internet Gateway.

Attach it to the VPC.

Architecture:

Internet
     │
     ▼
Internet Gateway
     │
     ▼
VPC

Important:

Attaching an IGW alone does not make instances public.

You also need Route Tables.

Step 4: What is a Route Table?
-------------------------------
A Route Table is like Google Maps for your subnet.

It tells AWS:

"If traffic is going here, send it there."

Example:

Destination        Target

10.0.0.0/16       Local

0.0.0.0/0         Internet Gateway

Meaning:

Traffic inside VPC

↓

Stay Local

-------------------

Traffic to Internet

↓

Internet Gateway

Step 5: Public Route Table
----------------------------
Example:

Destination	Target
10.0.0.0/16	Local
0.0.0.0/0	Internet Gateway

Associate this Route Table with:

Public Subnet A
Public Subnet B

Now these subnets become Public Subnets.

Step 6: Public Subnet Traffic Flow
----------------------------------
Suppose a user opens:

https://app.company.com

Flow:

Internet User
      │
      ▼
Internet
      │
      ▼
Internet Gateway
      │
      ▼
Public Route Table
      │
      ▼
Public Subnet
      │
      ▼
Application Load Balancer

Step 7: What is a Private Subnet?
-------------------------------------
A Private Subnet has no route to the Internet Gateway.

Example Route Table:

Destination	Target
10.0.0.0/16	Local

Notice:

There is no:

0.0.0.0/0 → Internet Gateway

So:

Internet

↓

Cannot Reach

↓

Private EC2

Step 8: Problem
-------------------
Suppose your Spring Boot application is in a private subnet.

It needs to:

Download Maven dependencies
Install OS updates
Pull Docker images
Access AWS APIs

How will it access the Internet?

It cannot use an Internet Gateway directly because it's in a private subnet.

Step 9: NAT Gateway
---------------------
AWS provides a NAT Gateway.

NAT stands for:

Network Address Translation

It allows outbound Internet access for private resources.

Step 10: Why NAT Gateway Needs a Public Subnet
----------------------------------------------

A NAT Gateway itself must be placed in a Public Subnet.

It also requires an Elastic IP (EIP).

Architecture:

Internet

↓

Internet Gateway

↓

Public Subnet

↓

NAT Gateway (Elastic IP)

↓

Private Subnet

↓

Spring Boot EC2


Step 11: What is an Elastic IP?
-------------------------------
An Elastic IP is a static public IPv4 address provided by AWS.

Why?

The NAT Gateway needs a public IP to communicate with the Internet.

NAT Gateway

↓

Elastic IP

↓

Internet

Step 12: Private Subnet Route Table
---------------------------------------
Instead of:

0.0.0.0/0

↓

Internet Gateway

We configure:

0.0.0.0/0

↓

NAT Gateway

Route Table:

Destination	Target
10.0.0.0/16	Local
0.0.0.0/0	NAT Gateway

Associate this Route Table with:

Private App A
Private App B


Step 13: Complete Traffic Flow
-------------------------------
Inbound Traffic
User

↓

Internet

↓

Internet Gateway

↓

Public Route Table

↓

Public ALB

↓

Private App

↓

Database
Outbound Traffic from Private App
Spring Boot EC2

↓

Private Route Table

↓

NAT Gateway

↓

Internet Gateway

↓

Internet

Notice:

The Internet cannot initiate connections to the private EC2.

Only the private EC2 can initiate outbound connections.

Step 14: Production Architecture
---------------------------------------

                      Internet
                          │
                          ▼
                  Internet Gateway
                          │
        ┌─────────────────┴─────────────────┐
        ▼                                   ▼
 Public Subnet A                      Public Subnet B
 ┌──────────────┐                    ┌──────────────┐
 │ Public ALB   │                    │ NAT Gateway  │
 └──────────────┘                    └──────────────┘
         │                                  ▲
         ▼                                  │
 Private App A                     Private App B
 ┌──────────────┐                  ┌──────────────┐
 │ Spring Boot  │                  │ EKS Nodes    │
 └──────────────┘                  └──────────────┘
         │
         ▼
 Private DB
 ┌──────────────┐
 │ Amazon RDS   │
 └──────────────┘

 
Hands-on Lab
--------------
Step 1

Create an Internet Gateway.

Step 2

Attach the Internet Gateway to your VPC.

Step 3

Create a Public Route Table.

Add:

Destination:
0.0.0.0/0

Target:
Internet Gateway

Associate it with:

Public Subnet A
Public Subnet B
Step 4

Allocate an Elastic IP.

Step 5

Create a NAT Gateway.

Place it in Public Subnet A
Assign the Elastic IP
Step 6

Create a Private Route Table.

Add:

Destination:
0.0.0.0/0

Target:
NAT Gateway

Associate it with:

Private App A
Private App B
Step 7

Launch:

One EC2 in a Public Subnet
One EC2 in a Private Subnet

Verify:

Public EC2 is reachable via SSH (if Security Groups and public IP are configured).
Private EC2 cannot be reached directly from the Internet.
Private EC2 can install packages using the NAT Gateway:
sudo yum update -y

or

sudo apt update

Interview Questions
-----------------------
Q1. What is the purpose of an Internet Gateway?

Answer:

An Internet Gateway enables communication between a VPC and the Internet. It allows resources in public subnets to send and receive internet traffic when the appropriate route table and public IP configuration are in place.

Q2. Does attaching an Internet Gateway make all subnets public?

Answer:

No. A subnet becomes public only when:

It has a route (0.0.0.0/0) pointing to the Internet Gateway.
The instance has a public IP (or Elastic IP if applicable).
Security Groups and Network ACLs allow the traffic.
Q3. Why is a NAT Gateway used?

Answer:

A NAT Gateway allows resources in private subnets to initiate outbound internet connections (for updates, package downloads, AWS API calls, etc.) while preventing inbound connections initiated from the Internet.

Q4. Why does a NAT Gateway require an Elastic IP?

Answer:

The NAT Gateway resides in a public subnet and communicates with the Internet through the Internet Gateway. It requires a static public IP (Elastic IP) so external services can return traffic to it.

📝 Homework

Build this architecture in your AWS account:

VPC
│
├── Internet Gateway
├── Public Route Table
├── Public Subnet
│      ├── ALB
│      └── NAT Gateway (Elastic IP)
│
├── Private Route Table
├── Private App Subnet
│      └── EC2
│
└── Private DB Subnet
       └── RDS

Verify:

Public EC2 has Internet access.
Private EC2 has outbound Internet access through the NAT Gateway.
Private EC2 cannot be accessed directly from the Internet.
