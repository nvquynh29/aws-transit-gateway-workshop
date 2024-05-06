---
title : "Chuẩn bị"
date : "`r Sys.Date()`"
weight : 2
chapter : false
pre : " <b> 2. </b> "
---

#### Chuẩn bị
Hãy tải tệp CloudFormation Template này về máy tính của bạn để chuẩn bị cho các bước tiếp theo.
{{%attachments title="CloudFormation Templates" style="green" pattern="AWSTransitGatewayWorkshop.zip"/%}}

Trong bước này, chúng ta sẽ cùng triển khai các tài nguyên như VPC và máy ảo EC2 để chuẩn bị cho các bước cấu hình mạng tiếp theo. Trong các bước tiếp theo, cấu hình mạng sẽ tương đối phức tạp, đặt biệt là route table. Vì mục tiêu chính của workshop này là hiểu được cách hoạt động của AWS Transit Gateway nên trong mỗi VPC, chúng ta sẽ chỉ tạo 1 EC2 instance để kiểm tra kết nối đến khác EC2 instance trong các VPC khác sau khi đã cấu hình xong Transit Gateway.

Trong các bước tiếp theo, chúng ta sẽ cùng triển khai trên 2 region là **ap-northeast-1 (Tokyo)** và **ap-southeast-1 (Singapore)**.

<!-- TODO: Thêm sơ đồ 3 VPC có EC2 ở trong, tách biệt với nhau chưa có kết nối gì -->

#### Triển khai CloudFormation Stack

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