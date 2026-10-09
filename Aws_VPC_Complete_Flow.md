AWS VPC:
---------
* VPC Is region scoped if you create a vpc in specific region the vpc only available in that region only

* one IGW we can attch to one VPC

* VPC CIDR range always in b/w the \16 to \18


       if you take less than 16 we will get more than 65,536 Ip address.
       if you take more than 28 we will get very less Ip address


* IP address in real world


        Always make sure give the ip with 10, 192, 172 use these 3 three only in real time. ----> recomened
        we can use other ip's as well like 11,12,13 but thise kind of ip makes some issues when we are config vpn.

* in real world we always update our Doc's with our VPC cidr blocks. if any one creating new VPC they will check these doc's before creating the vpc.it will
  help us to don't overlap our VPC CIDR ranges.
   

* we can add more than one CIDR block to our VPC:


        if required we can add more then one cidr block but the sequence is same.
        like fiest cidr is 172.31.0.0\16
        the second cider is like 172.34.0.0.\16

* If we create a VPC what are the default things we will get:


       1. default route table
       2. default NACL
       3. default security group

* DHCP option set in vpc: 

       by default we will get default dns servers from aws.
       but in real time for active driectires we have to create cutome dns servers.


* My Project CIDR ranges:

        10.100.0.0/16 --- dev
        10.120.0.0/16 -- stage
        10.140.0.0/16 -- prod

* What is CIDR:

      CIDR means: CLassless-inter Domine Routings.
      CIDR is a mentiod for allocating the IP addresses and for ip routing

       If i took 10.100.0.0/16 as a CIDR:

       we have to devide that in to three parts:

       10.100.0.0 ---> is the IP address
       / --> slash
       16 --> Decimal Number

       Combination of (/) slash and (16) Decimal Number will called it as Subnet Mask

* IP address is the 32 bit number that qniquely identify the a host on a tcp ip network.

       IPV4 Ip has 4 bytes in that 4 bytes each byte contains 8 bits so total we have 32 bits.

       how we can calculate it ?
        
       Eg: 192.168.0.1
       
       every byte is separated by (.)
       
       --------.--------.--------.--------
       
       8 bits  | 8 bits  | 8 bits  | 8 bits 

        Take one byte as eg:
  
        1     1    0    0     0   0    0    0 ---> this is the total 8 bits i have in that single byte
        -     -    -    -     -   -    -    - 
       2po7 2po6 2po5 2po4 2po3 2po2 2po1 2po0  -----> the value is 192 = in this byte i have to calculate the bit which have the value 1 we can ignore 0 so for 1         we have 2po7 and 2po6. 


CIDR-Tables to remember:

Most important CIDR cheat sheet:
              
              | CIDR | Host bits | Total IPs | Equivalent /24s |
              |---|---:|---:     |------------:|------------------
              | `/16`  | 16      | 65,536    | 256 |
              | `/17` | 15       | 32,768    | 128 |
              | `/18` | 14       | 16,384    | 64 |
              | `/19` | 13       | 8,192     | 32 |
              | `/20` | 12       | 4,096     | 16 |
              | `/21` | 11       | 2,048     | 8 |
              | `/22` | 10       | 1,024     | 4 |
              | `/23` | 9        | 512       | 2 |
              | `/24` | 8        | 256       | 1 |
              | `/25` | 7        | 128       | 1/2 |
              | `/26` | 6        | 64        | 1/4 |
              | `/27` | 5        | 32        | 1/8 |
              | `/28` | 4        | 16        | 1/16 |
              | `/29` | 3        | 8         | 1/32 |
              | `/30` | 2        | 4         | 1/64 |

How we caluculate the Total IP addresses of CIDR:

      Total IP addresses = 2^(32 - CIDR) 

      if my cidr is 10.0.0.0/16 ---> 32-16 = 16 ---> 2^16 = 65536

⭐ Memorize this part:

             /16 = 65,536
              /20 = 4,096
              /21 = 2,048
              /22 = 1,024
              /23 = 512
              /24 = 256
              /25 = 128
              /26 = 64
              /27 = 32
              /28 = 16

