🎯 Day 4 Objectives
======================

By the end of today, you should understand:

Local routes
Internet routing
NAT routing
Private routing
Route propagation
Longest Prefix Match
How to troubleshoot routing problems
Hands-on routing lab

1. First: What is Routing?
   ----------------------
Routing means:

Deciding where network traffic should go based on the destination IP address.

Imagine an EC2 instance:

EC2
10.0.1.10
   |
   ↓
Route Table
   |
   ↓
Where should the packet go?

The route table contains:

Destination       Target
10.0.0.0/16       local
0.0.0.0/0         Internet Gateway

If the EC2 wants to communicate with:

10.0.2.20

AWS checks:

10.0.2.20
    ↓
Matches 10.0.0.0/16
    ↓
local

So the packet stays inside the VPC.

2. Local Route
   -----------
When you create a VPC:

VPC CIDR:
10.0.0.0/16

AWS automatically creates:

Destination       Target
10.0.0.0/16       local

This is called the local route.

It allows communication between resources inside the VPC.

For example:

        VPC 10.0.0.0/16
               |
       ┌───────┴───────┐
       ↓               ↓
Public Subnet      Private Subnet
10.0.1.0/24        10.0.2.0/24
       |               |
       EC2             EC2
10.0.1.10             10.0.2.10

Traffic:

10.0.1.10 → 10.0.2.10

matches:

10.0.0.0/16 → local

Therefore:

EC2 → local route → EC2

No Internet Gateway is required.

3. Internet Routing
   -----------------
Now suppose your EC2 wants to access:

8.8.8.8

The destination isn't inside:

10.0.0.0/16

So the route table needs:

Destination       Target
10.0.0.0/16       local
0.0.0.0/0         igw-id

The important route is:

0.0.0.0/0

This means:

Any IPv4 destination that doesn't match a more specific route.

Traffic:

EC2
10.0.1.10
   |
   ↓
Route Table
   |
   | 0.0.0.0/0
   ↓
Internet Gateway
   |
   ↓
Internet

4. Public Subnet
   ----------------
A subnet is considered public when its route table has a route to an Internet Gateway.

Example:

Destination       Target
10.0.0.0/16       local
0.0.0.0/0         igw-123456

Notice:

A subnet is not public merely because the EC2 has a public IP.

The subnet's route table must provide a path to the Internet Gateway.

5. NAT Routing
   ------------
Now let's look at the classic production architecture.

                 Internet
                    ↑
                    |
             Internet Gateway
                    |
             Public Subnet
                    |
                NAT Gateway
                    |
             Private Subnet
                    |
                   EC2

Private EC2:

10.0.2.10

needs to download something from the Internet.

The private subnet route table might contain:

Destination       Target
10.0.0.0/16       local
0.0.0.0/0         nat-123456

Therefore:

Private EC2
    |
    ↓
Private Route Table
    |
    | 0.0.0.0/0
    ↓
NAT Gateway
    |
    ↓
Internet Gateway
    |
    ↓
Internet

6. Very Important: NAT Gateway Does NOT Receive Internet-Initiated Traffic
   ------------------------------------------------------------------------------
NAT Gateway provides outbound Internet access for private resources.

Example:

Private EC2
    |
    | HTTPS request
    ↓
NAT Gateway
    ↓
Internet

But an Internet user cannot normally initiate a connection directly to:

Private EC2

through the NAT Gateway.

That's one of the reasons we use private subnets.

7. Public vs Private Route Table
   -----------------------------
Public
Destination       Target
10.0.0.0/16       local
0.0.0.0/0         Internet Gateway
Private
Destination       Target
10.0.0.0/16       local
0.0.0.0/0         NAT Gateway

This is a very common interview question.

8. Private Routing
   ------------------
Private routing doesn't necessarily mean:

"The instance cannot communicate with anything outside the subnet."

It usually means the resource does not have a direct route through an Internet Gateway for Internet access.

For example:

Private EC2
    |
    +---- VPC local
    |
    +---- NAT Gateway → Internet
    |
    +---- Transit Gateway → another VPC
    |
    +---- VPN → on-premises

So private routing can involve many destinations.

9. Route Propagation
   ------------------
Now let's discuss route propagation.

