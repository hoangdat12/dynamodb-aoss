---
title: 'Chuẩn bị IAM Role'
date: '`r Sys.Date()`'
weight: 5
chapter: false
pre: ' <b> 2.5 </b> '
---

Trong bước này, chúng ta cần tạo một IAM Role cho Lambda function và API Gateway

1. Tạo **Lambda replicate Role**
   - Truy cập vào [Giao diện quản trị IAM](https://us-east-1.console.aws.amazon.com/iam/home?region=ap-southeast-1#/home)
   - Chọn **Roles**
   - Chọn **Create role**

![IAM Console](/images/2.prerequisite/001-iamconsole.png)

- Trong bước **Select trusted entity**
- Chọn **AWS service**
- Tại **Service or use case**, chọn **Lambda**
- Tại **Use case**, chọn **Lambda**
- Click **Create**

![Create Lambda Replicate Role](/images/2.prerequisite/002-lambdareplicaterole.png)

- Trong bước **Add permissions**
- Tìm và chọn **AWSLambdaExecute**
- Click **Next**

![Create Lambda Replicate Role](/images/2.prerequisite/003-createlambdareplicaterole.png)

- Trong bước **Name, review, and create**
- Nhập `lambda-lab-role`
- Kéo xuống và click **Create role**

![Create Lambda Replicate Role](/images/2.prerequisite/004-createlambdareplicaterole.png)