After /24, each increase in the prefix halves the IP count.

              /24 → 256
              /25 → 128
              /26 → 64
              /27 → 32
              /28 → 16

The most useful AWS subnet table:

       | CIDR | Total IPs | AWS usable IPs* |
       |---|---:          |---:             |
       | `/16` | 65,536   | 65,531 |
       | `/20` | 4,096    | 4,091 |
       | `/21` | 2,048    | 2,043 |
       | `/22` | 1,024    | 1,019 |
       | `/23` | 512      | 507 |
       | `/24` | 256      | 251 |
       | `/25` | 128      | 123 |
       | `/26` | 64       | 59 |
       | `/27` | 32       | 27 |
       | `/28` | 16       | 11 |
*AWS reserves 5 addresses in each subnet.


/24 is your easiest reference point: 

This is the trick I recommend for you.

Memorize:

       10.0.1.0/24

means:

       10.0.1.0
               ↓
       10.0.1.255


/23:
Two /24s:

       10.0.2.0/23
       
       10.0.2.0/24
       +
       10.0.3.0/24
Range:

       10.0.2.0 → 10.0.3.255

/22:
Four /24s:

       10.0.4.0/22
       
       10.0.4.0/24
       10.0.5.0/24
       10.0.6.0/24
       10.0.7.0/24
Range:

       10.0.4.0 → 10.0.7.255

/21
Eight /24s:

       10.0.8.0/21
       
       10.0.8.0/24
       10.0.9.0/24
       10.0.10.0/24
       10.0.11.0/24
       10.0.12.0/24
       10.0.13.0/24
       10.0.14.0/24
       10.0.15.0/24

Range:

       10.0.8.0 → 10.0.15.255

Subnet boundary trick ⭐:

       | CIDR | Increment in 3rd octet |
       |---    |---                    :|
       | `/16` | 1 |
       | `/17` | 128 in 3rd?* |
       | `/18` | 64 |
       | `/19` | 32 |
       | `/20` | 16 |
       | `/21` | 8 |
       | `/22` | 4 |
       | `/23` | 2 |
       | `/24` | 1 |

For the common /20–/24 range:


       /20 → increments of 16
       /21 → increments of 8
       /22 → increments of 4
       /23 → increments of 2
       /24 → increments of 1

So:

       10.0.2.0/23  ✅
       10.0.4.0/22  ✅
       10.0.8.0/21  ✅
       10.0.16.0/20 ✅

But:

       10.0.3.0/23  ❌
because /23 boundaries are:

       10.0.0.0/23
       10.0.2.0/23
       10.0.4.0/23
       10.0.6.0/23
       ...

Private VPC CIDR ranges ⭐:

For AWS VPCs, remember the three RFC 1918 private ranges:       


       10.0.0.0/8
       172.16.0.0/12
       192.168.0.0/16


Examples:

       10.0.0.0/16        ✅
       172.16.0.0/16      ✅
       192.168.1.0/24     ✅

These are private IP ranges.

VPC → Subnet hierarchy:

Think like this:

       VPC
       │
       ├── 10.0.0.0/16
       │
       ├── Public Subnet
       │   └── 10.0.1.0/24
       │
       ├── Public Subnet
       │   └── 10.0.2.0/24
       │
       ├── Private Subnet
       │   └── 10.0.3.0/24
       │
       └── Private Subnet
           └── 10.0.4.0/24

The subnet must be inside the VPC CIDR and subnet CIDRs cannot overlap.

For example:

       VPC:        10.0.0.0/16
       
       Subnet 1:   10.0.1.0/24  ✅
       Subnet 2:   10.0.2.0/24  ✅
       Subnet 3:   10.0.3.0/24  ✅

But:

       Subnet 1:   10.0.1.0/24
       Subnet 2:   10.0.1.128/25  ❌

because the second subnet is inside the first one.

🧠 Your interview cheat sheet

