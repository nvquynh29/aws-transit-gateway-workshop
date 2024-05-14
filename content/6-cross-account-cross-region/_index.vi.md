---
title : "Khác tài khoản - Khác Region"
date : "`r Sys.Date()`"
weight : 6
chapter : false
pre : " <b> 6. </b> "
---
Trong phần này, chúng ta sẽ cùng cấu hình AWS Transit Gateway kết nối các VPC trong các tài khoản và region khác nhau.

**Tình huống**: Sau khi thành công trong lĩnh vực tự phát triển sản phẩm, công ty bạn có nhiều khách hàng lớn. Một trong 
số những khách hàng này muốn hợp tác với công ty bạn để phát triển một hệ thống cho công ty của họ. Công ty khách hàng
đã có sẵn một VPC là Customer VPC và Transit Gateway có thể cho phép mạng của công ty bạn kết nối đến. Đội phát triển
cần lấy dữ liệu từ khác hàng trong quá trình phát triển hệ thống nhưng công ty khách hàng chỉ đồng ý chia sẻ dữ liệu này
thông qua đường truyền mạng riêng vì lý do bảo mật. Ngoài ra, trước khi release sản phẩm, họ muốn sử dụng hệ thống trước.

Trong trường hợp này, bạn không nên kết nối trực tiếp Dev VPC tới Customer VPC vì môi trường Dev chưa được kiểm thử
cẩn thận nên rất dễ xảy ra lỗi khi khách hàng sử dụng. Thay vào đó, bạn nên cấu hình để kết nối Test VPC đến Customer VPC
vì môi trường Test đã được kiểm thử cẩn thận và môi trường Dev đang sử dụng chung data source với môi trường này nên 
nó có thể đáp ứng được yêu cầu của cả khách hàng và đội phát triển. Cách cấu hình rất giống với các phần trước. Cấu hình
mạng của bạn sẽ trông như sau:
<!-- TODO: Sơ đồ 2 Transit Gateway kết nối với nhau thông qua peering, có bảng định tuyến (khó vẽ) -->

#### Nội dung

1. [Chuẩn bị](4.1-preparation/)
2. [Cấu hình định tuyến](4.2-configure-route-tables/)