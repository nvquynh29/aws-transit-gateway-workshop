---
title : "Cấu hình định tuyến"
date : "`r Sys.Date()`"
weight : 2
chapter : false
pre : " <b> 6.2 </b> "
---
Trong phần này chúng ta sẽ cùng cấu hình để kết nối 2 VPCs là Test và Customer. Phần cấu hình này rất giống với phần 4 nên các
bước dưới đây chỉ mô tả ngắn gọn chứ không chi tiết.

#### 1. Cấu hình peering giữa các transit gateway
Tạo transit gateway attachment mới trong tài khoản thứ hai với cấu hình như sau:
- Name tag: `peering-company-x-att`
- Transit gateway ID: Chọn **customer-tgw**
- Attachment type: **Peering connection**
- Account: Other account
- Account ID: ID của tài khoản thứ nhất
- Region: Tokyo
- Transit gateway (accepter): ID của transit gateway **tokyo-tgw**
![Peering transit gateways](/images/6-cross-account-cross-region/configure_route_tables_1.png)

Quay lại giao diện **Transit gateway attachments** của tài khoản thứ nhất sẽ thấy một attachment có trạng thái là 
**Pending Acceptance**. Đặt tên cho attachment này là **peering-customer-att** rồi chấp nhận.
![Peering transit gateways](/images/6-cross-account-cross-region/configure_route_tables_2.png)
![Peering transit gateways](/images/6-cross-account-cross-region/configure_route_tables_3.png)

Tại giao diện **Transit gateway route tables** của tài khoản thứ nhất, chọn **test-tgw-rtb** rồi thêm static route mới với
CIDR `10.6.0.0/16` và attachment **peering-customer-att**.
![Peering transit gateways](/images/6-cross-account-cross-region/configure_route_tables_4.png)
![Peering transit gateways](/images/6-cross-account-cross-region/configure_route_tables_5.png)

Làm tương tự để thêm static route mới cho **customer-tgw-rtb** trong tài khoản thứ hai với CIDR `10.2.0.0/16` và attachment **peering-company-x-att**.
![Peering transit gateways](/images/6-cross-account-cross-region/configure_route_tables_6.png)
![Peering transit gateways](/images/6-cross-account-cross-region/configure_route_tables_7.png)

#### 2. Cấu hình bảng định tuyến cho các transit gateway
Tạo thêm transit gateway route table mới trong tài khoản thứ hai tên là `peering-company-x-tgw-rtb` gắn với **customer-tgw** sau đó tạo association với **peering-company-x-att** attachment và propagation tới **customer-att** attachment.
![Configure route tables](/images/6-cross-account-cross-region/configure_route_tables_8.png)
![Configure route tables](/images/6-cross-account-cross-region/configure_route_tables_9.png)
![Configure route tables](/images/6-cross-account-cross-region/configure_route_tables_10.png)
![Configure route tables](/images/6-cross-account-cross-region/configure_route_tables_11.png)

Làm tương tự để tạo transit gateway route table `peering-customer-tgw-rtb` trong tài khoản thứ nhất gắn với **tokyo-tgw**
sau đó tạo association với **peering-customer-att** attachment và propagation tới **test-att** attachment.
![Configure route tables](/images/6-cross-account-cross-region/configure_route_tables_12.png)
![Configure route tables](/images/6-cross-account-cross-region/configure_route_tables_13.png)
![Configure route tables](/images/6-cross-account-cross-region/configure_route_tables_14.png)
![Configure route tables](/images/6-cross-account-cross-region/configure_route_tables_15.png)

#### 3. Cấu hình bảng định tuyến cho các VPC
Thêm route mới với destination `10.6.0.0/16` và target **test-att** cho bảng định tuyến **test-rtb** của tài khoản thứ nhất.
![Configure route tables](/images/6-cross-account-cross-region/configure_route_tables_16.png)
![Configure route tables](/images/6-cross-account-cross-region/configure_route_tables_17.png)

Làm tương tự để thêm route có destination `10.2.0.0/16` và target **customer-att** cho bảng định tuyến **customer-rtb** của tài khoản thứ hai.
![Configure route tables](/images/6-cross-account-cross-region/configure_route_tables_18.png)

#### 4. Kiểm tra kết nối
Kết nối đến máy ảo **test** rồi bằng EC2 instance connect rồi thử ping đến máy ảo **customer** để kiểm tra xem cấu hình mạng đã đúng chưa.
![Check network configuration](/images/6-cross-account-cross-region/configure_route_tables_19.png)

#### Tự thực hành
Hãy thử cấu hình thêm để kết nối giữa Dev VPC và Customer VPC để hiểu hơn về cách thực hiện nhé.
