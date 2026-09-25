NAT-Gateway
============


* NAT Gateway is used by the private subnet ec2 instances to access the internet.

* For that we have to create NAT gate way in public subnet and private route-table we have to update the route.
* At the time of creating the NAT Gateway we have to select Public Subnet and one elastic IP address.

Create the NAT Gateway in the public subnet: Dev-VPC-Nat-Gateway_NAT_Name
--------------------------------------------

Add the Nat Gateway route to the Private RouteTable.
----------------------------------------------------

Route Entry

Destination : 0.0.0.0/0
Target : NAT Gateway ID


To connect the Private subnet server:
---------------------------------------

1. We have to launch one bastenhost in public subnet.
2. ssh to public subnet.
3. create one .pem file in the bastenhost.
4. copy the private server pem file to basten host and provide 400 permisions.
5. use : ssh -i .pem key ec2-user@private_ip


For High availability of NAT Gateway:
-------------------------------------

* insted of creating a NAT Gateway in a single available zone we can create in multiple public subnets.
* insted of creating a single routerable to all private subnets we will create multiple route tables and we will attch ecah private subnet to single route table.
* in that route table's we have to add this nat-gateway routes.


AWS NACL
---------

* NACL - Network access controll list

* these are stateless so we have to enable both indound and outbound for the traffic.

* these NACL'S are applied at subnetlevel.

* we can allow or denay the traffic hear

* least no of rule have high priority.

* we will create seperate NACL'S for public and private subnets.

* if you denay any ip or port at subnet level but if you allowed at sg level it will not work.

* if you create a vpc by default we will get one default NACL all the subnets will attched to that NACL only.

* in NACL rules if you see * means it is the highest number.

* when creating a  NACL select the vpc at the time of creating the NACL.

* onc'e NACL created go to subnet accosiation and slect the private subnets.

* After creating the NACL all the inbound and outbond is denay only.

* inbound rule number will start from 1 to 32,767. in rule's we always give our client vpn range not 0.0.0.0./0.

* 
