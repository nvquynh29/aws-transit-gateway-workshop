---
title : "Multiple route tables"
date : "`r Sys.Date()`"
weight : 4
chapter : false
pre : " <b> 3.4 </b> "
---

From this section forward, we will transition the AWS Transit Gateway configuration from utilizing a single route table 
to employing multiple route tables.

As mentioned previously, each newly created attachment is automatically associated with the default route table. 
While this simplifies initial configuration, it can lead to complex, cumbersome, and challenging route table management 
as the number of attachments and routes grows. Additionally, due to the association and propagation of all attachments, 
any attachment connected to the Transit Gateway can route to all other attachments. In many scenarios, this is not desirable, 
and you may instead want finer control over traffic direction for each network component. 
To achieve this granular control, multiple route tables can be employed.

#### Configure routing
1\. Delete the association between transit gateway attachments and the default route table
- Within VPC interface, select **Transit gateway route tables**
- Select the unique transit gateway route table and change the name to `default-tgw-rtb`
- Select **Associations** tab
- Select any association and then press **Delete association**
![Multiple route tables](/images/3-single-account-single-region/multiple_route_table_1.png)
- Continue deleting the remaining associations until they are gone
![Multiple route tables](/images/3-single-account-single-region/multiple_route_table_2.png)

2\. Check connection from **dev** instance to **share** instance

Return to the **EC2 instance connect** interface of the **dev** instance and try to connect to the **share** instance, 
you will see that you cannot connect to it anymore. The reason is because we have removed the link between 
transit gateway attachments and the default route table without configuring new route tables.
![Multiple route tables](/images/3-single-account-single-region/multiple_route_table_3.png)

3\. Create new transit route table

- Within transit gateway route tables interface, click **Create transit gateway route table**
![Multiple route tables](/images/3-single-account-single-region/multiple_route_table_4.png)
- Set new route table name to `dev-tgw-rtb`
- Select transit gateway for route table then click **Create transit gateway route table**
![Multiple route tables](/images/3-single-account-single-region/multiple_route_table_5.png)

  Repeat the above steps to create two new route tables, `test-tgw-rtb` and `share-tgw-rtb` as follows:
  ![Multiple route tables](/images/3-single-account-single-region/multiple_route_table_6.png)
  ![Multiple route tables](/images/3-single-account-single-region/multiple_route_table_7.png)

4\. Create new association for transit gateway attachments
- Within transit gateway route tables interface, select **dev-tgw-rtb**
- Select **Associations** tab
- Click **Create association**
![Multiple route tables](/images/3-single-account-single-region/multiple_route_table_8.png)
Select **dev-att** then click **Create association**
![Multiple route tables](/images/3-single-account-single-region/multiple_route_table_9.png)

  Repeat the above steps to create the association of **test-tgw-rtb** and **share-tgw-rtb** as follows:
  ![Multiple route tables](/images/3-single-account-single-region/multiple_route_table_10.png)
  ![Multiple route tables](/images/3-single-account-single-region/multiple_route_table_11.png)

5\. Create propagations
- Within transit gateway route tables interface, select **dev-tgw-rtb**
- Select **Propagations** tab then click **Create propagation**
![Multiple route tables](/images/3-single-account-single-region/multiple_route_table_12.png)
Select **share-att** then click **Create propagation**
![Multiple route tables](/images/3-single-account-single-region/multiple_route_table_13.png)
After creating propagation, a route will be automatically created as follows:
![Multiple route tables](/images/3-single-account-single-region/multiple_route_table_14.png)

  Do the same to create propagation from **share-tgw-rtb** to **dev-att**
  ![Multiple route tables](/images/3-single-account-single-region/multiple_route_table_15.png)
  ![Multiple route tables](/images/3-single-account-single-region/multiple_route_table_16.png)

6\. Check connection from **dev** instance to **share** instance
![Multiple route tables](/images/3-single-account-single-region/multiple_route_table_17.png)

#### Practice yourself
Repeat the above steps to configure the connection from **test** instance to **share** instance and then check your results.
![Multiple route tables](/images/3-single-account-single-region/multiple_route_table_18.png)
