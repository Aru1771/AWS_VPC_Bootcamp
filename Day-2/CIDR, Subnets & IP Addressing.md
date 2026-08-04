CIDR, Subnets & IP Addressing
==============================

🎯 Goal
---------
By the end of today, you'll understand:

What is CIDR?
CIDR Notation
Network ID
Host ID
Subnet Mask
Public vs Private Subnets
AWS Reserved IP Addresses
Subnet Sizing
Production Best Practices
Interview Questions



Why Do We Need CIDR?
------------------------------
Imagine your company has:

500 Servers

Every server needs a unique IP address.

Instead of assigning IPs randomly, we allocate a block of IP addresses.

Example:

10.0.0.0/16

This block contains thousands of IP addresses that you can divide into subnets.


What is CIDR?
-----------------
CIDR stands for:

Classless Inter-Domain Routing

CIDR defines:

The network portion of an IP address.
The host portion of an IP address.

Example:

10.0.0.0/16

The /16 tells us how many bits belong to the network.


Understanding CIDR Notation
--------------------------------
Example:

192.168.1.0/24

Break it down:

IP Address

192.168.1.0

CIDR

/24

This means:

First 24 bits = Network

Remaining 8 bits = Hosts

Visual representation:

32 Bits Total

11111111.11111111.11111111.00000000
       24 Bits              8 Bits


Common CIDR Blocks
------------------

| CIDR | Total IPs | Usable in AWS* |
| ---- | --------: | -------------: |
| /16  |    65,536 |         65,531 |
| /17  |    32,768 |         32,763 |
| /18  |    16,384 |         16,379 |
| /19  |     8,192 |          8,187 |
| /20  |     4,096 |          4,091 |
| /21  |     2,048 |          2,043 |
| /22  |     1,024 |          1,019 |
| /23  |       512 |            507 |
| /24  |       256 |            251 |
| /25  |       128 |            123 |
| /26  |        64 |             59 |
| /27  |        32 |             27 |
| /28  |        16 |             11 |


Note: AWS reserves 5 IP addresses in every subnet.


Network ID and Host ID
-----------------------
Example:

192.168.10.0/24

Network portion:

192.168.10

Host portion:

0 - 255

Visualization:

192.168.10.0

│───────────│───────│

 Network      Hosts


What is a Subnet?
------------------
A subnet is a smaller network created from a larger CIDR block.

Suppose your VPC CIDR is:

10.0.0.0/16

You can divide it into:

10.0.1.0/24

10.0.2.0/24

10.0.3.0/24

10.0.4.0/24

Each subnet has its own IP range.


Public and Private Subnets
------------------------------
Public Subnet
---------------
A public subnet has a route to an Internet Gateway (IGW).

Example:

0.0.0.0/0

↓

Internet Gateway

Resources:

Bastion Host
Public ALB
NAT Gateway


Private Subnet
---------------
A private subnet has no direct route to the Internet Gateway.

Resources:

Application Servers
Databases
Internal Services
EKS Worker Nodes (commonly)


AWS Reserved IP Addresses
--------------------------
AWS reserves five IP addresses in every subnet.

Example:

Subnet:

10.0.1.0/24

Reserved:

| IP         | Purpose                     |
| ---------- | --------------------------- |
| 10.0.1.0   | Network Address             |
| 10.0.1.1   | VPC Router                  |
| 10.0.1.2   | Amazon DNS                  |
| 10.0.1.3   | Reserved                    |
| 10.0.1.255 | Broadcast (reserved by AWS) |

Usable:
--------
10.0.1.4

↓

10.0.1.254

So a /24 subnet has:

256 Total IPs

↓

251 Usable IPs


Production Example
---------------------
Suppose you're designing a production VPC.

VPC:
---
10.0.0.0/16

Subnets:

Public Subnet A

10.0.1.0/24

Public Subnet B

10.0.2.0/24

Private App A

10.0.11.0/24

Private App B

10.0.12.0/24

Private DB A

10.0.21.0/24

Private DB B

10.0.22.0/24

This design provides separation between public-facing resources, application servers, and databases.



🧪 Hands-on Lab
----------------
Create a VPC:

CIDR:
10.0.0.0/16

Create these subnets:

| Subnet        | CIDR         |
| ------------- | ------------ |
| Public-A      | 10.0.1.0/24  |
| Public-B      | 10.0.2.0/24  |
| Private-App-A | 10.0.11.0/24 |
| Private-App-B | 10.0.12.0/24 |
| Private-DB-A  | 10.0.21.0/24 |
| Private-DB-B  | 10.0.22.0/24 |


For now, just create the VPC and subnets. We'll connect them with Route Tables, Internet Gateway, and NAT Gateway in the next session.

📚 Homework
Create the VPC and six subnets in your AWS account.
Verify the CIDR ranges don't overlap.
Calculate the total and usable IP addresses for:
/24
/25
/26
Explain why application and database servers should be placed in private subnets.
