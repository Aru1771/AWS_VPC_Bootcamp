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
