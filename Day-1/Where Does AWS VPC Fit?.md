Where Does AWS VPC Fit?
=========================
Imagine AWS owns a massive data center.

It contains millions of servers.

If every customer shared the same network, it would be insecure.

AWS solves this by creating a Virtual Private Cloud (VPC).

Think of it as your own isolated virtual network inside AWS.

AWS Cloud

+----------------------------------+
| Customer A - VPC                 |
| 10.0.0.0/16                      |
+----------------------------------+

+----------------------------------+
| Customer B - VPC                 |
| 172.16.0.0/16                    |
+----------------------------------+

+----------------------------------+
| Customer C - VPC                 |
| 192.168.0.0/16                   |
+----------------------------------+

Each customer has their own isolated network.
