---
title : "Chuẩn bị"
date : "`r Sys.Date()`"
weight : 1
chapter : false
pre : " <b> 4.1 </b> "
---
Trong bước này chúng ta sẽ deploy một VPC và một máy ảo EC2 trong region `ap-southeast-1 (Singapore)`

#### Triển khai CloudFormation Stack
Phần triển khai này rất giống với phần 2, chỉ có một vài điểm cần lưu ý sau đây:
- Sử dụng file **VPC.yaml**

Điền các tham số như sau:
- EC2InstanceAMIId: Chọn AMI ID của **Amazon Linux 2023**
- SubnetCidr: `10.4.0.0/24`
- VPCCidr: `10.4.0.0/16`
- VPCPrefix: `branch`