This is especially important with:

VPN
Virtual Private Gateway
Transit Gateway

Suppose your company has:

AWS VPC
10.0.0.0/16

and an on-prem network:

192.168.0.0/16

You create a VPN connection.

The route table may learn:

Destination       Target
192.168.0.0/16    vgw-123456

Instead of manually adding every route, routes can be propagated from the attached gateway.

Conceptually:

On-Prem
192.168.0.0/16
      |
      ↓
VPN
      |
      ↓
Virtual Private Gateway
      |
      ↓
Route Propagation
      |
      ↓
VPC Route Table

10. Longest Prefix Match 🔥
    ----------------------
This is one of the most important routing concepts today.

Suppose your route table contains:

Destination       Target
10.0.0.0/16       local
10.0.1.0/24       nat-gateway
0.0.0.0/0         igw

Now the destination is:

10.0.1.50

Which route is selected?

Both:

10.0.0.0/16

and:

10.0.1.0/24

match.

AWS chooses:

10.0.1.0/24

Why?

Because /24 is more specific than /16.

That's Longest Prefix Match.

11. Think of It Like This
    -----------------------
Destination: 10.0.1.50

0.0.0.0/0
      ↓
Very broad

10.0.0.0/16
      ↓
More specific

10.0.1.0/24
      ↓
Most specific

AWS chooses:

10.0.1.0/24

12. Another Example
    -----------------
Route table:

10.0.0.0/16     local
10.0.1.0/24     NAT Gateway
10.0.1.50/32    Firewall
0.0.0.0/0       IGW

Destination:

10.0.1.50

All four may appear relevant, but the winner is:

10.0.1.50/32

because:

/32 > /24 > /16 > /0

The more specific route wins.

🧠 Day 4 Mental Model

Whenever an EC2 sends traffic, think:

EC2
 |
 ↓
Subnet Route Table
 |
 ↓
Destination IP
 |
 ↓
Find matching routes
 |
 ↓
Longest Prefix Match
 |
 ↓
Target
 |
 ├── local
 ├── IGW
 ├── NAT Gateway
 ├── Transit Gateway
 ├── VPC Peering
 ├── VPN / VGW
 └── Network Firewall
🧪 Day 4 Hands-On Lab

Now I want you to build and reason about this, rather than just read it.

Task

Create this architecture:

                    VPC
                10.0.0.0/16
                     |
          ┌──────────┴──────────┐
          ↓                     ↓
     Public Subnet         Private Subnet
     10.0.1.0/24           10.0.2.0/24
          |                     |
        EC2-A                 EC2-B
          |                     |
          ↓                     ↓
         IGW                 NAT Gateway
                                |
                                ↓
                               IGW
Requirements

Public subnet route table:

10.0.0.0/16 → local
0.0.0.0/0   → Internet Gateway

Private subnet route table:

10.0.0.0/16 → local
0.0.0.0/0   → NAT Gateway

Then test:

Test 1

From EC2-A:

curl https://checkip.amazonaws.com

Should work.

Test 2

From EC2-B:

curl https://checkip.amazonaws.com

Should also work if the NAT Gateway and routing are correctly configured.

Test 3

From EC2-B, determine its route:

ip route

Understand that the Linux routing table and AWS VPC route table are different layers.

🎯 Your Day 4 Questions

Answer these in your own words before looking anything up:

Q1

What is a local route?

Q2

What makes a subnet public?

Q3

What is the difference between an Internet Gateway and NAT Gateway?

Q4

Why does a private subnet need a NAT Gateway to access the Internet?

Q5

Can the Internet directly initiate a connection to an EC2 in a private subnet through a NAT Gateway? Why?

Q6

What is route propagation?

Q7

What is Longest Prefix Match?

Q8 🔥

Given this route table:

Destination        Target
10.0.0.0/16        local
10.0.1.0/24        nat-gateway
10.0.1.50/32       firewall
0.0.0.0/0          igw

Where will traffic destined for:

10.0.1.50

go?

Q9 🔥

Where will traffic destined for:

10.0.2.50

go?

Q10 — Production Interview Question

An EC2 instance in a private subnet cannot access the Internet.

List the things you would check, in order, to troubleshoot it.

Start with the route table, not the security group.
