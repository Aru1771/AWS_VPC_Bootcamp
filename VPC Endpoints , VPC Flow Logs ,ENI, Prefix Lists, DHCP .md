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