If you're preparing for AWS/DevOps interviews, remember these 7 things:

       1. IPv4 = 32 bits
       
       2. Total IPs = 2^(32 - prefix)
       
       3. /24 = 256 IPs
       
       4. Smaller prefix = bigger network
          /23 > /24 > /25
       
       5. /23 = 2 × /24
          /22 = 4 × /24
          /21 = 8 × /24
          /20 = 16 × /24
       
       6. Private ranges:
          10.0.0.0/8
          172.16.0.0/12
          192.168.0.0/16
       
       7. AWS subnets cannot overlap.



Aws VPC- Subnets:
-----------------

we used:

    public subnet
    private subnet
    application subnet
    data subnet
    vpn-only-subnets

 public subnet:

        used to host the front end webserver, bastenhost, Natgateway's, 


private subnet:

       used to host some backed servers, database servers, internal servers.


application subnet:

       it's same as a privatesubnet but as a origanization level some times we can call it as a app subnet.

       used to host EKS clusters, middleware services.
data subnet:


        used to host DATABASES like databases.




Block ip in every subnet:
--------------------------

* if you take vpc cidr :                        10.0.0.0./16

* in that vpc if i create any subnet like:      10.0.11.0/24 --> subnet 1
* for the second subnet in the same vpc use --> 10.0.12.0/24.

* Never reduce the value you have given in the subnet. if you reduce it will overlap the cidr blocks.

* Blocked Ip's:

      10.0.0.0 --> for network
      10.0.0.1 --> vpc routing purpose
      10.0.0.2 ---> for DNS
      10.0.0.3 ---> for future reference
      10.255.255.255 --> brod cast 


* Subnet level imp:

       we have to enable auto-assign public ip to assing ip's to ec2 at subnet level.


* IN realworld we always maintain 2 TO 3 public, private, app, db subnets for high availability

       if we created a VPC CIDR BLOCK with 10.100.0.0/16 --it will give 65,536 + IP'S  

       Now if you are creating the subnets first two values will be same "10.100" for the subnets cidr blocks.

       Now we have to define a subnet CIDR block range with the help of CIDR caluculater.

How can we do that ?


      My VPC CIDR range is 10.100.0.0/16

      In my vpc now i want to create the Subnet:

      EG: 1 subnet-1 with cidr block -10.100.8.0/24 

      My Subnet-1 CIDR Range:
                CIDR IP Range
                10.100.8.0 - 10.100.8.255

     in this case i have no issue i can define my another subnet like 10.100.9.0/24

     But if you are takeing the host range is below /24:

     we have to check the cider range in CIDR caluculater  https://mxtoolbox.com/subnetcalculator.aspx.

     
       Eg: 2 subnet-2 with cidr block - 10.100.8.0/21

       My Subnet-2 CIDR Range:
       CIDR IP Range
       10.100.8.0 - 10.100.15.255

        it starts Ip with 10.100.8 and ends Ip with 10.100.15

        in this case if want to create another subnet we have to take the 10.100.16.0/21

        then it will not overlap the CIDR ranges in our subnets.

* Once we created the Subnet we can't modify the CIDR range

* If you create any subnet in vpc it will attached to default route table which we get at the time of VPC creation and By default it is attched to default NACL as    well.


* for public subnets we have to enable auto-assign Ip address Option 


* once we attach the subnets to custome route tables those subnets will automatically deattach from the default route table


* If you want to host the private server from the basten-Host/ Public server:
  
        we have to copy the pem file into the server. then for that pem file we have to give 400.
        How we are connecting private server from public.
        actually we are in the same VPC. and private route table rule we have a local rule.
        this local rule tells i will allow the request from this VPC CIDR range.
        permissions and with the help of CMD: ssh -i key.pem user@private.ip


AWS VPC-SG
-----------

