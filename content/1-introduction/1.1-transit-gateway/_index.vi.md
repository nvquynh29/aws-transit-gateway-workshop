---
title : "AWS Transit Gateway"
date : "`r Sys.Date()`"
weight : 1
chapter : false
pre : " <b> 1.1 </b> "
---

Trong phần này chúng ta sẽ cùng tìm hiểu AWS Transit Gateway là gì và tại sao tính năng này lại cần thiết.

#### Amazon VPC và VPC Peering
Amazon Virtual Private Cloud (VPC) là một mạng ảo nằm cô lập trong AWS Cloud. Amazon VPC rất giống với mạng truyền thống, 
cho phép người dùng triển khai các máy chủ, cơ sở dữ liệu và các tài nguyên máy tính khác. 

Trên thực tế, có rất nhiều trường hợp chúng ta cần kết nối hai hoặc nhiều VPC với nhau để chia sẻ tài nguyên, dữ liệu 
giữa các mạng này. Cách đơn giản nhất để kết nối các mạng này là đi qua đường truyền Internet, tuy nhiên cách này 
tiềm ẩn rất nhiều rủi ro về bảo mật, không đáng tin cậy và không đảm bảo hiệu suất.

**VPC Peering** là tính năng cho phép kết nối hai hoặc nhiều Amazon VPC với nhau thông qua hệ thống mạng riêng của AWS, 
không sử dụng đường truyền Internet. Khi sử dụng VPC Peering để kết nối 2 VPC, các tài nguyên trong một mạng có thể 
kết nối với các tài nguyên trong mạng còn lại thông qua địa chỉ IP riêng tư như thể chúng nằm trong cùng một mạng.
![VPC Peering](/images/1-introduction/vpc_peering.svg)

Tuy VPC Peering là một tính năng hiệu quả để kết nối các VPC với nhau nhưng nó cũng có một số nhược điểm. Một trong số đó
phải kể đến là khi phải số lượng kết nối lớn khi cần kết nối nhiều VPC. VPC Peering có đặc điểm là không có tính chất bắc cầu,
tức là trong trường hợp VPC A đã peering với VPC B, VPC B đã peering với VPC C thì VPC A không kết nối được đến VPC C. 
Ngoài ra một Amazon VPC không thể peering với quá nhiều VPC khác.
![VPC Peering Overwhelmed](/images/1-introduction/vpc_peering_overwhelmed.svg)
Khi cần kết nối nhiều VPC với nhau, cấu hình định tuyến sẽ trở nên rất phức tạp và khó quản lý cũng như xử lý sự cố.

#### AWS Transit Gateway
**AWS Transit Gateway** là một tính năng mạnh mẽ của VPC cho phép kết nối nhiều VPC, VPN connection, mạng on-premises 
với một cổng duy nhất. AWS Transit Gateway giúp đơn giản hoá cấu hình mạng bằng cách loại bỏ các kết nối ngang hàng giữa 
các VPC với nhau, các VPC sẽ kết nối đến một hub trung tâm duy nhất, đóng vai trò điều hướng lưu lượng truy cập mạng. 
So với VPC Peering, AWS Transit Gateway có một số ưu điểm sau:
- Nâng cao hiệu suất: Transit Gateway mang lại hiệu suất tốt hơn nhiều bằng việc tận dụng cơ chế định tuyến tập trung,
giảm chi phí liên quan đến các kết nối ngang hàng giữa các VPC.
- Đơn giản hoá việc quản lý: Bằng cách loại bỏ các kết nối ngang hàng giữa các VPC, tất cả các cấu hình định tuyến được
quản lý tập trung giúp cho việc quản lý trở nên dễ dàng hơn nhiều.
- Khả năng mở rộng: Transit Gateway có khả năng mở rộng cao, có khả năng xử lý số lượng lớn VPC và kết nối mà không ảnh
hưởng đến hiệu và khả năng quản lý. Điều này làm cho nó trở nên lý tưởng cho việc cấu hình mạng quy mô lớn.
- Cải thiện khả năng khắc phục sự cố: Việc khắc phục sự cố mạng trở nên dễ dàng hơn với Transit Gateway vì kiến trúc 
tập trung cung cấp cái nhìn tổng quan, rõ rằng về lưu lượng mạng và đơn giản háo việc xác định các sự cố định tuyến.

Vì những ưu điểm này, AWS Transit Gateway đã trở thành lựa chọn phổ biến để triển khai các cấu hình mạng phức tạp. Nó
đặc biệt phù hợp với các tổ chức cần triển khai hệ thống quy mô lớn, cần kết nối nhiều VPC, VPN Connection và mạng on-premises
trong khi vẫn đảm bảo hiệu suất, khả năng quản lý và mở rộng.
![AWS Transit Gateway](/images/1-introduction/aws_transit_gateway.svg)

Để sử dụng hiệu quả Transit Gateway, cần phải hiểu được các tình huống có thể liên quan:
- Các VPC nằm trên cùng tài khoản AWS, cùng region
- Các VPC nằm trên cùng tài khoản AWS, khác region
- Các VPC nằm trên khác tài khoản AWS, cùng region
- Các VPC nằm trên khác tài khoản AWS, khác region

Bằng cách hiểu rõ những tình huống này, bạn có thể sử dụng AWS Transit Gateway một cách hiệu quả để kết nối nhiều VPC trên
các cấu hình khác nhau, đơn giản hóa việc quản lý mạng và nâng cao hiệu suất mạng.
