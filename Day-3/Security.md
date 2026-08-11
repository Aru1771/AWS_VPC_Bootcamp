Today we move from VPC networking into how AWS controls traffic inside the VPC.

Your Day 3 topics are:

Security Groups
NACLs
Stateful vs Stateless
Security Group Referencing
Ephemeral Ports
Production Security Design


1. First understand the big picture
   ---------------------------------
   Imagine you have:
   Internet
   |
   v
Internet Gateway
   |
   v
Public Subnet
   |
   v
ALB
   |
   v
Private Subnet
   |
   v
EC2 Application Server
   |
   v
Database Subnet
   |
   v


There are different security controls protecting these resources.

The two important VPC controls are:

              VPC
               |
       -----------------
       |               |
  Security Group     NACL
       |               |
   Resource level    Subnet level
Simple difference

Security Group → protects the resource

NACL → protects the subnet

Remember this first.

2. Security Group
   ----------------

   A Security Group, or SG, is a virtual firewall attached to AWS resources such as:

EC2
ALB
RDS
other supported network interfaces

It controls traffic going into and out of the resource.

For example:

EC2
 |
 +---- Security Group

Suppose your EC2 is a web server.

You might configure:

Inbound

HTTP   80     0.0.0.0/0
HTTPS  443    0.0.0.0/0
SSH    22     Your-IP/32

Meaning:

Anyone
  |
  | HTTP/HTTPS
  v
EC2

But SSH:

Your laptop
    |
    | SSH :22
    v
   EC2

Only your IP is allowed.

3. Security Groups are allow-only
   -----------------------------
This is very important.

Security Groups have:

ALLOW rules

They don't have explicit DENY rules.

For example:

Inbound:

ALLOW TCP 443 from 0.0.0.0/0

means HTTPS is allowed.

You cannot add:

DENY TCP 443 from 1.2.3.4

to a Security Group.

If you need explicit deny behavior, that's where NACLs become useful.

4. Security Group is STATEFUL
   -------------------------
This is one of today's most important concepts.

Suppose:

Client
  |
  | TCP 443
  v
EC2

Your Security Group allows:

Inbound TCP 443

The EC2 sends a response:

EC2
 |
 | Response
 v
Client

You do not need to create a separate outbound rule specifically for that response.

Why?

Because Security Groups are:

Stateful

The SG remembers the connection.

5. Easy example of stateful behavior
   ---------------------------------
   Suppose your SG says:

Inbound:
ALLOW TCP 443 from Internet

Client:

Client ---> EC2:443

SG says:

443 allowed ✅

EC2 responds:

EC2 ---> Client

SG understands:

"This is the response to an already allowed connection."

So:

Response allowed ✅

6. NACL
   -----
Now let's look at the Network ACL.

NACL stands for:

Network Access Control List

It is associated with a subnet.

Think:

Subnet
  |
  +---- NACL
          |
          +---- Controls traffic entering/leaving subnet

For example:

Public Subnet
     |
    NACL
     |
    ALB



NACL works at subnet level
------------------------------
This is the key difference.

Security Group
Resource level

Example:

EC2
 |
SG
NACL
Subnet level

Example:

Subnet
 |
NACL
 |
Resources


8. NACL allows AND denies
   -------------------------
Unlike Security Groups, NACLs support:

ALLOW
DENY

For example:

Rule 100
ALLOW TCP 443 from 0.0.0.0/0

Rule 110
DENY TCP 443 from 1.2.3.4/32

The deny rule can explicitly block traffic.

9. NACL is STATELESS
   ----------------
This is the second major concept.

Suppose:

Client
  |
  | Request
  v
Server

NACL allows the incoming request.

The server sends a response:

Server
  |
  | Response
  v
Client

NACL doesn't automatically remember the connection.


10. Stateful vs Stateless — easiest way
    ------------------------------------
Remember this:

Security Group
      ↓
   STATEFUL
      ↓
Remembers connection
NACL
      ↓
  STATELESS
      ↓
Doesn't remember connection
You must make sure the return traffic is also allowed.

That's because:

NACL = Stateless


11. Real example
    -------------
Suppose:

Client
  |
  | HTTPS :443
  v
ALB

ALB responds from an ephemeral port.

This brings us to today's next important topic.

What is an Ephemeral Port?
--------------------------
This sounds complicated but is actually simple.

