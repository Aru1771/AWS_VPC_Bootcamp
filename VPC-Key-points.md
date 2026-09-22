IP address in real world
---------------------------

* Always make sure give the ip with 10, 192, 172 use these 3 three only in real time. ----> recomened
* we can use other ip's as well like 11,12,13 but thise kind of ip makes some issues when we are config vpn.


Subnets in real world:
------------------------

we used:

    public subnet
    private subnet
    application subnet
    data subnet

 public subnet:

        used to host the webserver, bastenhost, Natgateway's, 


private subnet:

       used to host backed servers, database servers, internal servers.


application subnet:

       it's same as a privatesubnet but as a origanization level some times we can call it as a app subnet.

       used to host EKS clusters, middleware services.

data subnet:


        used to host DATABASES like databases.




Block ip in every subnet:
--------------------------

* if you take vpc cidr : 10.0.0.0./16

* in that vpc if i create any subnet like: 10.0.11.0/24 --> subnet 1
* for the second subnet in the same vpc use --> 10.0.12.0/24.

* Never reduce the value you have given in the subnet. if you reduce it will overwrite.

* Blocked Ip's:

      10.0.0.0 --> for network
      10.0.0.1 --> vpc routing purpose
      10.0.0.2 ---> for DNS
      10.0.0.3 ---> for future reference
      10.255.255.255 --> brod cast 

DHCP service in vpc: 

       by default we will get default dns servers from aws.
       but in real time for active driectires we have to create cutome dns servers.

  Subnet level imp:

       we have to enable auto-assign public ip to assing ip's to ec2 at subnet level.


My Project CIDR ranges:

        10.100.0.0/16 --- dev
        10.120.0.0/16 -- stage
        10.140.0.0/16 -- prod


we can add more than one CIDR block:
-------------------------------------

        if required we can add more then one cidr block but the sequence is same.
        like fiest cidr is 172.31.0.0\16
        the second cider is like 172.34.0.0.\16

If we create a VPC what are the default things we will get:
--------------------------------------------------------------

       1. default route table
       2. default NACL
       3. default security group

one IGW we can attch to one VPC:
--------------------------------
