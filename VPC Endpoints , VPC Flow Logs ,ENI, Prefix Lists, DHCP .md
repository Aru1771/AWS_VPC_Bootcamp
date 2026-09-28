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
* 