* we configure sg at resource level.
* Hear we can allow or denay the traffic.
* These sg are statefull sets.
* sg will only have allow. it will not have denay.
* if you allow the request at inbound level no need to allow at outbound level because it uses Connection Tracking Mechanism.
* Sg use this Connection Tracking to track information about traffic to and from the instance.
* Sg name must be unique.
* in sg if you allow outbound to allow all traffic with 0.0.0.0/0 is not a problem. but we have to takecare on inbound rules.
* we can give source in inbound rules always custome- with our oraganization cidr range.
* for a single instance we will attach multiple security groups.
* SG at vpc level. we can't access one vpc sg from other vpc's.
* Security Groups are stateful, so if incoming traffic is allowed, the response traffic is automatically allowed back, even if the outbound rule is removed.


AWS Route Table:
-----------------

Route Table = Traffic Map  

* It’s just a list of rules telling your VPC traffic where to go.

Each Route = Destination + Target

* Destination = “Where?” (like 0.0.0.0/0 → the whole internet) from where the traffic is comming

* Target = “How?” (like IGW → Internet Gateway) via which resource the traffic is comming

Local Route Always Exists  

* Every VPC automatically knows how to talk inside itself (10.0.0.0/16 → LOCAL). You can’t delete this.

Main Route Table = Default Map  

* If a subnet doesn’t have its own route table, it follows the main one.


Main Route Table exists by default  

       Every VPC automatically has one main route table.

Controls routing for subnets without custom tables  

       If a subnet doesn’t have its own route table, it follows the main one.

You can edit routes in the main table  

       Add, remove, or modify routes — but the local route (inside VPC) is always there.

Local route cannot be overridden  

       You can’t create a more specific route than the default local route.

Main route table cannot be deleted  

       It’s permanent, but you can replace it with a custom one.

Gateway route tables cannot be set as main  

       Internet Gateway or Virtual Private Gateway route tables are separate and can’t become the main.





AWS NAT Gateway
-----------------

* we use AWS NAT gateway to provide internet access to our private subnet instances.
  1. for software updates.
  2. for using other aws services.
  3. forwards treaffic from the instance in the private subnet to the internet or the other aws services.
  4. sends the responce back to the instance.
 
* AWS offers diff kind of NAT devices.

  1. NAT Gateway
  2. NAT instance

* Aws recomended NAT gateway for better availability.

* Nat gateway is chargeble service in aws.

* AWS charge based on two thingd hourly useage of NAT Gateway and per GB data processing by the NAT gateway.

* which don't have the direct access to internet.

* NAT Gateway are not supported to IPV6 treaffic.

* Evry time create the NAT Gateway in the same AZ so the resources in the same AZ use this NAT Gateway.

 NAT Gateway limitations and Rules:

               1. we can assign only one Elastic IP address with NAT Gateway.
               2. we cannot disassociate an Elastic IP address from the NAT Gateway once it was created.
               3. NAT Gateway supports the following Gateways UDP, TCP and ICMP.
               4. we can not associate the security groups to NAT Gateway.
               5. we can use NACL to control the traffic from and to the subnet.Network ACLs act like a filter at the subnet’s door, deciding which traffic is                       allowed to reach the NAT Gateway and which is blocked.
               6. You cannot send traffic to a NAT Gateway through:VPC Peering (connecting two VPCs), Site-to-Site VPN (on-prem to AWS),  Connect (dedicated AWS                     link)


Table over view of NAT Gatewat:


               | Source IP     | Destination IP | Source Port | Destination Port | Source IP Translated |
              |               |-               |--          -|               ---|-                   --|
              | `192.138.0.3` | `47.12.22.3`   | `53600/TCP` | `80/TCP`         | `32.35.12.22`        |
              | `192.138.0.4` | `47.12.22.3`   | `53601/TCP` | `80/TCP`         | `32.35.12.22`        |
              | `192.138.0.5` | `47.12.22.3`   | `53602/TCP` | `80/TCP`         | `32.35.12.22`        |

 How to remember it:

              Internal Client              NAT/Public IP             Web Server
              
              192.138.0.3:53600  ───────► 32.35.12.22:53600 ─────► 47.12.22.3:80
              192.138.0.4:53601  ───────► 32.35.12.22:53601 ─────► 47.12.22.3:80
              192.138.0.5:53602  ───────► 32.35.12.22:53602 ─────► 47.12.22.3:80


