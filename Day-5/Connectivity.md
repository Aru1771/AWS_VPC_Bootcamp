☁️ AWS VPC – Day 5: Connectivity

Today's topics:

VPC Peering
Transit Gateway
Site-to-Site VPN
Direct Connect
AWS PrivateLink
VPC Endpoints
Gateway Endpoint
Interface Endpoint
1. Why Do We Need Connectivity?

Imagine you have multiple networks:

VPC-A
10.0.0.0/16

VPC-B
10.1.0.0/16

On-Premises
192.168.0.0/16

By default:

VPC-A  ❌  VPC-B

They cannot communicate just because both are inside AWS.

We need connectivity mechanisms.

2. VPC Peering

Suppose you have only two VPCs:

VPC-A
10.0.0.0/16
    │
    │ Peering
    │
    ▼
VPC-B
10.1.0.0/16

VPC Peering creates a private network connection between two VPCs.

Traffic stays on the AWS network.

Example
Application VPC
10.0.0.0/16
      │
      │ VPC Peering
      ▼
Database VPC
10.1.0.0/16

Your application can privately communicate with resources in the other VPC.

Important

Creating the peering connection alone isn't enough.

You need routes.

VPC-A route table:

Destination       Target

10.0.0.0/16       local

10.1.0.0/16       VPC Peering

VPC-B:

Destination       Target

10.1.0.0/16       local

10.0.0.0/16       VPC Peering
3. Problem With VPC Peering

Imagine you have:

VPC-A
  │
  ├── VPC-B
  │
  ├── VPC-C
  │
  ├── VPC-D
  │
  └── VPC-E

With peering, you need many individual connections.

For example:

A ─── B
A ─── C
A ─── D
A ─── E
B ─── C
B ─── D
...

This becomes difficult to manage.

That's where Transit Gateway comes in.

4. Transit Gateway

Think of Transit Gateway as a central network router.

              VPC-A
                │
                │
                ▼
         ┌───────────────┐
         │   Transit     │
         │   Gateway     │
         └───────────────┘
          │      │      │
          ▼      ▼      ▼
        VPC-B  VPC-C  VPC-D

Instead of connecting every VPC to every other VPC:

VPC-A ──┐
VPC-B ──┤
VPC-C ──┼── Transit Gateway
VPC-D ──┤
VPC-E ──┘

Each VPC connects to the Transit Gateway.

5. Production Example

Suppose your company has:

Production VPC
Development VPC
Testing VPC
Security VPC
Shared Services VPC

Architecture:

              Production
                  │
                  │
Development ── Transit Gateway ── Security
                  │
                  │
             Shared Services
                  │
                  │
                Testing

This is much easier to manage at scale.

6. VPC Peering vs Transit Gateway
VPC Peering	Transit Gateway
Direct connection	Central router
Good for few VPCs	Good for many VPCs
Point-to-point	Hub-and-spoke
More connections at scale	Centralized routing
No transitive routing	Supports transitive routing
Easy memory:
2 VPCs → Peering

Many VPCs → Transit Gateway
7. Site-to-Site VPN

Now imagine your company has an on-premises data center.

Company Data Center
192.168.0.0/16
        │
        │ Internet
        │
        ▼
       AWS
        │
        ▼
VPC
10.0.0.0/16

How can on-premises communicate privately with AWS?

Use AWS Site-to-Site VPN.

8. VPN Architecture
        ON-PREMISES
        Data Center
             │
             │
        VPN Device
             │
             │ Encrypted VPN Tunnel
             │
          Internet
             │
             │
             ▼
       AWS Virtual
       Private Gateway
             │
             ▼
            VPC

The traffic travels over the internet but is encrypted through the VPN tunnel.

9. Example

Your company has:

On-Premises
10.20.0.0/16

AWS:

VPC
10.0.0.0/16

VPN allows:

10.20.0.0/16
       │
       │ VPN
       ▼
10.0.0.0/16

Applications can communicate privately between the networks.

10. Direct Connect

Now imagine your company has very important production workloads.

You don't want your AWS traffic to travel over the public internet.

You can use:

AWS Direct Connect

It provides a dedicated network connection from your on-premises environment to AWS.

On-Premises
Data Center
     │
     │ Dedicated Connection
     │
     ▼
AWS Direct Connect
     │
     ▼
AWS
     │
     ▼
VPC
11. VPN vs Direct Connect
VPN
On-Prem
   │
   │ Internet
   │ Encrypted Tunnel
   ▼
AWS
Direct Connect
On-Prem
   │
   │ Dedicated Connection
   ▼
AWS
VPN	Direct Connect
Uses internet	Dedicated connection
Encrypted tunnel	Private connectivity
Faster to deploy	More setup
Generally lower cost	More expensive
Good for many use cases	Good for high-throughput/consistent connectivity

A common production design is actually:

Direct Connect
      +
VPN backup
12. PrivateLink

Now let's look at a different problem.

Suppose Company A provides a service:

Payment Service

Company B wants to consume it.

You don't necessarily want to connect the entire VPCs.

Instead:

Consumer VPC
     │
     │
     ▼
AWS PrivateLink
     │
     ▼
Provider Service

PrivateLink provides private access to a specific service without requiring full network connectivity between the VPCs.

