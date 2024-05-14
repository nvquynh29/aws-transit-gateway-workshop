---
title : "Single Account - Cross Region"
date : "`r Sys.Date()`"
weight : 4
chapter : false
pre : " <b> 4. </b> "
---

In this section, we will configure Transit Gateway to connect VPCs in the same AWS account but in different regions.

**Scenario**: After a period of growth, your company has expanded and opened a second branch in a different country. 
The first product of this branch is a website designed for marketing purposes. This website needs to retrieve data from 
the database to generate statistics and reports on the impressive achievements of your company. Since the new branch is 
still in its early stages, they do not have many engineers. You are tasked with assisting the development team in preparing 
the deployment environment for the product on AWS. To save time and costs, you want to use the Share VPC to share 
the database and cache, similar to the previous section.

The branch has already prepared a VPC named Branch and a Transit Gateway. Your task is to connect the Branch VPC to the Share VPC. 
Similar to the previous section, you could use VPC Peering. However, in this section, you will explore an alternative 
approach using AWS Transit Gateway. The network configuration will look as follows:
<!-- TODO: Sơ đồ 2 Transit Gateway kết nối với nhau thông qua peering, có bảng định tuyến (khó vẽ) -->

#### Content

1. [Preparation](4.1-preparation/)
2. [Configure routing](4.2-configure-route-tables/)