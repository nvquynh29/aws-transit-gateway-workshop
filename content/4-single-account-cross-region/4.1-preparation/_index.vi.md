---
title : "Chuẩn bị"
date : "`r Sys.Date()`"
weight : 1
chapter : false
pre : " <b> 4.1 </b> "
---

Trong bước này chúng ta sẽ triển khai các tài nguyên như VPC, EC2 instance, AWS Transit Gateway và bảng định tuyến trong
region **ap-southeast-1 (Singapore)**.

#### CloudFormation Stack
Phần triển khai này rất giống với phần trước, chỉ có một vài điểm cần lưu ý sau đây:
- Sử dụng file **VPC.yaml**
![Deploy CloudFormation Stack](/images/4-single-account-cross-region/preparation_1.png)

Điền các tham số như sau:
- Stack name: `BranchVPC`
- EC2InstanceAMIId: Chọn AMI ID của **Amazon Linux 2023**
- SubnetCidr: `10.4.0.0/24`
- VPCCidr: `10.4.0.0/16`
- VPCPrefix: `branch`
![Deploy CloudFormation Stack](/images/4-single-account-cross-region/preparation_2.png)

#### Transit Gateway
1\. Truy cập dịch vụ VPC, chọn **Transit gateways** rồi bấm **Create transit gateway**
![Deploy CloudFormation Stack](/images/4-single-account-cross-region/preparation_3.png)

Cấu hình transit gateway như sau:
- Name tag: `singapore-tgw`
- Description: `Transit Gateway for region Singapore`
- Bỏ chọn **Default route table association** và **Default route table propagation**

{{% notice note %}}
Bỏ chọn default route table association và propagation để khi các attachment mới được tạo ra không tự tạo association và 
propagation với bảng định tuyến mặc định.
{{% /notice %}}

![Deploy CloudFormation Stack](/images/4-single-account-cross-region/preparation_4.png)

2\. Tạo một transit gateway attachment nữa với cấu hình như sau
- Name tag: `branch-att`
- Transit gateway ID: Chọn **singapore-tgw**
- Attachment Type: **VPC**
- VPC ID: Chọn **branch-vpc**
![Configure route tables](/images/4-single-account-cross-region/configure_route_tables_4.png)

3\. Tạo transit gateway route table mới và liên kết với attachment
- Tại giao diện **Transit gateway route tables** của region Singapore, bấm **Create transit gateway route table**
![Configure route tables](/images/4-single-account-cross-region/configure_route_tables_5.png)
- Điền các thông tin cấu hình như sau:
  - Name tag: `branch-tgw-rtb`
  - Transit gateway ID: **singapore-tgw**

  ![Configure route tables](/images/4-single-account-cross-region/configure_route_tables_6.png)

Sau khi tạo xong **branch-tgw-rtb** thì chọn tab **Associations** rồi bấm **Create association** rồi liên kết
với **branch-att** attachment.
![Configure route tables](/images/4-single-account-cross-region/configure_route_tables_7.png)
![Configure route tables](/images/4-single-account-cross-region/configure_route_tables_8.png)

{{% notice note %}}
Hãy mở sẵn 2 tab chọn region Tokyo và Singapore trong trình duyệt của bạn để tiết kiệm thời gian khi cấu hình.
{{% /notice %}}
