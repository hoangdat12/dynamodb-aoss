---
title: 'Các bước chuẩn bị'
date: '`r Sys.Date()`'
weight: 2
chapter: false
pre: ' <b> 2. </b> '
---

### Tổng quan

- Trong bước này, chúng ta sẽ thực hiện tạo IAM Role
- Tạo một DynamoDB table
- Tạo một API Gateway như một Proxy cho DynamoDB table
- Tạo một S3 Bucket để lưu file export từ DynamoDB
- Tạo một OpenSearch Serverless Collection

### Nội dung

- [Tạo Dynamodb table](2.1-createdynamodb/)
- [Tạo API Gateway](2.2-createapigateway/)
- [Tạo OpenSearch Serverless Collection](2.3-createopensearchserverless/)
- [Tạo S3 bucket](2.4-creates3bucket/)
- [Chuẩn bị IAM Role](2.5-createiamrole/)