migrating from a NAT instance to a NAT gateway:

              Create NAT Gateway → In the same subnet where your NAT instance exists.
              
              Update Route Table → Change the route that points to the NAT instance so it points to the NAT gateway instead.
              
              Move Elastic IP → Detach the Elastic IP from the NAT instance and attach it to the NAT gateway.


 NAT Instance:

* Purpose → NAT Instance in a public subnet lets private subnet instances send outbound IPv4 traffic to internet or AWS services.

* IPv6 not supported → For IPv6, you must use an egress-only internet gateway.

* Quota → NAT instance limits depend on your EC2 instance quota in that AWS region.

          Example: If your region allows you to run 20 EC2 instances, then you can only have up to 20 NAT instances as part of that quota.

* AMI → Use Amazon Linux AMI named “amzn-ami-vpc-nat” to launch NAT instances.

* Config changes →

* IPv4 forwarding enabled

* ICMP redirects disabled

* Startup script /usr/sbin/configure-pat.sh configures IP settings

👉 In short: NAT Instance = EC2-based NAT for IPv4, limited by instance quota, not for IPv6.

* when we are launcing a NAT instance we have check the stop source and destination check for that NAT instance.


DHCP option Set:
-----------------

Dynamic Host Configration protocol provides a standard for passing configrations information to the host on TCP/IP network.

What DHCP does

Instead of manually configuring:

       IP address
       Subnet mask
       Default gateway
       DNS server

on every computer, a DHCP server automatically provides these network settings.

For example, your laptop connects to a network and says:

       "I need an IP address."

The DHCP server responds:

       "You can use this IP address."

How the flow works
The important thing to remember is DORA:

       D → Discover
       O → Offer
       R → Request
       A → Acknowledge

1. DHCP Discover

A new computer initially doesn't have an IP address.

It sends:

DHCP Discover

Basically:

"Is there any DHCP server available? I need an IP address."

This is normally sent as a broadcast on the local network.

       Client
          |
          | DHCP Discover
          v
       Network

2. DHCP Offer

The DHCP server receives the request and offers an IP address.

For example:

       DHCP Server
            |
            | DHCP Offer
            | IP = 192.168.1.50
            v
       Client

The offer can contain things like:

       IP address      → 192.168.1.50
       Subnet mask     → 255.255.255.0
       Default gateway → 192.168.1.1
       DNS server      → 8.8.8.8
       Lease time      → 8 hours

3. DHCP Request

The client says:

       "Yes, I want that IP address."

So it sends:

DHCP Request

to the DHCP server.

4. DHCP Acknowledge

The DHCP server confirms:

       DHCP ACK

Meaning:

       "Okay. You can use 192.168.1.50."

Now the client has a valid network configuration.

Connecting this to your image

The basic concept shown is:

       Computer
          |
          | "I need an IP"
          v
       Router / Network
          |
          v
       DHCP Server
          |
          | "Here is an IP"
          v
       Computer

The DHCP server is responsible for assigning the IP address.

For example:

Client

       IP: 192.168.1.50

The DHCP server maintains a pool such as:

       192.168.1.50
       192.168.1.51
       192.168.1.52
       192.168.1.53
       ...

When clients request addresses, the server leases available addresses from this pool.

One important correction to the diagram

The image makes it look like the client directly reaches a DHCP server across the Internet.
That's not normally how DHCP works.
DHCP broadcasts are generally local-network broadcasts and routers don't forward those broadcasts by default.
In larger networks, a DHCP Relay Agent is used.

For example:

       Client
          |
          | DHCP Broadcast
          v
       Router / DHCP Relay
          |
          | DHCP Relay
          v
       DHCP Server

The router forwards the DHCP request to the DHCP server.
This is very common in enterprise networks.
Simple real-world example
Imagine your laptop connects to your office Wi-Fi.

