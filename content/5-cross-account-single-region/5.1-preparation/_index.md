---
title : "Preparation"
date : "`r Sys.Date()`"
weight : 1
chapter : false
pre : " <b> 5.1 </b> "
---

To proceed with the steps in this section, you will need an additional AWS account. However, if you do not have a 
second AWS account, you can still follow the steps below to gain an understanding of how to connect two VPCs from 
different AWS accounts within the same region.

In this step we will deploy a VPC and an EC2 instance running in this VPC in the Tokyo region of the second AWS account.

1. Login to the second AWS account using an incognito window or another browser, and make sure you keep it turned on so
you can easily using the two accounts simultaneously.
2. Switch to region **ap-northeast-1 (Tokyo)**
3. Deploy a new VPC using CloudFormation

Use the **VPC.yaml** file to deploy a VPC with the following parameters:
- Stack name: `PartnerVPC`
- EC2InstanceAMIId: AMI ID of **Amazon Linux 2023**
- SubnetCidr: `10.5.0.0/24`
- VPCCidr: `10.5.0.0/16`
- VPCPrefix: `partner`
![Deploy CloudFormation stack](/images/5-cross-account-single-region/preparation_1.png)
![Deploy CloudFormation stack](/images/5-cross-account-single-region/preparation_2.png)