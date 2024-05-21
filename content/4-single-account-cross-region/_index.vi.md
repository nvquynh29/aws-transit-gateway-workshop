---
title : "Cùng tài khoản - Khác Region"
date : "`r Sys.Date()`"
weight : 4
chapter : false
pre : " <b> 4. </b> "
---
Trong phần này, chúng ta sẽ cùng cấu hình AWS Transit Gateway kết nối các VPC trong cùng một tài khoản AWS nhưng khác region.

**Tình huống**: Sau một thời gian, công ty bạn phát triển và bắt đầu mở chi nhánh thứ hai ở một nước khác. 
Sản phẩm đầu tiên của chi nhánh này là một website được thiết kế với mục đích marketing. Website này cần lấy dữ liệu từ database 
để thống kê, làm báo cáo về những con số ấn tượng mà công ty bạn đã đạt được. Vì chi nhánh mới thành lập cho nên chưa 
có nhiều kỹ sư, bạn được giao nhiệm vụ hỗ trợ đội phát triển chuẩn bị môi trường triển khai sản phẩm trên AWS. Để tiết
kiệm thời gian và chi phí, bạn muốn sử dụng Share VPC để chia sẻ database và cache giống như phần trước.

Chi nhánh đó đã chuẩn bị sẵn một VPC tên là Branch và một Transit Gateway. Việc của bạn là kết nối Branch VPC với Share VPC 
để chia sẻ tài nguyên. Tương tự như phần trước, bạn hoàn toàn có thể sử dụng VPC Peering. Nhưng trong phần này, chúng ta 
sẽ cùng tìm hiểu một cách cấu hình khác đó là dùng AWS Transit Gateway. Khi đó, cấu hình mạng của bạn sẽ trông như sau:
![Diagram](/images/4-single-account-cross-region/single_account_cross_region.svg)

Trông sơ đồ này rất phức tạp phải không, hãy sử dụng những kiến thức về Route table association và Route propagation trong
phần giới thiệu để giải thích đồ này. Hãy hiểu đơn giản là lưu lượng truy cập sẽ đi từ Share VPC đến Tokyo TGW rồi sau
đó đi tới Singapore TGW thông qua Peering Attachment. Sau khi lưu lượng truy cập đến được đây thì nó sẽ tiếp tục được 
định tuyến đến Branch VPC.


#### Nội dung

1. [Chuẩn bị](4.1-preparation/)
2. [Cấu hình định tuyến](4.2-configure-route-tables/)