Initially:

       Laptop
       IP = ?
       
       It sends:
       DHCP Discover
       
       The DHCP server says:
       DHCP Offer
       
       IP:      10.10.10.25
       Mask:    255.255.255.0
       Gateway: 10.10.10.1
       DNS:     10.10.10.10
       
       Laptop accepts it:
       DHCP Request
       
       Server confirms:
       DHCP ACK
       
       Now:
       Laptop
       IP:      10.10.10.25
       Gateway: 10.10.10.1
       DNS:     10.10.10.10

The laptop can now communicate with other networks through the default gateway.

DHCP option set: 

An AWS DHCP option set provides network configuration to EC2 instances in a VPC through DHCP.

       Domain name
       Domain name servers (DNS)
       NTP servers
       NetBIOS name servers
       NetBIOS node type


1. DNS — very important
   
By default, AWS provides a DNS resolver for the VPC.

For example, if your VPC is:
10.0.0.0/16

the default AWS VPC DNS resolver is typically:
10.0.0.2

You can also configure a custom DNS server in the DHCP option set.

For example:

domain-name-servers = 10.10.10.10

Then EC2 instances in the VPC receive that DNS configuration through DHCP and use the specified DNS server for name resolution.

A common enterprise scenario is:

       EC2
        |
        | DNS query
        v
       Custom DNS
       10.10.10.10
        |
        +----> Corporate domain
        |
        +----> Internal services
        |
        +----> Forward external queries
       
For example:

       app.company.local
       database.company.local
       jenkins.company.local

can be resolved through the organization's DNS infrastructure.       

Domain name / hostname

This part needs a small correction.
The DHCP option set can specify the domain name that is provided to instances.
For example:

       domain-name = company.internal

This doesn't mean the DHCP option set itself creates DNS records.
Think of it as providing the DNS/domain configuration, while DNS itself is responsible for resolving names.
So distinguish:

       DHCP Option Set
               |
               | provides DNS/domain configuration
               v
       EC2 instance
               |
               | DNS query
               v
       DNS server
               |
               | resolves hostname
               v
       IP address


"A DHCP option set in AWS allows us to provide network configuration parameters to resources in a VPC through DHCP. The important parameters from a DevOps perspective are DNS and NTP. By default, AWS provides a VPC DNS resolver, but in an enterprise environment we can configure custom DNS servers through the DHCP option set so instances can resolve internal corporate domains. We can also specify NTP servers for time synchronization. NetBIOS settings are mainly relevant to Windows-based environments."


Network Access Control List (ACL):
----------------------------------

* A network access control list (ACL) is an optional layer of security for your VPC that acts as a firewall for controlling traffic in and out of one or more        subnets.

NACL Basics:

* VPC automatically comes with a modifiable default network ACL.
* By default, it allows all inbound and outbound IPv4 traffic.
* You can create a custom network ACL and associate it with a subnet.
* Each subnet in your VPC must be associated with a network ACL.
* You can associate a network ACL with multiple subnets:
* A subnet can be associated with only one network ACL at a time.
* A network ACL contains a numbered list of rules.
* Rules are evaluated in order of the number of the rule.
* The highest number that you can use for a rule is 32766.
* You can create rules like 100, 150, 200, 250. --> this pattern is recomended by aws.or increment by 10.
* Network ACL has separate inbound and outbound rules, and each rule can either allow or deny traffic.
* Network ACLs are stateless.

         |Resource               | Default |
         | ---                   | ---     |
         | Network ACLs per VPC  | 200     |
         | Rules per network ACL | 20      |

* Network ACLs per VPC → 200 : You can have up to 200 separate NACLs in one VPC.
* Rules per Network ACL → 20 : Each NACL can have up to 20 inbound rules and 20 outbound rules (so 40 total).
* AWS NACL quotas can be increased by submitting a request through the AWS Service Quotas console or AWS Support. Smaller increases are often auto‑approved, while    larger ones require manual review.

* in NACL we have to allow both ingress and engress then only the traffic will comein and go out.
* in my NACL i allowd ingress rules 100 for http with port 80 and TCP protocol and source is 0.0.0.0/0 it will allow all IP address and in egress rules i allowd     rule with 120 for http with port 80 and TCP protocol and source is 0.0.0.0/0. but with out allowing the Ephemeral Ports it won't work.

