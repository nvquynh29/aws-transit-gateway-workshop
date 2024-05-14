---
title : "Cross Account - Cross Region"
date : "`r Sys.Date()`"
weight : 6
chapter : false
pre : " <b> 6. </b> "
---
In this section, we will configure AWS Transit Gateway to connect VPCs located in different accounts and regions.

**Scenario**: After achieving success in self-developing products, your company has attracted numerous large customers. 
One of these customers wishes to collaborate with your company to develop a system for their organization. The customer 
company has an existing VPC named Customer VPC and a Transit Gateway that can facilitate a connection between your company's network. 
During the system development process, the development team requires access to customer data. However, due to security concerns, 
the customer company only agrees to share this data via a private network connection. Additionally, they want to use the 
system before its official release.

In this case, you should not connect Dev VPC directly to Customer VPC because the Dev environment has not been carefully tested, 
so errors may occur when customer use it. Instead, you should configure to connect Test VPC to Customer VPC because the 
Test environment has been carefully tested and the Dev environment is using the same data source with this environment 
so it can meet the requirements of both customer company and development team. The configuration is very similar to the 
previous sections. Your network configuration will look like this:
<!-- TODO: Sơ đồ 2 Transit Gateway kết nối với nhau thông qua peering, có bảng định tuyến (khó vẽ) -->

#### Content

1. [Preparation](4.1-preparation/)
2. [Configure routing](4.2-configure-route-tables/)