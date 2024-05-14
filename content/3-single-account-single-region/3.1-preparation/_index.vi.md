---
title : "Chuẩn bị"
date : "`r Sys.Date()`"
weight : 1
chapter : false
pre : " <b> 3.1 </b> "
---

Trong bước này, chúng ta sẽ cùng triển khai các tài nguyên như VPC và máy ảo EC2 để chuẩn bị cho các bước cấu hình mạng tiếp theo. Trong các bước tiếp theo, cấu hình mạng sẽ tương đối phức tạp, đặt biệt là route table. Vì mục tiêu chính của workshop này là hiểu được cách hoạt động của AWS Transit Gateway nên trong mỗi VPC, chúng ta sẽ chỉ tạo 1 EC2 instance để kiểm tra kết nối đến EC2 instance trong các VPC khác sau khi đã cấu hình mạng.

#### Triển khai CloudFormation Stack
Chúng ta sẽ triển khai ba VPC như sau bằng dịch vụ CloudFormation.
![Initial VPCs](/images/3-single-account-single-region/initial_vpcs.svg)

1\. Tại giao diện của AWS Console:
- Tìm kiếm từ khoá: `cloudformation`
- Chọn **CloudFormation**
![Deploy CloudFormation Stack](/images/2-preparation/preparation_1.png)

2\. Tại giao diện của CloudFormation:
- Bấm **Create stack**
![Deploy CloudFormation Stack](/images/2-preparation/preparation_2.png)
- Chọn **Upload a template file**
- Bấm **Choose file**
![Deploy CloudFormation Stack](/images/2-preparation/preparation_3.png)
- Chọn file **TransitGatewayWorkshop.yaml** để tải lên
- Bấm **Next**
![Deploy CloudFormation Stack](/images/2-preparation/preparation_4.png)

3\. Bạn có thể đặt tên bất kỳ cho stack (VD: `TransitGatewayWorkshop`). Làm theo các bước sau đây để lấy EC2 AMI:
- Tìm kiếm từ khoá `ec2` rồi mở dịch vụ **EC2** trong cửa số mới
![Deploy CloudFormation Stack](/images/2-preparation/preparation_5.png)
- Tạo EC2 instance mới rồi kéo xuống phần cấu hình Amazon Machine Image (AMI)
- Sao chép **AMI ID**
![Deploy CloudFormation Stack](/images/2-preparation/preparation_6.png)
- Quay lại cửa sổ CloudFormation rồi thay thế AMI ID bạn vừa copy vào sau đó bấm **Next**
![Deploy CloudFormation Stack](/images/2-preparation/preparation_7.png)
- Kéo xuống cuối trang rồi bấm **Next**
- Tiếp tục kéo xuống cuối trang rồi bấm **Submit**

Hãy đợi vài phút để CloudFormation tạo thành công trước khi chuyển sang bước tiếp theo.
![Deploy CloudFormation Stack](/images/2-preparation/preparation_8.png)