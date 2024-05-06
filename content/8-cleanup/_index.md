---
title : "Clean Up"
date : "`r Sys.Date()`"
weight : 8
chapter : false
pre : " <b> 8. </b> "
---
After completing this workshop, you need to delete all created resources or you will be charged. Please delete carefully and check again after deleting because Transit Gateway is a expensive resource. If you forget to delete Transit Gateway, it can cost a lot of money.

Please delete the resources in the order below in both **Tokyo** and **Singapore** regions.

If you did not do part 5 and 6:
- Delete all transit gateway attachments
- Delete transit gateway
- Delete CloudFormation Stack
- Delete S3 buckets with names similar to the format **cf-templates-*** (S3 buckets automatically created by AWS to store your CloudFormation template files)

If you do part 5 and 6:
- Account A:
  - Delete shared resources in the AWS RAM service
- Account B:
  - Delete all transit gateway attachments
  - Delete transit gateway
  - Delete CloudFormation Stack
  - Delete S3 buckets with names similar to the format **cf-templates-***