Ephemeral Ports

* An ephemeral port is a short‑lived transport protocol port used for IP communications.
* The client initiating the request chooses the ephemeral port range.
* The range depends on the client’s operating system.

Port Ranges by System:

       * Amazon Linux kernel → 32768–61000
       * Elastic Load Balancing requests → 1024–65535
       * Windows (up to Server 2003) → 1025–5000
       * Windows Server 2008 and later → 49152–65535
       * NAT Gateway → 1024–65535
       * AWS Lambda functions → 1024–65535

Eg:

if i make a request to the server with my IP at that time my OS will atomatically add one source port to my IP based on the OS. eg: 42.892.83.1:32777 --> ip with source. Now the request flow and reach the NACL and check the http is allowd or not and ephemeral ports are opned or not. if both are allowed the request will flow forward.

       | **Type**     | **Source IP** | **Source Port** | **Destination IP** | **Destination Port** |
       | ---          | ---           | ---             | ---                | ---                  |
       | **REQUEST**  | 32.12.22.11   | 32770           | 42.1.2.10          | 443                  |
       | **RESPONSE** | 42.1.2.10     | 443             | 32.12.22.11        | 32770                |


       In the request:

       Source IP: 32.12.22.11 → Source Port: 32770
       
       Dest. IP: 42.1.2.10 → Dest. Port: 443
       
       In the response:
       
       Source IP: 42.1.2.10 → Source Port: 443
       
       Dest. IP: 32.12.22.11 → Dest. Port: 32770

now the server process the request and send back the request at the time of responding back it will check in the NACL egress is this http traffic is allowd and the ephemeral ports are allowd or not.

Scenario: 1

     ✅ Improved Explanation:  
    
      In ingress rules, you must allow both the server’s destination port (e.g., 443) and the client’s ephemeral port range so that the incoming request can reach       the server.
              
     In egress rules, you don’t need to specify the server’s port (443) because, in egress traffic, that port becomes the source port. Instead, you must allow the      destination ports, which correspond to the client’s ephemeral port range, to ensure the response can return to the client.

Scenario: 2

       If you configure ingress to allow only the server’s destination port (e.g., 443), and configure egress to allow only the client’s ephemeral port range, the        communication will still succeed.
       
       Ingress: The server port (443) is open, so requests can reach the server.
       
       Egress: The client’s ephemeral ports are allowed, so the server’s response can return to the client.
       
       You don’t need to explicitly allow the server port in egress, because in outbound traffic that port is the source port, not the destination.


✅ Working Scenarios

       Ingress: server port + client ephemeral range  
       Egress: client ephemeral range → Works (full coverage).
       
       Ingress: only server port  
       Egress: only client ephemeral range → Works (minimal but sufficient).
       
       Ingress: server port + client ephemeral range  
       Egress: all ports allowed → Works (looser but fine).

❌ Non‑Working Scenarios

       Ingress: only server port  
       Egress: only server port (443) → Fails (client ephemeral not allowed).
       
       Ingress: only client ephemeral range  
       Egress: only client ephemeral range → Fails (server port not allowed in ingress).
       
       Ingress: deny ephemeral range  
       Egress: allow ephemeral range → Fails (request blocked before reaching server).

* Always allow server port in ingress.

* Always allow client ephemeral range in egress.


AWS VPC-Peering:
-----------------

* VPC-Peering is the service we use to establesh a connection b/w the two vpc's

* Perring means the method allows two networks to coonect and exchage the traffic directly without having to pay a third party to carry traffic accross the          network.

* AWS uses the existing infrastructure of a VPC to create a VPC peering connection:

       Sharing data across accounts becomes easier.
       
       Sharing data across instances across VPCs becomes easier.
       
       We can establish peering relationships between VPCs across different AWS Regions (Inter‑Region VPC Peering).
       
       Communication with EC2, RDS, Lambda is possible without needing:
       • Gateways
       • VPN connections
       • Separate network appliances
       
       All traffic remains in the Private IP Space.

