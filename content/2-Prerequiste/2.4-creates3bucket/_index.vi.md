---
title: 'Tạo S3 Bucket'
date: '`r Sys.Date()`'
weight: 4
chapter: false
pre: ' <b> 2.4 </b> '
---

**Amazon Simple Storage Service (Amazon S3)** là một object storage service cung cấp khả năng mở rộng hàng đầu, tính khả dụng của dữ liệu, bảo mật và hiệu năng cao. Amazon S3 cung cấp các tính năng quản lý để bạn có thể tối ưu hóa, tổ chức và cấu hình quyền truy cập vào dữ liệu của mình để đáp ứng các yêu cầu cụ thể về kinh doanh, tổ chức và tuân thủ của bạn.

Trong bước này, chúng ta sẽ tạo ra một S3 bucket để lưu trữ file export dữ liệu từ DynamoDB

1. Tạo **S3 Bucket**

- Truy cập vào [Giao diện quản trị S3](https://ap-southeast-1.console.aws.amazon.com/s3/home?region=ap-southeast-1)
- Chọn **Bucket**
- Chọn **Create role**

![S3 Console](/images/2.prerequisite/021-creates3bucket.png)

- Trong bước **Create bucket**
- Nhập `dynamodb-export-data-lab`
- Chọn `ACLs disabled`
- Kéo xuống và click **Create bucket**

![Create Bucket](/images/2.prerequisite/022-creates3bucket.png)
