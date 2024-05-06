---
title : "AWS Transit Gateway"
date : "`r Sys.Date()`"
weight : 1
chapter : false
pre : " <b> 1.1 </b> "
---

Trong phần này chúng ta sẽ cùng tìm hiểu AWS Transit Gateway là gì và tại sao cần sử dụng nó.

#### Amazon VPC và VPC Peering
Amazon Virtual Private Cloud (VPC) là một mạng ảo nằm cô lập trong AWS Cloud. Amazon VPC rất giống với mạng truyền thống, 
cho phép người dùng triển khai các máy chủ, cơ sở dữ liệu và các tài nguyên máy tính khác. 

Trên thực tế, có rất nhiều trường hợp chúng ta cần kết nối hai hoặc nhiều VPC với nhau để chia sẻ tài nguyên, dữ liệu 
giữa các mạng này. Cách đơn giản nhất để kết nối các mạng này là đi qua đường truyền Internet, tuy nhiên cách này 
tiềm ẩn rất nhiều rủi ro về bảo mật, không đáng tin cậy và không đảm bảo hiệu suất.

**VPC Peering** là tính năng cho phép kết nối hai hoặc nhiều Amazon VPC với nhau thông qua hệ thống mạng riêng của AWS, 
không sử dụng đường truyền Internet. Khi sử dụng VPC Peering để kết nối 2 VPC, các tài nguyên trong một mạng có thể 
kết nối với các tài nguyên trong mạng còn lại thông qua địa chỉ IP riêng tư như thể chúng nằm trong cùng một mạng.
<!-- TODO: VPC Peering diagram -->

Tuy VPC Peering là một tính năng hiệu quả để kết nối các VPC với nhau nhưng nó cũng có một số nhược điểm. Một trong số đó
phải kể đến là khi phải số lượng kết nối lớn khi cần kết nối nhiều VPC. VPC Peering có đặc điểm là không có tính chất bắc cầu,
tức là trong trường hợp VPC A đã peering với VPC B, VPC B đã peering với VPC C thì VPC A không kết nối được đến VPC C. 
Ngoài ra một Amazon VPC không thể peering với quá 125 VPC khác. Những đặc điểm này khiến cho việc quản lý, 
mở rộng khi có nhu cầu kết nối nhiều VPC trở nên khó khăn. AWS Transit Gateway là giải pháp cho vấn đề này.
<!-- TODO: VPC Peering overwhelm diagram -->

#### AWS Transit Gateway
**AWS Transit Gateway** là một tính năng mạnh mẽ của VPC cho phép kết nối nhiều VPC, mạng on-premises với một cổng duy nhất. AWS Transit Gateway 
giúp đơn giản hoá cấu hình mạng bằng cách loại bỏ các kết nối ngang hàng giữa các VPC với nhau, các VPC sẽ kết nối đến một hub trung tâm duy nhất, 
đóng vai trò điều hướng lưu lượng truy cập mạng. So với VPC Peering, tính năng này có hiệu suất tốt hơn, dễ dàng quản lý, mở rộng và khắc phục sự cố hơn vì vậy nó được sử dụng rất phổ biến trong các hệ thống lớn có cấu hình mạng phức tạp.

<!-- TODO: AWS Transit gateway, from VPC Peering to Transit gateway diagram -->
Khi sử dụng AWS Transit Gateway để kết nối các VPC với nhau, các VPC này có thể:
- Cùng tài khoản, cùng region
- Cùng tài khoản, khác region
- Khác tài khoản, cùng region
- Khác tài khoản, khác region

Hãy cùng trang bị những kiến thức cần thiết trong các phầnt tiếp theo trước khi khám phá từng trường hợp cụ thể nhé.