13. Easy Difference
VPC Peering
VPC A  ←────────→  VPC B

Network-to-network connectivity.

PrivateLink
VPC A
  │
  │
  ▼
Specific Service

Service-to-consumer connectivity.

14. VPC Endpoints

Now imagine your private EC2 needs to access:

Amazon S3

Without a VPC endpoint:

Private EC2
    │
    ▼
NAT Gateway
    │
    ▼
Internet
    │
    ▼
S3

But AWS provides VPC Endpoints.

Then:

Private EC2
    │
    ▼
VPC Endpoint
    │
    ▼
S3

Traffic can stay within the AWS network instead of requiring a NAT Gateway for that service.

15. Two Important Types

There are two main types you need to know:

VPC Endpoint
│
├── Gateway Endpoint
│
└── Interface Endpoint
16. Gateway Endpoint

Gateway endpoints are used for:

S3
DynamoDB

Architecture:

Private EC2
     │
     ▼
Route Table
     │
     ▼
Gateway Endpoint
     │
     ▼
S3

You configure a route to the endpoint.

17. Example: S3

Suppose your private EC2 needs to download something from S3.

Without endpoint:

EC2
 │
 ▼
NAT Gateway
 │
 ▼
S3

With Gateway Endpoint:

EC2
 │
 ▼
Route Table
 │
 ▼
S3 Gateway Endpoint
 │
 ▼
S3

This can reduce NAT Gateway usage and costs.

18. Interface Endpoint

Interface endpoints use:

Elastic Network Interface (ENI)

inside your subnet.

Architecture:

Private Subnet

EC2
 │
 ▼
ENI
 │
 ▼
Interface Endpoint
 │
 ▼
AWS Service

They are powered by AWS PrivateLink.

19. Examples of Interface Endpoints

Many AWS services can be accessed through interface endpoints.

For example:

EC2
ECR
CloudWatch
STS
Secrets Manager
SSM
KMS

The exact services available depends on the AWS region/service support.

20. Your EKS Example

This is especially useful for your EKS knowledge.

Imagine your EKS nodes are in private subnets.

They need:

ECR
STS
CloudWatch
S3
Secrets Manager

Instead of sending everything through NAT Gateway:

EKS Node
   │
   ▼
NAT Gateway
   │
   ▼
Internet
   │
   ▼
AWS Service

you can use VPC endpoints:

                   EKS Node
                      │
          ┌───────────┼────────────┐
          ▼           ▼            ▼
       S3 Endpoint  ECR Endpoint  STS Endpoint
          │           │            │
          ▼           ▼            ▼
         S3          ECR           STS

This is a very useful production networking pattern.

21. Gateway vs Interface Endpoint
Gateway Endpoint	Interface Endpoint
S3, DynamoDB	Many AWS services
Uses route tables	Uses ENI
No hourly endpoint charge	Hourly/data processing charges generally apply
Doesn't use PrivateLink	Uses AWS PrivateLink
Simple routing	DNS/ENI-based access
22. Complete Day 5 Picture

Now put everything together:

                         AWS
                          │
          ┌───────────────┼────────────────┐
          │               │                │
          ▼               ▼                ▼
       VPC Peering   Transit Gateway    PrivateLink
          │               │                │
          ▼               ▼                ▼
      Another VPC     Many VPCs        Specific Service
                                          │
                                          ▼
                                      Consumer VPC


On-Premises
    │
    ├──────── VPN ──────────► AWS VPC
    │
    └──── Direct Connect ──► AWS

And inside a VPC:

Private EC2
     │
     ├──────────────► NAT Gateway ──► Internet
     │
     ├──────────────► Gateway Endpoint ──► S3/DynamoDB
     │
     └──────────────► Interface Endpoint ──► AWS Services
🧠 The Easy Memory Trick

Remember the problem each service solves:

VPC Peering
    ↓
Connect 2 VPCs


Transit Gateway
    ↓
Connect MANY networks/VPCs


VPN
    ↓
Connect On-Premises ↔ AWS over encrypted tunnel


Direct Connect
    ↓
Dedicated On-Premises ↔ AWS connection


PrivateLink
    ↓
Privately consume a SPECIFIC service


VPC Endpoint
    ↓
Private access to AWS services


Gateway Endpoint
    ↓
S3 / DynamoDB


Interface Endpoint
    ↓
Many AWS services using ENIs/PrivateLink
🎤 Interview Questions
1. VPC Peering vs Transit Gateway?

VPC Peering provides direct point-to-point connectivity between VPCs, while Transit Gateway acts as a centralized network hub for connecting multiple VPCs, VPNs, and other networks.

2. VPN vs Direct Connect?

VPN uses an encrypted tunnel over the internet, while Direct Connect provides a dedicated private network connection between an on-premises environment and AWS.

3. VPC Peering vs PrivateLink?

VPC Peering provides network-level connectivity between VPCs, while PrivateLink provides private access to a specific service without exposing the entire provider VPC.

4. Gateway Endpoint vs Interface Endpoint?

Gateway endpoints provide private access to S3 and DynamoDB through route tables. Interface endpoints create ENIs in your subnets and use AWS PrivateLink to provide private access to supported AWS services.
