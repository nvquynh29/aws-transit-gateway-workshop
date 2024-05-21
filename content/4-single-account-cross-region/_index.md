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
![Diagram](/images/4-single-account-cross-region/single_account_cross_region.svg)

At first glance, the network diagram might seem intricate, but let's break it down using the concepts of Route Table Association and Route Propagation introduced earlier.

Imagine traffic originating from the Share VPC embarking on a journey through the network. Its first stop is the Tokyo TGW, where it's greeted by the peering attachment connecting it to the Singapore TGW. This attachment acts as a bridge, allowing the traffic to seamlessly traverse to the Singapore TGW. Upon arrival at the Singapore TGW, the traffic encounters Branch VPC route table that directs it towards the Branch VPC. This route table, acting as a traffic guide, ensures that the data reaches its intended destination.


#### Content

1. [Preparation](4.1-preparation/)
2. [Configure routing](4.2-configure-route-tables/)