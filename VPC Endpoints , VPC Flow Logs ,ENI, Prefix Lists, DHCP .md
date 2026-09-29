VPC Endpoints , VPC Flow Logs ,ENI, Prefix Lists, DHCP 
========================================================


VPC Endpoints 
--------------

An AWS VPC Endpoints allow you to privately connect your VPC to supported AWS services without requiring an internet gateway, NAT devices, VPN connection, or AWS Direct Connect.

There are two types of VPC endpoints:

1. Interface Endpoints: These are powered by AWS PrivateLink. They allow private connectivity to services like Amazon S3, DynamoDB, or custom services hosted by other AWS accounts,
   without using public IP addresses. by using this we can connect to other AWS services with in the other region and other AWS accounts.

2. Gateway Endpoints: These are used to connect directly to services like Amazon S3 and DynamoDB within your VPC, without needing an internet gateway or NAT device.
   By using this we can connect the Amazon S3 and DynamoDB within your VPC.

* If route table have both endpoint and NAT gateway. based up on the access it will choose it will select the end point / nat-gateway.

VPC Flow Logs and How to Create Them
-------------------------------------


* VPC Flow Logs capture information about the IP traffic going to and from network interfaces in your VPC.
* Flow logs can help you with monitoring and troubleshooting network connectivity.

Key Features of VPC Flow Logs:

      Capture Network Traffic: Logs all traffic going in and out of your VPC.
      Integration: Can be sent to CloudWatch Logs, S3, or a partner service.
      Filtering: You can filter logs based on traffic type.

* In Production we will store VPC flow Logs in Cloudwatch
* in lower envronment related VPC floe logs we will store in S3.

* we can enble VPC flow logs both subnet and VPC level. recommended way is always implement it in VPC level.
* if we are storing vpc logs at s3 we have to choose aws athina service or splunk to see those logs. because these logs will store in log.gz format.
* we can also downlod the lof file and unzip in local pc and we can able to see the logs.
* if any issue happed at network level we can see in these vpc flow logs.

* To learn more About it check this page: https://docs.aws.amazon.com/vpc/latest/userguide/flow-log-records.html


Elastic Network Interface (ENI) in AWS
---------------------------------------

* Elastic Network Interfaces (ENIs) are virtual network interfaces that can be attached to EC2 instances.
* They are used to manage network traffic, IP addresses, and security group rules.

Key Features of ENIs:
-
* Flexible Attachment: Can be attached or detached from instances without stopping them.
* Multiple IP Addresses: Can have one or more private IP addresses.
* Security Groups: Can be associated with multiple security groups.

* if i create a EC2 by default one private IP was assigned to the instance. that private IP will create one Network interface by default.
* if you go to Network interface tab and serch with instance Private IP there you can find one Network interface for the Serched Private IP.

Use case of ENI:

       for eg if i installed a DB server in the ec2. at the time of failover we can use this ENI we can create a multiple private IP addrees to EC2.

How to Create a Netwoek interface:

       Now we can see How to create NI for EC2.
       1. Go to ---> EC2 ---> search for network interface option in left side controlpanel.
       2. click on create Network interface.
       * Description:
       * subnet: select the subnet where our ec2 was created.
       * Private IP address: Auto_assign/custome --> if you select custome we can give our VPC cidr range also.
       * Security Group: select atleast one.
       3. Click on create NI.

How to attach NI to ec2.

     How we can attch NI to ec2.
     1. in the NI tab select the NI and go settings---> click on Attach ---> there we have to selet the VPC.
     2. then we will see the instances from the subnets what we have selected at the time of creating the NI.
     3. click on Attach

     Now if you see the instance have two Private IP address.


Prefix Lists in AWS VPC
-------------------------


* Prefix Lists in AWS VPC are collections of CIDR blocks that you can use to simplify the management of large sets of IP addresses in security groups, route         tables, and other resources that require CIDR blocks.
  
* By using prefix lists, you can manage and reference multiple CIDR blocks as a single entity, reducing complexity and the potential for errors.

Key Benefits of Prefix Lists
-
* Simplification: Manage multiple CIDR blocks as a single entity, making it easier to maintain and update.
* Consistency: Ensure consistent CIDR block usage across multiple resources.
* Scalability: Easily update the list of CIDR blocks without having to update each individual resource.


