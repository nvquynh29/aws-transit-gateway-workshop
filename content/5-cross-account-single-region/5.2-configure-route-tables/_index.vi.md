---
title : "Cấu hình bảng định tuyến"
date : "`r Sys.Date()`"
weight : 2
chapter : false
pre : " <b> 5.2 </b> "
---
#### Chia sẻ AWS Transit Gateway bằng AWS Resource Access Maanger (AWS RAM)
1. Truy cập AWS RAM
- Tại giao diện AWS Console của tài khoản A, tìm kiếm từ khoá `ram`
- Truy cập dịch vụ **Resource Access Manager**
![Share Transit Gateway](/images/5-cross-account-single-region/share_tgw_via_ram_1.png)
2. Tạo share resource
- Chọn **Resource shares** trong phần **Shared by me** rồi bấm **Create resource share**
![Share Transit Gateway](/images/5-cross-account-single-region/share_tgw_via_ram_2.png)
- Cấu hình như sau:
  - Name: `tokyo-shared-tgw-via-ram`
  - Resources: Chọn **Transit Gateways** rồi chọn **tokyo-tgw**
  ![Share Transit Gateway](/images/5-cross-account-single-region/share_tgw_via_ram_3.png)
3. Bấm next cho đến bước thêm Principals, sao chép id của tài khoản B (12 chữ số) rồi sau đó thêm
![Share Transit Gateway](/images/5-cross-account-single-region/share_tgw_via_ram_4.png)
Sau khi tạo xong resource share thì giao diện sẽ trông như sau:
![Share Transit Gateway](/images/5-cross-account-single-region/share_tgw_via_ram_5.png)

4. Quay lại tài khoản B, truy cập đến dịch vụ AWS RAM, chọn **Resource shares** trong phần **Shared with me** sẽ thấy có 
một lời mời với trạng thái là Pending.
![Share Transit Gateway](/images/5-cross-account-single-region/share_tgw_via_ram_6.png)
Bấm vào lời mời đó rồi bấm **Accept resource share**
![Share Transit Gateway](/images/5-cross-account-single-region/share_tgw_via_ram_7.png)

#### Cấu hình bảng định tuyến
1. Tạo transit gateway attachment cho Partner VPC
- Tại giao diện **Transit gateways** của tài khoản B, chọn transit gateway mới được share rồi đổi tên thành `tokyo-shared-tgw`
![Configure route tables](/images/5-cross-account-single-region/configure_route_tables_1.png)
- Tạo transit gateway attachment **partner-att** gắn với transit gateway này và **partner-vpc**
![Configure route tables](/images/5-cross-account-single-region/configure_route_tables_2.png)
![Configure route tables](/images/5-cross-account-single-region/configure_route_tables_3.png)
Sau khi tạo xong bạn sẽ thấy trạng thái của attachment này là **Pending Acceptance** vì tài khoản B đang tạo tài nguyên gắn với tài nguyên được chia sẻ từ tài khoản A nên cần phải được tài khoản A xác nhận trước khi tạo.
![Configure route tables](/images/5-cross-account-single-region/configure_route_tables_4.png)

5. Quay lại giao diện **Transit gateway attachments** của tài khoản A sẽ thấy attachment vừa được tạo bởi tài khoản B.
Sửa tên của attachment này thành **partner-att** rồi chấp nhận như sau:
![Configure route tables](/images/5-cross-account-single-region/configure_route_tables_5.png)
![Configure route tables](/images/5-cross-account-single-region/configure_route_tables_6.png)

6. Cấu hình bảng định tuyến cho transit gateway

Tạo transit gateway route table mới **partner-tgw-rtb** trong tài khoản A
![Configure route tables](/images/5-cross-account-single-region/configure_route_tables_7.png)
Xoá association giữa **partner-att** và **default-tgw-rtb**
![Configure route tables](/images/5-cross-account-single-region/configure_route_tables_8.png)
<!-- TODO: Add note to show that user can update transit gateway config but this demo want to show errors -->
Thêm association giữa bảng định tuyến **partner-tgw-rtb** với **partner-att**
![Configure route tables](/images/5-cross-account-single-region/configure_route_tables_9.png)
![Configure route tables](/images/5-cross-account-single-region/configure_route_tables_10.png)

Thêm propagation từ bảng định tuyến **partner-tgw-rtb** tới **dev-att** attachment.
![Configure route tables](/images/5-cross-account-single-region/configure_route_tables_11.png)
![Configure route tables](/images/5-cross-account-single-region/configure_route_tables_12.png)

Thêm propagation từ bảng định tuyến **dev-tgw-rtb** tới **partner-att** attachment.
![Configure route tables](/images/5-cross-account-single-region/configure_route_tables_13.png)
![Configure route tables](/images/5-cross-account-single-region/configure_route_tables_14.png)

7. Cấu hình bảng định tuyến cho các VPC
- Tại giao diện **Route tables** của tài khoản A, chọn **dev-rtb** rồi thêm route mới với destination là `10.5.0.0/16` và 
target là **dev-att** attachment.
![Configure route tables](/images/5-cross-account-single-region/configure_route_tables_15.png)
![Configure route tables](/images/5-cross-account-single-region/configure_route_tables_16.png)

Lặp lại bước trên để thêm route có destination là `10.1.0.0/16` và target là **partner-att** attachment cho bảng định tuyến **partner-rtb** trong tài khoản B.
![Configure route tables](/images/5-cross-account-single-region/configure_route_tables_17.png)
![Configure route tables](/images/5-cross-account-single-region/configure_route_tables_18.png)

8. Kiểm tra cấu hình
Truy cập đến dịch vụ EC2 của tài khoản B rồi chọn máy ảo **partner** sau đó sao chép private IPv4.
![Configure route tables](/images/5-cross-account-single-region/configure_route_tables_15.png)

Kết nối đến máy ảo **dev** bằng EC2 instance connect rồi chạy lệnh sau để kiểm tra kết nối.
```shell
ping <partner_instance_private_ipv4> -c5
```
Kết quả cho thấy chúng ta đã cấu hình thành công. Bạn có thể kết nối từ máy ảo **partner** đến **dev** sẽ thấy kết quả tương tự.
![Configure route tables](/images/5-cross-account-single-region/configure_route_tables_16.png)

Hãy thử cấu hình thêm để kết nối giữa Test VPC và Partner VPC để hiểu hơn về cách thực hiện nhé.
