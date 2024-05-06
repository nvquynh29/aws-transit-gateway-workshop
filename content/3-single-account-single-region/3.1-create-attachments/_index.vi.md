---
title : "Tạo transit gateway attachments"
date : "`r Sys.Date()`"
weight : 1
chapter : false
pre : " <b> 3.1 </b> "
---

Trong bước này, chúng ta sẽ cùng tạo AWS Transit Gateway và các attachment sau đó gắn vào các VPC đã tạo ở bước trước.

1\. Tìm kiếm từ khoá `vpc` rồi chọn service **VPC**
![Create attachments](/images/3-single-account-single-region/create_attachments_1.png)

2\. Tạo AWS Transit Gateway
- Tại giao diện của VPC chọn **Transit gateways**
- Bấm **Create transit gateway**
![Create attachments](/images/3-single-account-single-region/create_attachments_2.png)
- Điền các thông tin như sau rồi bấm **Create transit gateway**
  - Name tag: `tokyo-tgw`
  - Description: `Transit Gateway for region Tokyo`
  ![Create attachments](/images/3-single-account-single-region/create_attachments_3.png)

3\. Sau khi transit gateway có trạng thái **Available** thì tạo các transit gateway attachment
- Tại giao diện của VPC chọn **Transit gateway attachments**
- Bấm **Create transit gateway attachment**
![Create attachments](/images/3-single-account-single-region/create_attachments_4.png)
- Điền các thông tin như sau:
  - Name tag: `dev-att`
  - Transit gateay ID: **tokyo-tgw**
  - Attachment type: **VPC**
  - VPC ID: **dev-vpc**
  ![Create attachments](/images/3-single-account-single-region/create_attachments_5.png)
- Bấm **Create transit gateway attachment**

4\. Lặp lại bước trên để tạo thêm 2 transit gateway attachment nữa:
- `test-att` attachment gắn với **test-vpc** VPC
![Create attachments](/images/3-single-account-single-region/create_attachments_6.png)
- `share-att` attachment gắn với **share-vpc** VPC
![Create attachments](/images/3-single-account-single-region/create_attachments_7.png)

Sau khi tạo xong chúng ta sẽ có các attachment như sau.
![Create attachments](/images/3-single-account-single-region/create_attachments_8.png)