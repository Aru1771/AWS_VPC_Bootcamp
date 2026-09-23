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

DHCP service in vpc: 

       by default we will get default dns servers from aws.
       but in real time for active driectires we have to create cutome dns servers.

  Subnet level imp:

       we have to enable auto-assign public ip to assing ip's to ec2 at subnet level.


My Project CIDR ranges:

        10.100.0.0/16 --- dev
        10.120.0.0/16 -- stage
        10.140.0.0/16 -- prod


we can add more than one CIDR block to our VPC:
-----------------------------------------------

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

IN realworld we always maintain 2 TO 3 public, private, app, db subnets for high availability
-----------------------------------------------------------------------------------------

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

Once we created the Subnet we can't modify the CIDR range
-----------------------------------------------------------
      

VPC Is region scoped if you create a vpc in specific region the vpc only available in that region only
--------------------------------------------------------------------------------------------------------

VPC CIDR range always in b/w the \16 to \18
---------------------------------------------

       if you take less than 16 we will get more than 65,536 Ip address.
       if you take more than 28 we will get very less Ip address

If you create any subnet in vpc it will attached to default route table which we get at the time of VPC creation and By default it is attched to default NACL as well.
-

for public subnets we have to enable auto-assign Ip address Option 
------------

once we attach the subnets to custome route tables those subnets will automatically deattach from the default route table
-