A client needs a source port when creating a TCP connection.

For example:

Client:52341
     |
     | TCP 443
     v
Server:443

Here:

52341 = client-side temporary port
443   = server listening port

That temporary client port is called an:

Ephemeral Port

Why do we need ephemeral ports?
---------------------------------
Imagine you open a website:

https://example.com

Your computer needs a source port.

It might choose:

52341

So the connection becomes:

192.168.1.10:52341
        |
        | ---> TCP 443
        |
10.0.1.20:443

Another connection might use:

52342

Another:

52343

These are temporary ports.


Why are ephemeral ports important with NACL?
----------------------------------------------
This is a very common DevOps interview question.

Imagine:

Client
10.0.1.10:52341
       |
       | Request
       v
Server
10.0.2.10:443

The request goes:

Source port = 52341
Destination port = 443

The server response goes the opposite direction:

Source port = 443
Destination port = 52341

Notice:

Request:
52341 → 443

Response:

443 → 52341

Therefore, the NACL needs to allow the appropriate return traffic.

15. Security Group vs NACL
    ------------------------
    | Feature         | Security Group                                   | NACL                          |
| --------------- | ------------------------------------------------ | ----------------------------- |
| Scope           | Resource                                         | Subnet                        |
| Rules           | Allow                                            | Allow + Deny                  |
| Stateful?       | ✅ Yes                                            | ❌ No                          |
| Rule evaluation | All applicable rules                             | Lowest numbered rule first    |
| Explicit deny   | ❌ No                                             | ✅ Yes                         |
| Return traffic  | Automatically allowed for established connection | Must be explicitly allowed    |
| Common use      | Protect EC2/ALB/RDS                              | Subnet-level boundary/control |


16. Security Group Referencing
     ------------------------
    This is one of the most useful AWS concepts.

Suppose you have:

Internet
   |
   v
ALB
   |
   v
EC2
   |
   v
RDS

Don't do this:

EC2 SG:
ALLOW 8080 from 0.0.0.0/0

That's dangerous.

Instead, create separate Security Groups:

ALB-SG
APP-SG
DB-SG

17. ALB → Application
    -----------------
ALB Security Group:

ALB-SG

Inbound:
443 from Internet

Application Security Group:

APP-SG

Inbound:
8080 from ALB-SG

Notice:

APP-SG
  |
  | Allow 8080
  | from
  v
ALB-SG

We are referencing the Security Group, not an IP address.

Why is SG referencing better?
-------------------------------
Without SG reference:

APP-SG

ALLOW 8080
from:
10.0.1.0/24

You're trusting an entire subnet.

With SG reference:

APP-SG

ALLOW 8080
from:
ALB-SG

Meaning:

Only resources associated with ALB-SG can access this port.

That's much cleaner.

19. Application → Database
    -----------------------

    Now suppose:

ALB
 |
 v
Application
 |
 v
RDS

Application listens on:

8080

RDS MySQL listens on:

3306

We create:

ALB-SG
APP-SG
DB-SG

Rules:

ALB-SG
Inbound:
443 from Internet
APP-SG
Inbound:
8080 from ALB-SG
DB-SG
Inbound:
3306 from APP-SG

So the traffic path becomes:

Internet
   |
   | 443
   v
ALB
[ALB-SG]
   |
   | 8080
   v
Application
[APP-SG]
   |
   | 3306
   v
RDS
[DB-SG]

This is a very good production design.

20. Why not allow DB from 0.0.0.0/0?
    --------------------------------
Bad:

DB-SG

3306
0.0.0.0/0

This potentially exposes MySQL to the Internet.

Instead:

DB-SG

3306
Source = APP-SG

Now:

Internet ❌
ALB ❌
Random EC2 ❌
Application servers ✅

This follows the principle:

Least Privilege

Only allow what is actually required.

21. Production Security Design
    -----------------------------
     Now let's combine everything.

     A common production architecture:
                        INTERNET
                       |
                       v
                Internet Gateway
                       |
                       v
                PUBLIC SUBNET
                       |
                       v
                     ALB
                   [ALB-SG]
                       |
                       |
                PRIVATE SUBNET
                       |
                       v
                  APP SERVERS
                   [APP-SG]
                       |
                       |
                PRIVATE SUBNET
                       |
                       v
                    RDS
                   [DB-SG]

    Security Groups:
    ALB-SG
  |
  +-- 443 from Internet

