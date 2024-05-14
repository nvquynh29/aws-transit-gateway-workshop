---
title : "Chuẩn bị"
date : "`r Sys.Date()`"
weight : 1
chapter : false
pre : " <b> 6.1 </b> "
---

Trong bước này, chúng ta sẽ triển khai một VPC và một EC2 instance chạy trong đó trong region Singapore của tài khoản thứ hai.

Sử dụng tài khoản thứ hai chuyển region sang Singapore rồi thực hiện các bước sau đây.

#### Customer VPC
- Truy cập dịch vụ **CloudFormation**
- Dùng file **VPC.yaml** để triển khai VPC giống các bước trước với các tham số như sau:
  - Stack name: `CustomerVPC`
  - EC2InstanceAMIId: AMI ID của **Amazon Linux 2023**
  - SubnetCidr: `10.6.0.0/24`
  - VPCCidr: `10.6.0.0/16`
  - VPCPrefix: `customer`
![Customer VPC](/images/6-cross-account-cross-region/preparation_1.png)

#### Customer Transit Gateway
Truy cập dịch vụ **VPC**, rồi tạo transit gateway mới với cấu hình như sau:
  - Name tag: `customer-tgw`
  - Bỏ chọn **Default route table association** và **Default route table propagation**
![Customer transit gateway](/images/6-cross-account-cross-region/preparation_2.png)

Tạo transit gateway attachment `customer-att` gắn với **customer-vpc** VPC.
![Customer transit gateway](/images/6-cross-account-cross-region/preparation_3.png)
![Customer transit gateway](/images/6-cross-account-cross-region/preparation_4.png)

Tạo transit gateway route table `customer-tgw-rtb` rồi tạo associate với **customer-att** attachment.
![Customer transit gateway](/images/6-cross-account-cross-region/preparation_5.png)
![Customer transit gateway](/images/6-cross-account-cross-region/preparation_6.png)
![Customer transit gateway](/images/6-cross-account-cross-region/preparation_7.png)