---
title : "Single Account - Single Region"
date : "`r Sys.Date()`"
weight : 3
chapter : false
pre : " <b> 3. </b> "
---

In this section, we will configure AWS Transit Gateway to connect VPCs in the same AWS account and the same region.

**Scenario**: You are a DevOps Engineer at startup company X. Your company is developing a system that will be deployed on AWS. 
As a new startup, cost-efficiency is crucial. You are tasked with deploying the system to two separate environments Dev and Test, 
in a cost-effective manner. Since these environments are only for development purposes, you can share resources between the two environments.

To meet these requirements, you decide to deploy three VPCs: Dev, Test, and Share, in the same region. The Share VPC 
will contain resources that can be used in both the Dev and Test environments, such as databases and cache. 
You need to configure routing for Dev VPC and Test VPC to connect to Share VPC.

For scenarios with a small number of VPC connections, VPC Peering is often the preferred choice. However, in this workshop, 
you will explore an alternative approach using AWS Transit Gateway. To simplify the deployment, we will only deploy one 
EC2 instance in each VPC to test network connectivity. We will configure the network as follows:
![Diagram](/images/3-single-account-single-region/single_account_single_region.svg)

#### Content

1. [Preparation](3.1-preparation)
2. [Create transit gateway attachments](3.2-create-attachments/)
3. [Single route table](3.3-single-route-table/)
4. [Multiple route tables](3.4-multiple-route-tables/)