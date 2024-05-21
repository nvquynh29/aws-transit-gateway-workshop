---
title : "Clean Up"
date : "`r Sys.Date()`"
weight : 7
chapter : false
pre : " <b> 7. </b> "
---
After completing this workshop, you need to delete all created resources or you will be charged. Please delete carefully and check again after deleting because Transit Gateway is a expensive resource. If you forget to delete Transit Gateway, it can cost a lot of money.

Please delete the resources in the order below in both **Tokyo** and **Singapore** regions.

If you did not practice part 5 and 6:
- Delete all transit gateway attachments
![Clean Up](/images/7-cleanup/cleanup_1.png)
![Clean Up](/images/7-cleanup/cleanup_2.png)
- Delete transit gateway
![Clean Up](/images/7-cleanup/cleanup_3.png)
- Delete CloudFormation Stack
![Clean Up](/images/7-cleanup/cleanup_4.png)
- Delete S3 buckets with names similar to the format **cf-templates-*** (S3 buckets automatically created by AWS to store your CloudFormation template files)
![Clean Up](/images/7-cleanup/cleanup_5.png)

If you practiced part 5 and 6:
- First Account:
  - Delete shared resources in the AWS RAM service
  ![Clean Up](/images/7-cleanup/cleanup_6.png)
- Second Account:
  - Delete all transit gateway attachments
  - Delete transit gateway
  - Delete CloudFormation Stack
  - Delete S3 buckets with names similar to the format **cf-templates-***