APP-SG
  |
  +-- 8080 from ALB-SG

DB-SG
  |
  +-- 3306 from APP-SG

  This creates a controlled chain:

Internet
   ↓
ALB
   ↓
Application
   ↓
Database

22. What should NOT happen?
    ------------------------------

    
❌ Don't expose application servers directly

Bad:

Internet
   |
   v
EC2

Better:

Internet
   |
   v
ALB
   |
   v
Private EC2



❌ Don't expose RDS publicly

Bad:

Internet
   |
   v
RDS:3306

Better:

Application
     |
     v
RDS:3306


❌ Don't allow SSH from anywhere

Bad:

22
0.0.0.0/0

Better:

22
Your trusted IP

Or, in a production design, use a controlled administrative access path rather than broadly exposing SSH.

23. Production NACL Design
    ----------------------
For most applications, you don't need to make NACL rules unnecessarily complicated.

Remember:

Security Group
=
Primary resource-level firewall

and:

NACL
=
Subnet-level additional control

You can use NACLs for additional subnet boundaries and explicit deny requirements.

For example, you may want to block known unwanted traffic at the subnet boundary.

24. Very important interview scenario
Question:

An EC2 instance can connect to an RDS database, but the response traffic is failing. The Security Groups look correct. What could you check?

Think:

SG
 ↓
NACL
 ↓
Ephemeral ports
 ↓
Route tables
 ↓
Network connectivity

Because NACLs are stateless, return traffic must be allowed.

You should check:

EC2 subnet NACL
RDS subnet NACL

and make sure the required traffic in both directions is permitted.

25. One more important point: Rule evaluation

Security Groups:

Allow rules

All applicable rules are evaluated together.

For example:

Rule 1:
ALLOW 443

Rule 2:
ALLOW 80

Both can apply.

NACLs are different.

They use rule numbers.

Example:

100  ALLOW TCP 443
110  DENY  TCP 443

Rule 100 is evaluated first and traffic matching it is allowed.

So:

NACL = Rule number matters

Lower rule number → evaluated first.

🧠 Day 3 — Simple memory trick

Remember:

SG = Security Group
    ↓
Resource
    ↓
Stateful
    ↓
Allow only
NACL
    ↓
Subnet
    ↓
Stateless
    ↓
Allow + Deny
    ↓
Rule number matters

And:

SG Reference
    ↓
ALB-SG → APP-SG → DB-SG

And:

Ephemeral Port
    ↓
Temporary client-side port
    ↓
Important for return traffic
🎯 Day 3 Production Architecture

This is the design I want you to be able to explain in an interview:

                         INTERNET
                            |
                            |
                     Internet Gateway
                            |
                            v
                    +---------------+
                    |      ALB      |
                    |    ALB-SG     |
                    +---------------+
                            |
                         :8080
                            |
                            v
                    +---------------+
                    |  APP SERVERS  |
                    |    APP-SG     |
                    +---------------+
                            |
                         :3306
                            |
                            v
                    +---------------+
                    |      RDS      |
                    |     DB-SG     |
                    +---------------+

ALB-SG:
443  ← Internet

APP-SG:
8080 ← ALB-SG

DB-SG:
3306 ← APP-SG

This is the core production security pattern you should remember.

📝 Day 3 Practice — Your Turn

Don't look at the answers. Explain these in your own words.

Q1. What is a Security Group?
Q2. What is a NACL?
Q3. What is the difference between Security Group and NACL?
Q4. What does stateful mean in a Security Group?
Q5. What does stateless mean in a NACL?
Q6. What is an ephemeral port?
Q7. Why are ephemeral ports important when configuring NACLs?
Q8. What is Security Group referencing?
Q9. Why is this better?
DB-SG:
3306 from APP-SG

instead of:

DB-SG:
3306 from 0.0.0.0/0
Q10. Design the Security Groups for:
Internet
   ↓
ALB
   ↓
EC2 Application
   ↓
RDS MySQL

Tell me which ports and sources you would allow for ALB-SG, APP-SG, and DB-SG.

Once you answer these, I'll correct them one by one like an AWS DevOps interview and then we'll do the Day 3 hands-on VPC lab.
