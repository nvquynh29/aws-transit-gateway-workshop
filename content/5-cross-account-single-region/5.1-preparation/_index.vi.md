---
title : "Chuẩn bị"
date : "`r Sys.Date()`"
weight : 1
chapter : false
pre : " <b> 5.1 </b> "
---

<!-- TODO: Update text -->
Bạn cần có thêm một tài khoản AWS để thực hiện các bước trong phần này. Tuy nhiên nếu như bạn chưa có tài khoản AWS thứ 
hai thì cũng có thể theo dõi các bước dưới đây để hiểu cách hoạt động và cấu hình kết nối 2 VPC thuộc hai tài khoản AWS khác nhau nhưng thuộc cùng một region.

1. Đăng nhập vào tài khoản AWS thứ hai bằng cửa sổ ẩn danh hoặc trình duyệt khác và hãy đảm bảo rằng bạn giữ cửa sổ này bật để có thể sử dụng đồng thời 2 tài khoản một cách dễ dàng.
2. Chọn region **ap-northeast-1 (Tokyo)**
3. Triển khai VPC mới bằng CloudFormation

Sử dụng file **VPC.yaml** để triển khai với các tham số như sau:
- Stack name: `PartnerVPC`
- EC2InstanceAMIId: AMI ID của **Amazon Linux 2023**
- SubnetCidr: `10.5.0.0/24`
- VPCCidr: `10.5.0.0/16`
- VPCPrefix: `partner`
![Deploy CloudFormation stack](/images/5-cross-account-single-region/preparation_1.png)
![Deploy CloudFormation stack](/images/5-cross-account-single-region/preparation_2.png)