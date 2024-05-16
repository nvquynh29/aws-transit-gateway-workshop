---
title : "Create transit gateway and attachments"
date : "`r Sys.Date()`"
weight : 2
chapter : false
pre : " <b> 3.2 </b> "
---

In this step, we will create AWS Transit Gateway and attachments then connect to the VPCs created in the previous step.

1\. Within AWS Management Console interface, search `vpc` then choose **VPC** service
![Create attachments](/images/3-single-account-single-region/create_attachments_1.png)

2\. Create AWS Transit Gateway
- Within VPC interface, navigate to **Transit gateways**
- Click **Create transit gateway**
![Create attachments](/images/3-single-account-single-region/create_attachments_2.png)
- Fill in the following information then click **Create transit gateway**
  - Name tag: `tokyo-tgw`
  - Description: `Transit Gateway for region Tokyo`
  ![Create attachments](/images/3-single-account-single-region/create_attachments_3.png)

3\. Create attachments
After your Transit Gateway become available state, you can create attachments.

- Within VPC interface, navigate to **Transit gateway attachments**
- Click **Create transit gateway attachment**
![Create attachments](/images/3-single-account-single-region/create_attachments_4.png)
- Enter the following information:
  - Name tag: `dev-att`
  - Transit gateay ID: **tokyo-tgw**
  - Attachment type: **VPC**
  - VPC ID: **dev-vpc**
  ![Create attachments](/images/3-single-account-single-region/create_attachments_5.png)
- Click **Create transit gateway attachment**

Repeat the above step to create 2 more transit gateway attachments with the following configuration:
- `test-att` attachment attach to **test-vpc** VPC
![Create attachments](/images/3-single-account-single-region/create_attachments_6.png)
- `share-att` attachment attach to **share-vpc** VPC
![Create attachments](/images/3-single-account-single-region/create_attachments_7.png)

After creating, we will have the following attachments.
![Create attachments](/images/3-single-account-single-region/create_attachments_8.png)