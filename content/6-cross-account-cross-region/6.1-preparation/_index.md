---
title : "Preparation"
date : "`r Sys.Date()`"
weight : 1
chapter : false
pre : " <b> 6.1 </b> "
---

In this step, we will deploy a VPC and an EC2 instance in the Singapore region of the second account.

Login to the second account, switch to Singapore region and then perform the following steps.

#### Deploy VPC
- Access **CloudFormation** service
- Use the **VPC.yaml** file to deploy VPC like the previous steps with the following parameters:
  - Stack name: `CustomerVPC`
  - EC2InstanceAMIId: AMI ID of **Amazon Linux 2023**
  - SubnetCidr: `10.6.0.0/24`
  - VPCCidr: `10.6.0.0/16`
  - VPCPrefix: `customer`
![Customer VPC](/images/6-cross-account-cross-region/preparation_1.png)

#### Create Transit Gateway
Access **VPC** service, then create new transit gateway with the following configuration:
  - Name tag: `customer-tgw`
  - Uncheck **Default route table association** and **Default route table propagation**
![Customer transit gateway](/images/6-cross-account-cross-region/preparation_2.png)

Create transit gateway attachment `customer-att` then attach to **customer-vpc** VPC.
![Customer transit gateway](/images/6-cross-account-cross-region/preparation_3.png)
![Customer transit gateway](/images/6-cross-account-cross-region/preparation_4.png)

Create transit gateway route table `customer-tgw-rtb` then create association with **customer-att** attachment.
![Customer transit gateway](/images/6-cross-account-cross-region/preparation_5.png)
![Customer transit gateway](/images/6-cross-account-cross-region/preparation_6.png)
![Customer transit gateway](/images/6-cross-account-cross-region/preparation_7.png)