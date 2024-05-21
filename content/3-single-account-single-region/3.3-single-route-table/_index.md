---
title : "Single route table"
date : "`r Sys.Date()`"
weight : 3
chapter : false
pre : " <b> 3.3 </b> "
---

In this step, we will configure routing for Dev VPC, Test VPC to connect to the Share VPC using only the default route table.

#### Check the connection between EC2 instances
1\. Connect to **dev** instance
- Access **EC2** service then navigate to **Instances** page, then select **dev** instance
- Click **Connect**
![Single route table](/images/3-single-account-single-region/single_route_table_1.png)
- Keep the default configurations then click **Connect**
![Single route table](/images/3-single-account-single-region/single_route_table_2.png)

2\. Check connection from **dev** to **share** instance
- Return to the list of instances page and select **share** instance
- Copy private IPv4 address of this instance
![Single route table](/images/3-single-account-single-region/single_route_table_3.png)
- Return to the **EC2 instance connect** interface of the **dev** instance and run the following command:
  ```shell
  ping <share_instance_private_ipv4> -c5
  ```
- After few minutes, we receive a message saying we cannot connect to the **share** instance. The reason is because 
these 2 virtual machines are located in 2 VPCs that are not connected to each other.
![Single route table](/images/3-single-account-single-region/single_route_table_4.png)

#### Configure routing for Dev VPC
1\. Configure route table for Dev VPC
- Return to VPC interface, select **Route tables**
- Select **dev-rtb**
- Select **Routes** tab then click **Edit routes**
![Single route table](/images/3-single-account-single-region/single_route_table_5.png)

  Add a new route with destination `10.3.0.0/16` (CIDR of Share VPC), select **Transit Gateway** for target then select **dev-att** and click **Save changes**
  ![Single route table](/images/3-single-account-single-region/single_route_table_6.png)

2\. Configure the routing table for Share VPC by repeating the above step
  ![Single route table](/images/3-single-account-single-region/single_route_table_7.png)
  ![Single route table](/images/3-single-account-single-region/single_route_table_8.png)

3\. Check the connection again
- Return to the **EC2 instance connect** interface and rerun the following command:
  ```shell
  ping <private_ipv4_share_instance> -c5
  ```

The results show that it is possible to connect from the **dev** instance to the **share** instance, proving that the 
Dev VPC and Share VPC can connect to each other through transit gateway.
![Single route table](/images/3-single-account-single-region/single_route_table_9.png)

#### Configure routing for Test VPC
Repeat the above steps to configure the connection between Test VPC and Share VPC and test your configuration.
![Single route table](/images/3-single-account-single-region/single_route_table_10.png)