Creating and Using Prefix Lists:

         Step 1: Create a Prefix List
         Navigate to the VPC Dashboard in the AWS Management Console.
         
         Select "Prefix Lists" from the left-hand menu.
         
         Click "Create prefix list".
         
         Provide the following details:
         
         Name: A descriptive name for the prefix list.
         Description: An optional description for the prefix list.
         Max entries: The maximum number of CIDR blocks that the prefix list can contain.
         CIDR blocks: Add the CIDR blocks that you want to include in the prefix list.
         Click "Create prefix list".

Example

         Name: MyPrefixList
         Description: List of allowed IP ranges
         Max entries: 10
         CIDR blocks:
           - 192.168.1.0/24
           - 10.0.0.0/16
           - 172.16.0.0/12

Step 2: Update Route Tables to Use the Prefix List

            Navigate to Route Tables in the VPC Dashboard.
            
            Select the route table you want to update.
            
            Click on the "Routes" tab, then click "Edit routes".
            
            Add a new route or update an existing route:
            
            Destination: Select "Prefix list" and choose your created prefix list from the dropdown.
            Target: Specify the target for the traffic (e.g., an internet gateway, NAT gateway, or VPC peering connection).
            Click "Save routes".


DHCP Option Set in AWS VPC
---------------------------
* DHCP Option Set is a VPC-level configuration that tells EC2 instances which DNS, domain, time, and related network settings to use.

For a DevOps engineer, initially remember:

      DHCP Option Set
             |
             +---- DNS Server
             |
             +---- Domain Name
             |
             +---- NTP Server
             |
             +---- NetBIOS settings

* domain-name-servers

      Question: "Which DNS server should I use?"
      
      Example:
      
      EC2 → DNS server → Find google.com IP
      
      In AWS, normally:
      
      AmazonProvidedDNS
      
      🧠 Remember:
      
      DNS = Find the IP

      Example:
      
      EC2
       ↓
      DHCP Option Set
       ↓
      DNS Server = AmazonProvidedDNS
      
      When you run:
      
      nslookup google.com
      
      the EC2 instance sends the DNS request to its configured DNS resolver.
      
      Remember:
      
      domain-name-servers = WHERE do I ask for DNS?

* domain-name → Domain

      Question: "What domain should I belong to/use?"
      
      Example:
      
      example.com
      
      It helps build hostnames.
      
      🧠 Remember:
      
      Domain = My network's name

      For example:
      
      domain-name = example.com
      
      An EC2 hostname can then be associated with that domain.
      
      Remember:
      
      domain-name = WHAT is my domain?


* ntp-servers → Time

      Question: "Where can I get the correct time?"
      
      NTP = Network Time Protocol.
      
      Example:
      
      EC2 → NTP Server → Correct time
      
      Why important?
      
      Things like logs, certificates, authentication, and distributed systems often depend on correct time.
      
      🧠 Remember:
      
      NTP = Correct time

* netbios-name-servers → NetBIOS Server

      This is mainly related to older Windows networking.
      
      It tells the machine:
      
      "Which server should I ask for NetBIOS names?"
      
      **You don't need to focus heavily on this for normal AWS DevOps work.**
      
      🧠 Remember:
      
      NetBIOS server = Windows name lookup

* netbios-node-type → Communication method

      This tells the machine:
      
      "How should I find other NetBIOS machines?"
      
      There are different types:
      
      1 = B-node
      2 = P-node
      4 = M-node
      8 = H-node
      
      For now, don't memorize these numbers.
      
      Just remember:
      
      NetBIOS node type = How NetBIOS communicates

* DNS finds names, Domain identifies, NTP gives time, NetBIOS finds Windows machines, Node Type tells how

* For your AWS DevOps learning, focus strongly on domain-name-servers and domain-name first.

* domain-name-servers specifies the DNS servers that instances should use, while domain-name specifies the domain name used for DNS hostnames.

DNS Hostnames and DNS Resolution in AWS VPC
---------------------------------------------

* DNS Hostnames: Enable DNS hostnames to assign a DNS name to EC2 instances.
* DNS Resolution: Allow or disable DNS resolution within your VPC. If enabled, instances can use AWS provided DNS servers.

