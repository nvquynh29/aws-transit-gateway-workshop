---
title : "AWS Transit Gateway Routing"
date : "`r Sys.Date()`"
weight : 2
chapter : false
pre : " <b> 1.2 </b> "
---

In this section, we will learn about the routing mechanism of AWS Transit Gateway.

#### AWS Transit Gateway Attachment
**AWS Transit Gateway Attachment** là một kết nối logic giữa một tài nguyên và AWS Transit Gateway. Tài nguyên này có thể
là VPC, kết nối VPN, cổng Direct Connect hoặc các loại tài nguyên khác được hỗ trợ. Khi một Transit Gateway attachment mới 
được tạo, nó sẽ được liên kết với bảng định tuyến mặc định của Transit Gateway đó.
Bạn có thể thay đổi hành vi này trong phần cấu hình của Transit Gateway.

Mỗi AWS transit gateway attachment chỉ có thể liên kết với một bảng định tuyến nên nếu muốn liên kết attachment đó với
bảng định tuyến mới thì chúng ta cần phải xoá liên kết đó đi trước. Khi bạn làm việc với AWS Transit Gateway mà gặp lỗi
không thể liên kết một attachment với một bảng định tuyến, hãy kiểm tra xem attachment đó đã được liên kết với bảng 
định tuyến nào khác chưa.

AWS Transit Gateway attachment có các loại:
- **VPC**: Kết nối một VPC với Transit Gateway
- **VPN**: Kết nối một VPC connection với Transit Gateway
- **Peering Connection**: Kết nối 2 Transit Gateway với nhau
- **Connect**: Kết nối SD-WAN appliance hoặc on-premises network với Transit Gateway

Trong workshop này chúng ta sẽ sử dụng 2 loại attachment đó là **VPC** và **Peering Connection**.

Dưới đây là hình minh hoạ các tài nguyên có thể kết nối với Transit Gateway thông qua attachment.
![AWS Transit Gateway Attachment](/images/1-introduction/aws_transit_gateway.svg)

#### Route table
**AWS Transit Gateway route table** là một thực thể logic chứa tập hợp các tuyến đường (route) xác định cách chuyển tiếp
lưu lượng giữa các tài nguyên được kết nối với Transit Gateway. Khi tạo một AWS Transit Gateway mới, một bảng định tuyến
mặc định sẽ được tự động tạo ra. Nếu như chúng ta tắt tính năng route table association và route propagation thì AWS sẽ 
không tạo ra bảng định tuyến mặc định này. Bạn có thể tự tạo thêm các bảng định tuyến cho AWS Transit Gateway.

Mỗi attachment chỉ có thể liên kết với một bảng định tuyến, propagate routes đến một hoặc nhiều bảng định tuyến.

This workshop will focus on two types of route tables: **Transit Gateway route table** và **VPC route table**.
- **Transit Gateway route table**: Manages traffic routing between VPCs, on-premises networks, and attachments 
connected to Transit Gateway.
- **VPC route table**: Manages traffic routing within the scope of a single VPC.

Trong workshop này chúng ta sẽ làm việc với 2 loại bảng định tuyến đó là **Transit Gateway route table** và **VPC route table**.
- **Transit Gateway route table**: Quản lý việc điều hướng lưu lượng giữa các VPC, on-premises networks, và các attachment
được kết nối tới Transit Gateway.
- **VPC route table**: Quản lý việc điều hướng lưu lượng trong phạm vi một VPC.

#### Route table association
Transit Gateway route table association thiết lập một cơ chế cho phép tựng động quảng bá (propagating) các tuyến đường
của một bảng định tuyến đến các attachment đã liên kết (associated), đảm bảo rằng lưu lượng truy cập tìm thấy đích đến
mà không cần sự can thiêp thủ công.

Một transit gateway attachment có thể liên kết với một bảng định tuyến. Một bảng định tuyến có thể liên kết với nhiều
attachment.

Khi một attachment được liên kết với một bảng định tuyến, các route trong bảng định tuyến đó sẽ tự động được propagate
tới attachment này. Điều này có nghĩa là các route trong bảng định tuyến sẽ được sử dụng để điều hướng lưu lượng đến
và đi tới các tài nguyên kết nối với attachment đó.

