---
title : "Xoá tài nguyên"
date : "`r Sys.Date()`"
weight : 7
chapter : false
pre : " <b> 7. </b> "
---
Sau khi thực hành xong workshop này, bạn cần phải xoá các tài nguyên đã tạo nếu không thì sẽ bị tính phí. Hãy xoá cẩn thận và kiểm tra lại sau khi xoá vì Transit Gateway là tài nguyên tương đối đắt, nếu bạn quên không xoá transit gateway thì nó có thể sẽ tốn khá nhiều tiền. 

Hãy xoá các tài nguyên theo thứ tự dưới đây trong cả hai region **Tokyo** và **Singapore**.

Nếu bạn không thực hiện các phần 5 và 6:
- Xoá toàn bộ transit gateway attachment
- Xoá transit gateway
- Xoá CloudFormation Stack
- Xoá các S3 bucket có tên giống với định dạng **cf-templates-*** (Đây là các S3 bucket được AWS tự động tạo ra để lưu trữ các tệp CloudFormation template của bạn)

Nếu bạn thực hiện các phần 5 và 6:
- Tài khoản thứ nhất:
  - Xoá share resource trong dịch vụ AWS RAM
- Tài khoản thứ hai:
  - Xoá toàn bộ transit gateway attachment
  - Xoá transit gateway
  - Xoá CloudFormation Stack
  - Xoá các S3 bucket có tên giống với định dạng **cf-templates-***