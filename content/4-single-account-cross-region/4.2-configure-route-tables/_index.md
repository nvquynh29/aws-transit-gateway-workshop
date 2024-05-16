---
title : "Configure routing"
date : "`r Sys.Date()`"
weight : 2
chapter : false
pre : " <b> 4.2 </b> "
---

In this step, we will configure routing to connect Share VPC to Branch VPC.

1\. Within the **Transit gateways** interface of the Tokyo region, select **tokyo-tgw** and copy the id of this transit gateway
![Configure route tables](/images/4-single-account-cross-region/configure_route_tables_1.png)

2\. Within **Transit gateway attachments** interface of Singapore region, click **Create transit gateway attachment**
![Configure route tables](/images/4-single-account-cross-region/configure_route_tables_2.png)

Fill in the configuration information as follows:
- Name tag: `peering-tokyo`
- Transit gateway ID: Select **singapore-tgw**
- Attachment Type: **Peering connection**
- Account: **My account**
- Region: **Tokyo**
- Transit gateway (accepter): ID of **tokyo-tgw** transit gateway
![Configure route tables](/images/4-single-account-cross-region/configure_route_tables_3.png)

3\. Back to **Transit gateway attachments** interface of Tokyo, you will see an attachment with **Pending Acceptance** status.

Rename this attachment to `peering-singapore` then select **Actions** and click **Accept transit gateway attachment**
![Configure route tables](/images/4-single-account-cross-region/configure_route_tables_9.png)
![Configure route tables](/images/4-single-account-cross-region/configure_route_tables_10.png)

4\. Configure peering between 2 transit gateways

Back to **Transit gateway route tables** interface of Singapore region, select **branch-tgw-rtb** then select **Routes** tab 
and click **Create static route**
![Configure route tables](/images/4-single-account-cross-region/configure_route_tables_11.png)
Enter the CIDR as `10.3.0.0/16` (CIDR of Share VPC) then select **peering-tokyo** attachment.
![Configure route tables](/images/4-single-account-cross-region/configure_route_tables_12.png)
Repeat the above step with **share-tgw-rtb** of the Tokyo region, enter CIDR as `10.4.0.0/16` (CIDR of Branch VPC) and 
select **peering-singapore** attachment.
![Configure route tables](/images/4-single-account-cross-region/configure_route_tables_13.png)
![Configure route tables](/images/4-single-account-cross-region/configure_route_tables_14.png)
After creation, there will be a new static route with type Peering as follows:
![Configure route tables](/images/4-single-account-cross-region/configure_route_tables_15.png)

5\. Configure route table for each transit gateway

Create a new transit gateway route table in the Singapore region named `peering-tokyo-tgw-rtb` and attach it to **singapore-tgw** transit gateway.
![Configure route tables](/images/4-single-account-cross-region/configure_route_tables_16.png)
Create association between **peering-tokyo-tgw-rtb** route table and **peering-tokyo** attachment.
![Configure route tables](/images/4-single-account-cross-region/configure_route_tables_17.png)
![Configure route tables](/images/4-single-account-cross-region/configure_route_tables_18.png)
Create new propagation between **peering-tokyo-tgw-rtb** route table and **branch-att** attachment.
![Configure route tables](/images/4-single-account-cross-region/configure_route_tables_19.png)
![Configure route tables](/images/4-single-account-cross-region/configure_route_tables_20.png)

Repeat the above steps to create a new transit gateway route table in Tokyo region named `peering-singapore-tgw-rtb` and add association with **peering-singapore**, propagating to **share-att** as follows:
![Configure route tables](/images/4-single-account-cross-region/configure_route_tables_21.png)
![Configure route tables](/images/4-single-account-cross-region/configure_route_tables_22.png)
![Configure route tables](/images/4-single-account-cross-region/configure_route_tables_23.png)

{{% notice note %}}
When you create an association with **peering-singapore** attachment, you may get the following error. The reason is because of the present
This attachment is being associated with **default-tgw-rtb**, you need to delete this association first before creating a new association.
{{% /notice %}}
![Configure route tables](/images/4-single-account-cross-region/configure_route_tables_24.png)
Delete the association between **peering-singapore** attachment and **default-tgw-rtb** route table.
![Configure route tables](/images/4-single-account-cross-region/configure_route_tables_25.png)
Re-create association between **peering-singapore** attachment and **peering-singapore-tgw-rtb** route table.
![Configure route tables](/images/4-single-account-cross-region/configure_route_tables_26.png)
Create propagation from **peering-singapore-tgw-rtb** route table to **share-att** attachment.
![Configure route tables](/images/4-single-account-cross-region/configure_route_tables_27.png)
![Configure route tables](/images/4-single-account-cross-region/configure_route_tables_28.png)

6\. Configure routing tables of VPCs

Within **Route tables** interface of Tokyo region, select **share-rtb** route table then select **Routes** tab and click **Edit routes**
![Configure route tables](/images/4-single-account-cross-region/configure_route_tables_29.png)
Add a new route with destination `10.4.0.0/16` and target **share-att** attachment.
![Configure route tables](/images/4-single-account-cross-region/configure_route_tables_30.png)

Repeat the above steps to add a new route to the **branch-rtb** route table in the Singapore region with destination
`10.3.0.0/16` and target is **branch-att**.
![Configure route tables](/images/4-single-account-cross-region/configure_route_tables_31.png)
![Configure route tables](/images/4-single-account-cross-region/configure_route_tables_32.png)

7\. Check the connection between instances

We will try to connect from the Tokyo region's **share** instance to the Singapore region's **branch** instance to check if the Share VPC and Branch VPC are connected to each other.

Within the EC2 interface of the Singapore region, copy the private IPv4 of the **branch** instance.
![Configure route tables](/images/4-single-account-cross-region/configure_route_tables_33.png)
Connect to the **share** instance using EC2 instance connect as in the previous sections and then run the following command:
```shell
ping <branch_instance_private_ipv4> -c5
```
The results show that we have successfully configured.
![Configure route tables](/images/4-single-account-cross-region/configure_route_tables_34.png)

#### Practice yourself
Repeat the above steps to configure the connection between Test VPC and Branch VPC and test your configuration.
![Configure route tables](/images/4-single-account-cross-region/configure_route_tables_35.png)