Steps to Establish VPC Peering Connection:

       Requester VPC sends a peering request to the accepter VPC.
       
       Accepter VPC approves the request.
       
       Both VPC owners update route tables to allow traffic between them.
       
       Update security groups (and optionally DNS resolution).
       
       Important: If instances use public hostnames for communication, you must enable DNS resolution in the VPC peering settings.
       
       Note: CIDR blocks of the two VPCs must not overlap.

VPC Peering Connection Lifecycle:

       Initiating – Request → moves to Pending Acceptance.
       
       Pending Acceptance can lead to:
       
       Provisioning (if accepted)
       
       Expired (if not accepted within 7 days)
       
       Rejected
       
       Provisioning can lead to:
       
       Active (connection established)
       
       Deleting
       
       Active → can later move to Deleting (either party can delete, including inter‑region peering).
       
       Deleting → becomes Deleted.
       
       Failed, Expired, Rejected, Deleted → all transition to No Longer Visible.

Visibility Notes

       Failed: visible for 2 hours to requester.
       
       Expired: visible for 2 days to both parties.
       
       Rejected: visible for 2 days to requester, 2 hours to accepter.
       
       Deleted: visible for 2 hours to the one who deleted, 2 days to the other party.


No Support – Overlapping CIDR Blocks

* AWS does not support VPC peering connections if the CIDR blocks overlap.

Case 1

       VPC‑1 CIDR: 10.0.0.0/16
       
       VPC‑2 CIDR: 10.0.0.0/16 → ❌ Not allowed

Case 2

       VPC‑1 CIDR: 10.3.0.0/16
       
       VPC‑2 CIDR: 10.2.0.0/16 → Allowed

🧠 Sleep‑easy summary:

       VPC peering requires non‑overlapping CIDR ranges.
       
       If CIDRs overlap, peering connection cannot be established.


Example scenarios:

Scenario: 1

Multiple VPC Peering Connection


       * VPC peering is a one‑to‑one relationship between two VPCs.
       
       * There is no support for transitive peering (you cannot connect VPC‑1 to VPC‑3 through VPC‑2).
       
       * Each VPC must establish a direct peering connection with the other if communication is required.


🧠 Sleep‑easy summary:

       One‑to‑one only.
       
       No transitive peering.
       
       Direct connections needed for each pair.

Scenario: 2

Edge to Edge Routing Through a VPN or AWS Direct Connect

* AWS does not support edge‑to‑edge routing between VPCs and external networks when using VPN or Direct Connect.

Example:

       VPC‑1 ↔ VPC‑2 → connected via VPC peering.
       
       VPC‑2 ↔ Corporate Network → connected via Site‑to‑Site VPN or Direct Connect.
       
       VPC‑1 ❌ cannot directly route to the Corporate Network through VPC‑2.

Scenario: 3

* Edge to Edge Routing Through an Internet Gateway
* AWS does not support edge‑to‑edge routing through an Internet Gateway.

Example:

       VPC‑1 ↔ VPC‑2 → connected via VPC peering.
       
       VPC‑1 ↔ Internet Gateway → connected to the internet.
       
       VPC‑2 ❌ cannot directly route to the internet through VPC‑1’s Internet Gateway.

VPC Peering – Things to Remember

       * No overlapping CIDRs → VPCs must have different IP ranges.
       
       * No transitive peering → You can’t connect VPC‑A → VPC‑B → VPC‑C. Each must connect directly.
       
       * Only one peering per pair → You can’t create multiple peering links between the same two VPCs.
       
       * Tags are local → The labels (tags) you add only show up in your own account/region. They don’t travel to the other VPC.
       
       * No DNS in peer VPC → You can’t use Amazon’s DNS server from the other VPC. Each VPC uses its own DNS.

Default Limits

       * Active peering connections → You can have up to 50 per VPC (can stretch to 125, but too many may slow things down).
       
       * Outstanding requests → You can only have 25 pending requests waiting for approval at a time.
       
       * Expiry → A peering request dies after 1 week if not accepted.