**Ví dụ**: Kết nối VPC và mạng on-premises 
Giả sử bạn có VPC A đã kết nối đến Transit Gateway bằng VPC A attachment.
Bạn tạo một custom Transit Gateway route table và liên kết với VPC A attachment. Trong bảng định tuyến này, bạn thêm một
route đến on-premises network (VD: 10.2.0.0/16). Vì VPC A attachment đã liên kết với bảng định tuyến
này nên route đến on-premises network tự động được propagate đến nó. Điều này cho phép các tài nguyên trong VPC A 
kết nối được đến on-premises network thông qua Transit Gateway mặc dù không cấu hình định tuyến từ VPC đến on-premises network.
![Route table association](/images/1-introduction/route_table_association.svg)

Bạn cũng có thể cấu hình **Transit Gateway policy table** để kiểm soát và lọc các route được propagate
đến một attachment cụ thể. Tuy nhiên để sử dụng tính năng này, bạn cần phải có hiểu biết nhất định về AWS Transit Gateway, 
chúng ta sẽ không thực hành nó trong workshop này.

#### Route propagation
Trong phạm vi AWS Transit Gateway, route propagation đóng vai trò tự động phân phối các tuyến đường từ bảng định tuyến 
đến các attachment liên quan. Quá trình này hợp lý hóa việc quản lý mạng bằng cách loại bỏ nhu cầu để cấu hình tuyến 
đường thủ công cho mọi attachment.

Mỗi attachment (VD: VPC, VPN, Direct Connect gateway) đều có sẵn một tập hợp các tuyến đường (route) riêng. Khi một attachment
được propagate tới một bảng định tuyến, tất cả các route của attachment sẽ được thêm vào bảng định tuyến này.

VD: Giả sử bạn có VPC A, VPC B đã kết nối đến Transit Gateway bằng VPC A attachment và VPC B attachment. 
Bạn tạo một custom Transit Gateway route table và liên kết với VPC A attachment. Sau đó bạn tạo propagation giữa
VPC B attachment và route table này. Khi đó route sau sẽ tự động được thêm vào route table:

| Destination  | Target  |  Route type |
|---|---|---|
| VPC B CIDR | Attachment ID for VPC B | Propagated |

Vì VPC A attachment đã được liên kết với route table này nên route mới được thêm vào sẽ được propagate đến nó. Khi đó
VPC A có thể kết nối được đến VPC B thông qua Transit Gateway.
![Route propagation](/images/1-introduction/route_propagation.svg)

#### Route table association vs. Route propagation
Điểm khác biệt chính giữa association và propagation là route table association xác định bảng định tuyến nào được sử dụng cho một
attachment cụ thể còn route propagation đảm bảo các tuyến đường (route) trong bảng định tuyến đó được áp dụng cho attachment,
cho phép tạo kết nối giữa các tài nguyên.

Hãy tưởng tượng Transit Gateway giống như một thư viện có các giá sách khác nhau, mỗi giá sách là một route table, mỗi
quyển sách là một route. Route table association giống như cấp cho ai đó một thẻ thư viện (quyền xem các quyển sách). 
Route propagation giống như việc cho phép họ mượn sách (quyền sử dụng các quyển sách).

Hãy cùng tìm hiểu ví dụ dưới đây để hiểu rõ hơn về sự khác biệt này.

Giả sử chúng ta cần phát triển một hệ thống thương mại điện tử sử dụng kiến trúc microservice bao gồm 4 dịch vụ 
Product, Order, Analyzer và Visualizer. Trong đó vai trò của các dịch vụ như sau:
- Product: Xử lý các yêu cầu liên quan đến sản phẩm
- Order: Xử lý các yêu cầu liên quan đến việc mua hàng
- Analyzer: Thống kê số lượng sản phẩm, số lượng đơn hàng, doanh thu...
- Visualizer: Sử dụng dữ liệu từ dịch vụ Analyzer để tạo ra các bảng, biểu đồ

