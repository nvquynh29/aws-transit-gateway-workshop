---
title : "Nhiều bảng định tuyến"
date : "`r Sys.Date()`"
weight : 4
chapter : false
pre : " <b> 3.4 </b> "
---

Từ bước này trở đi, chúng ta sẽ chuyển cấu hình của AWS Transit Gateway từ việc sử dụng một bảng định tuyến sang sử dụng 
nhiều bảng định tuyến.

Như đã đề cập ở phần trước, mỗi attachment mới được tạo ra sẽ tự động liên kết với bảng định tuyến mặc định. Mặc dù điều
này giúp đơn giản hoá việc cấu hình, nó cũng khiến cho bảng định tuyến trở nên phức tạp, khó quản lý và theo dõi khi số
lượng attachment và route tăng lên. Ngoài ra, vì tất cả các attachment đều có association và propagation nên tất cả các
attachment kết nối đến cùng Transit Gateway đều có thể định tuyến đến các attachment khác. Trong nhiều trường hợp, 
bạn sẽ không muốn như vậy mà muốn nhiều quyền kiểm soát việc điều hướng lưu lượng cho từng thành phần trong mạng. 
Để làm được như vậy bạn có thể sử dụng nhiều bảng định tuyến, mỗi bảng định tuyến chỉ liên kết với một attachment 
hoặc sử dụng **Transit Gateway policy table**.

#### Cấu hình định tuyến
1\. Xoá liên kết giữa các transit gateway attachment và bảng định tuyến mặc định
- Tại giao diện VPC chọn **Transit gateway route tables**
- Chọn transit gateway route table duy nhất rồi sửa tên thành `default-tgw-rtb`
- Chọn tab **Associations**
- Chọn một liên kết bất kỳ rồi bấm **Delete association**
![Multiple route tables](/images/3-single-account-single-region/multiple_route_table_1.png)
- Tiếp tục xoá các liên kết còn lại cho đến khi hết
![Multiple route tables](/images/3-single-account-single-region/multiple_route_table_2.png)

2\. Kiểm tra kết nối từ máy ảo **dev** đến máy ảo **share** 
Quay lại giao diện **EC2 instance connect** của máy ảo **dev** rồi thử kết nối đến máy ảo **share** ta sẽ thấy không thể 
kết nối đến được nữa. Nguyên nhân là vì chúng ta đã xoá liên kết giữa các transit gateway attachment với bảng định tuyến 
mặc định mà chưa cấu hình các bảng định tuyến mới.
![Multiple route tables](/images/3-single-account-single-region/multiple_route_table_3.png)

3\. Tạo các bảng định tuyến mới
- Tại giao diện transit gateway route tables, bấm **Create transit gateway route table**
![Multiple route tables](/images/3-single-account-single-region/multiple_route_table_4.png)
- Điền tên cho bảng định tuyến mới là `dev-tgw-rtb`
- Chọn transit gateway cho bảng định tuyến rồi bấm **Create transit gateway route table**
![Multiple route tables](/images/3-single-account-single-region/multiple_route_table_5.png)

  Lặp lại các bước trên để tạo thêm 2 bảng định tuyến mới là `test-tgw-rtb` và `share-tgw-rtb` như sau
  ![Multiple route tables](/images/3-single-account-single-region/multiple_route_table_6.png)
  ![Multiple route tables](/images/3-single-account-single-region/multiple_route_table_7.png)

4\. Tạo liên kết mới cho các transit gateway attachment
- Tại giao diện transit gateway route tables, chọn **dev-tgw-rtb**
- Chọn tab **Associations**
- Bấm **Create association**
![Multiple route tables](/images/3-single-account-single-region/multiple_route_table_8.png)
Chọn **dev-att** rồi bấm **Create association**
![Multiple route tables](/images/3-single-account-single-region/multiple_route_table_9.png)

  Lặp lại các bước trên để tạo liên kết của **test-tgw-rtb** và **share-tgw-rtb** như sau
  ![Multiple route tables](/images/3-single-account-single-region/multiple_route_table_10.png)
  ![Multiple route tables](/images/3-single-account-single-region/multiple_route_table_11.png)

5\. Tạo propagation
- Tại giao diện transit gateway route tables, chọn **dev-tgw-rtb**
- Chọn tab **Propagations** rồi bấm **Create propagation**
![Multiple route tables](/images/3-single-account-single-region/multiple_route_table_12.png)
Chọn **share-att** rồi bấm **Create propagation**
![Multiple route tables](/images/3-single-account-single-region/multiple_route_table_13.png)
Sau khi tạo xong propagation thì sẽ có một route tự động được tạo như sau
![Multiple route tables](/images/3-single-account-single-region/multiple_route_table_14.png)

  Làm tương tự để tạo propagation từ **share-tgw-rtb** tới **dev-att**
  ![Multiple route tables](/images/3-single-account-single-region/multiple_route_table_15.png)
  ![Multiple route tables](/images/3-single-account-single-region/multiple_route_table_16.png)

6\. Kiểm tra kết nối từ máy ảo **dev** tới máy ảo **share**
![Multiple route tables](/images/3-single-account-single-region/multiple_route_table_17.png)

#### Tự thực hành
Lặp lại các bước trên để cấu hình kết nối từ **test** instance đến **share** instance rồi kiểm tra kết quả của bạn.
![Multiple route tables](/images/3-single-account-single-region/multiple_route_table_18.png)
