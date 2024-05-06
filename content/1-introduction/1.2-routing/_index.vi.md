---
title : "AWS Transit Gateway Routing"
date : "`r Sys.Date()`"
weight : 2
chapter : false
pre : " <b> 1.2 </b> "
---

Trong phần này chúng ta cùng tìm hiểu về cơ chế định tuyến của AWS Transit Gateway.

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
<!-- TODO: hình minh hoạ transit gateway với nhiều kết nối như VPC, direct connect, VPN, Onpremises... -->


#### Route table
**AWS Transit Gateway route table** là một thực thể logic chứa tập hợp các tuyến đường (route) xác định cách chuyển tiếp
lưu lượng giữa các tài nguyên được kết nối với Transit Gateway. Khi tạo một AWS Transit Gateway mới, một bảng định tuyến
mặc định sẽ được tự động tạo ra. Nếu như chúng ta tắt tính năng route table association và route propagation thì AWS sẽ 
không tạo ra bảng định tuyến mặc định này.

Mỗi attachment chỉ có thể liên kết với một bảng định tuyến, propagate routes đến một hoặc nhiều bảng định tuyến.
<!-- TODO: Thêm hình nếu có thể, VD về bảng định tuyến xem trông thế nào -->

Trong workshop này chúng ta sẽ làm việc với 2 loại bảng định tuyến đó là **Transit Gateway route table** và **VPC route table**.
**Transit Gateway route table** được dùng để quản lý việc điều hướng lưu lượng giữa các VPC, on-premises networks, và các attachment
được kết nối tới Transit Gateway. **VPC route table** được dùng để quản lý việc điều hướng lưu lượng trong phạm vi một VPC.

#### Route table association
Một transit gateway attachment có thể liên kết với một bảng định tuyến. Một bảng định tuyến có thể liên kết với nhiều
attachment.

Khi một attachment được liên kết với một bảng định tuyến, các route trong bảng định tuyến đó sẽ tự động được propagate
tới attachment này. Điều này có nghĩa là các route trong bảng định tuyến sẽ được sử dụng để điều hướng lưu lượng đến
và đi tới các tài nguyên kết nối với attachment đó.

**Ví dụ:** Giả sử bạn có VPC A đã kết nối đến Transit Gateway bằng VPC A attachment.
Bạn tạo một custom Transit Gateway route table và liên kết với VPC A attachment. Trong bảng định tuyến này, bạn thêm một
route đến on-premises network (VD: 10.2.0.0/16). Vì VPC A attachment đã liên kết với bảng định tuyến
này nên route đến on-premises network tự động được propagate đến nó. Điều này cho phép các tài nguyên trong VPC A 
kết nối được đến on-premises network thông qua Transit Gateway mặc dù không cấu hình định tuyến từ VPC đến on-premises network.
<!-- TODO: Thêm hình mô tả propagation, route table, onpremise network -->

Bạn cũng có thể cấu hình **Transit Gateway policy table** để kiểm soát và lọc các route được propagate
đến một attachment cụ thể. Tuy nhiên để sử dụng tính năng này, bạn cần phải có hiểu biết nhất định về AWS Transit Gateway, 
chúng ta sẽ không thực hành nó trong workshop này.

#### Route propagation
Mỗi attachment (VD: VPC, VPN, Direct Connect Gateway) đều có sẵn một tập hợp các tuyến đường (route) riêng. Khi một attachment
được propagate tới một bảng định tuyến, tất cả các route của attachment sẽ được thêm vào bảng định tuyến này.

VD: Giả sử bạn có VPC A, VPC B đã kết nối đến Transit Gateway bằng VPC A attachment và VPC B attachment. 
Bạn tạo một custom Transit Gateway route table và liên kết với VPC A attachment. Sau đó bạn tạo propagation giữa
VPC B attachment và route table này. Khi đó route sau sẽ tự động được thêm vào route table:
<!-- TODO: Format giống AWS: https://docs.aws.amazon.com/vpc/latest/tgw/transit-gateway-peering-scenario.html -->
VPC B Cidr -> VPC B attachment

Vì VPC A attachment đã được liên kết với route table này nên route mới được thêm vào sẽ được propagate đến nó. Khi đó
VPC A có thể kết nối được đến VPC B thông qua Transit Gateway.

#### Route table association vs. Route propagation
The key difference is that association determines which route table is used for a specific attachment, while propagation ensures the routes in that table are applied to the attachment, enabling connectivity between the connected resources.
<!-- TODO: VD giống trong vid: https://www.youtube.com/watch?v=AlPK4meZrLA -->
Chứng minh: "while propagation ensures the routes in that table are applied to the attachment"
Có 2 Prod VPC, chỉ cho phép 2 VPC này kết nối với nhau, không cho VPC NonProd kết nối đến => Không được associate chung
vào một table.
Tuy nhiên nếu chỉ muốn NonProd VPC 2 được két nối với các ProdVPC thì phải làm sao? Đó là dùng propagation như VD trên.

Sửa lại ví dụ:
2 Prod VPC là 2 service trong microservice là product và order service chẳng hạn. Còn 2 NonProd VPC là analytics service.
Trong trường hợp cần thông kê data, một service analytics NonProd VPC cần kết nối đến 2 Prod VPC -> Dùng propagation.

Bảng định tuyến ProdVPC chỉ có 2 Prod mà ko có Analytics vì muốn control network traffic trong prod network chứa các service này.

#### Routes for peering attachments
Limitations: Peering attachments between Transit Gateways do not support automatic route propagation. In such cases, you need to manually create static routes in the Transit Gateway route tables to route traffic between the peered Transit Gateways.
