---
title : "Một bảng định tuyến"
date : "`r Sys.Date()`"
weight : 3
chapter : false
pre : " <b> 3.3 </b> "
---

Trong bước này, chúng ta sẽ cấu hình định tuyến Dev VPC, Test VPC để kết nối tới Share VPC chỉ sử dụng bảng định tuyến mặc định.

#### Kiểm tra kết nối giữa các máy ảo
1\. Kết nối đến máy ảo **dev**
- Truy cập dịch vụ **EC2** rồi chọn **Instances**, sau đó chọn máy ảo **dev**
- Bấm **Connect**
![Single route table](/images/3-single-account-single-region/single_route_table_1.png)
- Giữ nguyên các cấu hình mặc định rồi bấm **Connect**
![Single route table](/images/3-single-account-single-region/single_route_table_2.png)

2\. Kiểm tra kết nối từ máy ảo **dev** đến **share**
- Quay lại giao diện danh sách các máy ảo rồi chọn máy ảo **share**
- Sao chép private IPv4 của máy ảo này
![Single route table](/images/3-single-account-single-region/single_route_table_3.png)
- Quay lại giao diện **EC2 instance connect** của **dev** instance rồi chạy lệnh sau:
  ```shell
  ping <share_instance_private_ipv4> -c5
  ```
- Sau một khoảng thời gian đợi thì ta nhận được kết quả báo không thể kết nối đến máy ảo **share**. Nguyên nhân là vì 
2 máy ảo này đang nằm trong 2 VPC chưa được kết nối với nhau.
![Single route table](/images/3-single-account-single-region/single_route_table_4.png)

#### Cấu hình định tuyến cho Dev VPC
1\. Cấu hình bảng định tuyến cho Dev VPC
- Quay lại giao diện của VPC, chọn **Route tables**
- Chọn **dev-rtb**
- Chọn tab **Routes** rồi bấm **Edit routes**
![Single route table](/images/3-single-account-single-region/single_route_table_5.png)

  Thêm một route mới với destination là `10.3.0.0/16` (CIDR của Share VPC), phần target thì chọn **Transit Gateway** rồi chọn **dev-att** sau đó bấm **Save changes**
  ![Single route table](/images/3-single-account-single-region/single_route_table_6.png)

2\. Cấu hình bảng định tuyến cho **share-vpc** VPC bằng cách lặp lại bước trên
  ![Single route table](/images/3-single-account-single-region/single_route_table_7.png)
  ![Single route table](/images/3-single-account-single-region/single_route_table_8.png)

3\. Kiểm tra lại kết nối
- Quay lại giao diện **EC2 instance connect** rồi chạy lại lệnh sau:
  ```shell
  ping <private_ipv4_share_instance> -c5
  ```

Kết quả cho thấy đã có thể kết nối từ máy ảo **dev** đến máy ảo **share** chứng tỏ Dev VPC và Share VPC đã kết nối được với nhau thông qua transit gateway.
![Single route table](/images/3-single-account-single-region/single_route_table_9.png)

#### Cấu hình bảng định tuyến cho Test VPC
Lặp lại các bước trên để cấu hình bảng định tuyến cho **test-vpc** VPC. Sau khi cấu hình xong, bạn có thể kiểm tra cấu 
hình của mình bằng cách kết nối từ máy ảo **test** đến máy ảo **share** như sau.
![Single route table](/images/3-single-account-single-region/single_route_table_10.png)