Mỗi dịch vụ đã được triển khai trên VPC tương ứng là Product VPC, Order VPC, Analyzer VPC, Visualizer VPC. Tất cả các VPC đều
đã được kết nối đến cùng một AWS Transit Gateway bằng các attachment Product Attachment, Order Attachment,
Analyzer Attachment, Visualizer Attachment. Để các VPC này kết nối được với nhau, chúng ta cần cấu hình định tuyến.

Vì chúng ta cần release sản phẩm đến tay người dùng sớm nhất có thể cho nên chúng ta ưu tiên release hai dịch vụ Product và
Order trước, sau đó tiếp tục phát triển hai dịch vụ Analyzer và Visualizer. Vì dịch vụ Analyzer vẫn đang phát triển nên chưa
thể sử dụng thực tế được, do đó chúng ta chưa cần kết nối dịch vụ này với hai dịch vụ đã release. Vì vậy, chúng ta sẽ có 
hai môi trường là *production* gồm hai dịch vụ Product, Order và *non-production* gồm hai dịch vụ Analyzer và Visualizer.

Khi đó, chúng ta cần cấu hình mạng như sau:
- Dịch vụ Product và Order có thể kết nối với nhau
- Dịch vụ Analyzer và Visualizer có thể kết nối với nhau
- Các dịch vụ Analyzer, Visualizer không thể kết nối đến các dịch vụ Product và Order

Trong trường hợp này, chúng ta không nên sử dụng một bảng định tuyến duy nhất liên kết với tất cả các attachment vì những
vấn đề sau:
- Security Isolation: Nếu như liên kết tất cả các attachment với cùng một bảng định tuyến thì không thể tách biệt cấu hình
bảo mật giữa môi trường *production* và *non-production*. Trong nhiều trường hợp cấu hình giữa hai môi trường này khác nhau,
nên cách cấu hình này không đủ linh hoạt.

- Unnecessary Propagation: Propagate routes từ các VPC của môi trường *production* tới các VPC của môi trường *non-production*
là không cần thiết vì các VPC này không cần kết nối đến nhau. Việc cấu hình làm tăng sự phức tạp và có thể làm chậm
quá trình định tuyến giữa các mạng với nhau.

Để khắc phục được những vấn đề trên, chúng ta có thể tạo hai bảng định tuyến tuỳ chỉnh:
- *ProductionRTB*: liên kết với Product Attachment và Order Attachment
- *NonProductionRTB*: liên kết với Analyzer Attachment và Visualizer Attachment

Bằng cách này, chúng ta đã chỉ định bảng định tuyến cho các attachment và tách biệt cấu hình giữa môi trường *production*
và *non-production*. Đây cũng chính là kỹ thuật sử dụng nhiều bảng định tuyến được sử dụng trong các phần tiếp theo.

Giả sử sau khi phát triển xong, chúng ta muốn kết nối dịch vụ Analyzer đến các dịch vụ Product, Order để thống kê và 
làm báo cáo doanh thu. Vì dịch vụ Visualizer chỉ cần lấy dữ liệu từ dịch vụ Analyzer nên, chúng ta chỉ cần kết nối 
Analyzer VPC với Product VPC và Order VPC là đủ. Chúng ta có thể làm điều này một cách dễ dàng bằng việc tạo propagation 
giữa bảng định tuyến ProductionRTB và Analyzer Attachment. Khi đó, một route mới cho phép định tuyến lưu lượng đến 
Analyzer Attachment sẽ tự động được tạo và propagate đến các attachment Product và Order.
![Association vs Propagation](/images/1-introduction/association_vs_propagation.svg)

#### Routes for peering attachments
Bạn có thể kết nối hai Transit Gateway với nhau bằng cách peering giống như việc kết nối hai VPC bằng VPC Peering.
Để làm được điều đó, bạn cần tạo một peering attachment trong một transit gateway và chỉ định transit gateway còn lại để
thiết lập kết nối. Vì peering attachment không hỗ trợ tự động propagate routes cho nên bạn cần phải tạo một tuyến đường tĩnh
để định tuyến lưu lượng. Lưu lượng sau khi được định tuyến đến peered transit gateway có thể tiếp tục được định tuyến
tới các attachment kết nối với transit gateway đó.
![Transit Gateway peering](/images/1-introduction/transit_gateway_peering.svg)