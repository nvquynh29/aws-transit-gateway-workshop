---
title : "Cấu hình định tuyến"
date : "`r Sys.Date()`"
weight : 2
chapter : false
pre : " <b> 4.2 </b> "
---

Trong bước này, chúng ta sẽ cấu hình định tuyến để kết nối Share VPC với Branch VPC.

1\. Tại giao diện **Transit gateways** của region Tokyo, chọn **tokyo-tgw** rồi sao chép id của transit gateway này
![Configure route tables](/images/4-single-account-cross-region/configure_route_tables_1.png)

2\. Tại giao diện **Transit gateway attachments** của region Singapore, bấm **Create transit gateway attachment**
![Configure route tables](/images/4-single-account-cross-region/configure_route_tables_2.png)

Điền các thông tin cấu hình như sau:
- Name tag: `peering-tokyo`
- Transit gateway ID: Chọn **singapore-tgw**
- Attachment Type: **Peering connection**
- Account: **My account**
- Region: **Tokyo**
- Transit gateway (accepter): ID của transit gateway **tokyo-tgw** đã sao chép ở bước 1
![Configure route tables](/images/4-single-account-cross-region/configure_route_tables_3.png)

3\. Quay lại giao diện **Transit gateway attachments** của region Tokyo sẽ thấy có một attachment đang ở trạng thái
**Pending Acceptance**.

Đổi tên của attachment này thành `peering-singapore` rồi chọn **Actions** sau đó bấm **Accept transit gateway attachment**
![Configure route tables](/images/4-single-account-cross-region/configure_route_tables_9.png)
![Configure route tables](/images/4-single-account-cross-region/configure_route_tables_10.png)

4\. Cấu hình peering giữa 2 transit gateways

Quay lại giao diện **Transit gateway route tables** của region Singapore, chọn **branch-tgw-rtb** rồi chọn tab **Routes** sau đó bấm **Create static route**
![Configure route tables](/images/4-single-account-cross-region/configure_route_tables_11.png)
Điền CIDR là `10.3.0.0/16` (CIDR của Share VPC) rồi chọn **peering-tokyo** attachment.
![Configure route tables](/images/4-single-account-cross-region/configure_route_tables_12.png)
Lặp lại bước trên với **share-tgw-rtb** của region Tokyo, điền CIDR là `10.4.0.0/16` (CIDR của Branch VPC) rồi chọn **peering-singapore** attachment.
![Configure route tables](/images/4-single-account-cross-region/configure_route_tables_13.png)
![Configure route tables](/images/4-single-account-cross-region/configure_route_tables_14.png)
Sau khi tạo xong thì sẽ có một static route mới với type là Peering như sau:
![Configure route tables](/images/4-single-account-cross-region/configure_route_tables_15.png)

5\. Cấu hình bảng định tuyến cho từng transit gateway

Tạo một transit gateway route table mới trong region Singapore tên là `peering-tokyo-tgw-rtb` và gắn vào **singapore-tgw** transit gateway.
![Configure route tables](/images/4-single-account-cross-region/configure_route_tables_16.png)
Tạo association giữa **peering-tokyo-tgw-rtb** và **peering-tokyo** attachment.
![Configure route tables](/images/4-single-account-cross-region/configure_route_tables_17.png)
![Configure route tables](/images/4-single-account-cross-region/configure_route_tables_18.png)
Tạo propagation từ **peering-tokyo-tgw-rtb** đến **branch-att** attachment.
![Configure route tables](/images/4-single-account-cross-region/configure_route_tables_19.png)
![Configure route tables](/images/4-single-account-cross-region/configure_route_tables_20.png)

Lặp lại các bước trên để tạo một transit gateway route table mới trong region Tokyo tên là `peering-singapore-tgw-rtb` và thêm association với **peering-singapore**, propagation tới **share-att** như sau:
![Configure route tables](/images/4-single-account-cross-region/configure_route_tables_21.png)
![Configure route tables](/images/4-single-account-cross-region/configure_route_tables_22.png)
![Configure route tables](/images/4-single-account-cross-region/configure_route_tables_23.png)

{{% notice note %}}
Khi bạn tạo associate với **peering-singapore** attachment, có thể bạn sẽ gặp lỗi như sau. Nguyên nhân là vì hiện 
tại attachment này đang được associate với **default-tgw-rtb**, bạn cần phải xoá association này trước rồi mới tạo được association mới.
{{% /notice %}}
![Configure route tables](/images/4-single-account-cross-region/configure_route_tables_24.png)
Xoá association giữa **peering-singapore** attachment và bảng định tuyến **default-tgw-rtb**.
![Configure route tables](/images/4-single-account-cross-region/configure_route_tables_25.png)
Tạo lại association giữa **peering-singapore** attachment và bảng định tuyến **peering-singapore-tgw-rtb**.
![Configure route tables](/images/4-single-account-cross-region/configure_route_tables_26.png)
Tạo propagation từ **peering-singapore-tgw-rtb** tới **share-att**.
![Configure route tables](/images/4-single-account-cross-region/configure_route_tables_27.png)
![Configure route tables](/images/4-single-account-cross-region/configure_route_tables_28.png)

6\. Cấu hình bảng định tuyến của các VPC

Tại giao diện **Route tables** của region Tokyo, chọn **share-rtb** sau đó chọn tab **Routes** rồi bấm **Edit routes**
![Configure route tables](/images/4-single-account-cross-region/configure_route_tables_29.png)
Thêm một route mới với destination là `10.4.0.0/16` và target là **share-att**
![Configure route tables](/images/4-single-account-cross-region/configure_route_tables_30.png)

Lặp lại các bước trên để thêm route mới cho **branch-rtb** route table trong region Singapore với destination là 
`10.3.0.0/16` và target là **branch-att**.
![Configure route tables](/images/4-single-account-cross-region/configure_route_tables_31.png)
![Configure route tables](/images/4-single-account-cross-region/configure_route_tables_32.png)

7\. Kiểm tra kết nối giữa các máy ảo

Chúng ta sẽ thử kết nối từ máy ảo **share** của region Tokyo với máy ảo **branch** của region Singapore để kiểm tra xem Share VPC và Branch VPC đã kết nối được với nhau chưa.

Tại giao diện EC2 của region Singapore, sao chép private IPv4 của máy ảo **branch**
![Configure route tables](/images/4-single-account-cross-region/configure_route_tables_33.png)
Kết nối đến máy ảo **share** bằng EC2 instance connect như các phần trước rồi chạy lệnh sau:
```shell
ping <branch_instance_private_ipv4> -c5
```
Kết quả cho thấy chúng ta đã cấu hình thành công.
![Configure route tables](/images/4-single-account-cross-region/configure_route_tables_34.png)

#### Tự thực hành
Hãy thử lặp lại các bước trên để cấu hình kết nối giữa Test VPC và Branch VPC và kiểm tra cấu hình của bạn.
![Configure route tables](/images/4-single-account-cross-region/configure_route_tables_35.png)
