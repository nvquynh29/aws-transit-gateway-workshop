---
title : "Cùng tài khoản - Cùng Region"
date : "`r Sys.Date()`"
weight : 3
chapter : false
pre : " <b> 3. </b> "
---

Trong phần này, chúng ta sẽ cùng cấu hình AWS Transit Gateway kết nối các VPC trong cùng một tài khoản AWS và cùng một region.

**Tình huống**: Bạn là DevOps Engineer của công ty khởi nghiệp X. Công ty của bạn đang phát triển một hệ thống chuẩn bị triển khai trên
AWS. Công ty bạn mới thành lập nên không có quá nhiều vốn, vì vậy bạn được giao nhiệm vụ triển khai hệ thống lên hai môi trường
khác nhau là Dev và Test với mức chi phí hợp lý. Vì hai môi trường này chỉ dùng để phục vụ cho quá trình phát triển nên
bạn có thể dùng chung các tài nguyên giữa hai môi trường.

Với những yêu cầu trên, bạn quyết định sẽ triển khai 3 VPCs là Dev, Test và Share trong cùng một region. Trong đó Share VPC 
chứa các tài nguyên có thể dùng trong cả hai môi trường Dev và Test như database, cache. Khi đó, bạn cần phải cấu hình
để Dev VPC và Test VPC kết nối được đến Share VPC.

Với những trường hợp cần kết nối ít VPC như vậy, sử dụng VPC Peering có lẽ là lựa chọn tốt hơn. Trong workshop này chúng ta
sẽ cùng tìm hiểu một cách cấu hình khác đó là sử dụng AWS Transit Gateway. Để cho việc triển khai đơn giản hơn, chúng ta
sẽ chỉ triển khai một máy ảo EC2 trong mỗi VPC để kiểm tra kết nối mạng. Trong phần này, chúng ta sẽ cùng cấu hình
mạng như sau:
![Diagram](/images/3-single-account-single-region/single_account_single_region.svg)

#### Nội dung

1. [Chuẩn bị](3.1-preparation)
2. [Tạo transit gateway attachments](3.2-create-attachments/)
3. [Một bảng định tuyến](3.3-single-route-table/)
4. [Nhiều bảng định tuyến](3.4-multiple-route-tables/)
