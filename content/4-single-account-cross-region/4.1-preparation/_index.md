---
title : "Preparation"
date : "`r Sys.Date()`"
weight : 1
chapter : false
pre : " <b> 4.1 </b> "
---

In this step we will deploy resources such as VPC, EC2 instance, AWS Transit Gateway and route table within 
**ap-southeast-1 (Singapore)** region.

#### CloudFormation Stack
This step is very similar to the preparation step of previous section, with just few things to note:
- Use **VPC.yaml** file
![Deploy CloudFormation Stack](/images/4-single-account-cross-region/preparation_1.png)

Fill in the parameters as follows:
- Stack name: `BranchVPC`
- EC2InstanceAMIId: AMI ID of **Amazon Linux 2023**
- SubnetCidr: `10.4.0.0/24`
- VPCCidr: `10.4.0.0/16`
- VPCPrefix: `branch`
![Deploy CloudFormation Stack](/images/4-single-account-cross-region/preparation_2.png)

#### Transit Gateway
1\. Access to VPC service, navigate to **Transit gateways** then click **Create transit gateway**
![Deploy CloudFormation Stack](/images/4-single-account-cross-region/preparation_3.png)

Configure the transit gateway as follows:
- Name tag: `singapore-tgw`
- Description: `Transit Gateway for region Singapore`
- Uncheck **Default route table association** and **Default route table propagation**

{{% notice note %}}
Uncheck default route table propagation to prevent new attachments from automatically creating association and propagation 
with the default route table.
{{% /notice %}}

![Deploy CloudFormation Stack](/images/4-single-account-cross-region/preparation_4.png)

2\. Create a transit gateway attachment with the following configuration
- Name tag: `branch-att`
- Transit gateway ID: Select **singapore-tgw**
- Attachment Type: **VPC**
- VPC ID: Select **branch-vpc**
![Configure route tables](/images/4-single-account-cross-region/configure_route_tables_4.png)

3\. Create transit gateway route table and association
- Within the **Transit gateway route tables** interface of the Singapore region, click **Create transit gateway route table**
![Configure route tables](/images/4-single-account-cross-region/configure_route_tables_5.png)
- Set the configuration as follows:
  - Name tag: `branch-tgw-rtb`
  - Transit gateway ID: **singapore-tgw**

  ![Configure route tables](/images/4-single-account-cross-region/configure_route_tables_6.png)

After creating **branch-tgw-rtb**, select **Associations** tab and click **Create association** then associate
with **branch-att** attachment.
![Configure route tables](/images/4-single-account-cross-region/configure_route_tables_7.png)
![Configure route tables](/images/4-single-account-cross-region/configure_route_tables_8.png)

{{% notice note %}}
Please open two tabs to select the Tokyo and Singapore regions in your browser to save time during configuration.
{{% /notice %}}
