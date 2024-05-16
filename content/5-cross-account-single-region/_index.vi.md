---
title : "Khác tài khoản - Cùng Region"
date : "`r Sys.Date()`"
weight : 5
chapter : false
pre : " <b> 5. </b> "
---
Trong phần này, chúng ta sẽ cùng cấu hình AWS Transit Gateway kết nối các VPC nằm trong các tài khoản khác nhau nhưng 
cùng một region.

**Tình huống**: Công ty bạn ngày càng có nhiều khách hàng lớn, việc bảo mật hệ thống dần trở nên bắt buộc. Tuy nhiên, công
ty bạn không có chuyên gia về bảo mật nào. Vì vậy, ban giám đốc đã quyết định hợp tác với một công ty chuyên về lĩnh vực
bảo mật để kiểm thử và tìm ra các lỗ hổng bảo mật của hệ thống. Công ty đối tác đã có nhiều năm hoạt động trong lĩnh vực bảo mật, 
họ biết rằng nếu hệ thống tồn tại lỗ hổng bảo mật thì nó phải được phát hiện càng sớm càng tốt nên muốn kiểm thử trên môi
trường Dev của hệ thống. Công ty đối tác đã cài sẵn các máy chủ và công cụ cần thiết trên một VPC tên là Partner 
trong region mà công ty bạn đang sử dụng.

Công việc của bạn là kết nối Partner VPC với Dev VPC để các chuyên gia bảo mật của công ty đối tác có thể kiểm thử trong
giai đoạn đầu của quá trình phát triển. Vì công ty bạn có thể hợp tác với nhiều đối tác trong tương lai nên để dễ dàng quản lý, 
cấu hình và xử lý sự cố mạng, bạn được yêu cầu sử dụng Transit Gateway để cấu hình và không được sử dụng VPC Peering.
Bạn hoàn toàn có thể sử dụng cách cấu hình peering hai transit gateway như phần trước. Tuy nhiên, công ty đối tác đã hợp 
tác với nhiều khách hàng nên cấu hình mạng của họ đã trở nên rất phức tạp, họ sẽ rất vui nếu như không cần quản lý thêm 
cấu hình mạng khi kết nối với mạng của công ty bạn. Trong trường hợp này, bạn có thể sử dụng một dịch vụ của AWS là
**AWS Resource Access Manager (AWS RAM)**. Hãy lưu ý rằng bạn chỉ có thể sử dụng dịch vụ này để kết nối các VPC nằm trong 
cùng một region. Khi đó, cấu hình mạng của bạn sẽ trông như sau:
![Diagram](/images/5-cross-account-single-region/cross_account_single_region.svg)

#### Nội dung

1. [Chuẩn bị](5.1-preparation/)
2. [Cấu hình định tuyến](5.2-configure-route-tables/)