---
title : "Preparation"
date : "`r Sys.Date()`"
weight : 1
chapter : false
pre : " <b> 3.1 </b> "
---

In this step, we will deploy essential resources, including VPCs and EC2 instances, to lay the foundation for subsequent 
network configuration phases. The upcoming network configuration involves intricate details, particularly in terms of route tables. 
Since the primary objective of this workshop is to grasp the workings of AWS Transit Gateway, we will create only one 
EC2 instance within each VPC. This will serve for connectivity testing with other EC2 instances in different VPCs 
once network configuration is complete.

#### Deploy CloudFormation Stack
We will deploy three VPCs as follows by CloudFormation service.
![Initial VPCs](/images/3-single-account-single-region/initial_vpcs.svg)

1\. Within the AWS Management Console interface:
- Search keyword: `cloudformation`
- Select **CloudFormation** service
![Deploy CloudFormation Stack](/images/2-preparation/preparation_1.png)

2\. Within CloudFormation interface:
- Click **Create stack**
![Deploy CloudFormation Stack](/images/2-preparation/preparation_2.png)
- Select **Upload a template file**
- Click **Choose file**
![Deploy CloudFormation Stack](/images/2-preparation/preparation_3.png)
- Select file **TransitGatewayWorkshop.yaml** to upload
- Click **Next**
![Deploy CloudFormation Stack](/images/2-preparation/preparation_4.png)

3\. You can give any name for stack (e.g. `TransitGatewayWorkshop`). Follow these steps to get the EC2 AMI:
- Search keyword `ec2` then open **EC2** service in the new window
![Deploy CloudFormation Stack](/images/2-preparation/preparation_5.png)
- Create a new EC2 instance and scroll down to the Amazon Machine Image (AMI) configuration section.
- Copy **AMI ID**
![Deploy CloudFormation Stack](/images/2-preparation/preparation_6.png)
- Go back to the CloudFormation window and replace the AMI ID you just copied then click **Next**
![Deploy CloudFormation Stack](/images/2-preparation/preparation_7.png)
- Scroll down to the bottom of the page and click **Next**
- Continue scrolling to the bottom of the page and click **Submit**

Wait a few minutes for your CloudFormation stack to be ready before moving on to the next step.
![Deploy CloudFormation Stack](/images/2-preparation/preparation_8.png)
