---
title: 'Tạo OpenSearch Serverless'
date: '`r Sys.Date()`'
weight: 3
chapter: false
pre: ' <b> 2.3 </b> '
---

**OpenSearch Serverless** is a serverless option in Amazon OpenSearch Service. As a developer, you can use OpenSearch Serverless to run petabyte-scale workloads without configuring, managing, and scaling OpenSearch clusters. You get the same interactive millisecond response times as OpenSearch Service with the simplicity of a serverless environment.

**OpenSearch Serverless** là một dịch vụ serverless trong Amazon OpenSearch Service. Với tư cách là một lập trình viên, bạn có thể sử dụng OpenSearch Serverless để chạy những công việc petabyte-scale workloads mà không cần cấu hình, quản lý và scale cụm OpenSearch. Bạn cũng nhận được thời gian phản hồi trong milisecond tương tự như OpenSearch Service nhờ sự đơn giản của môi trường serverless.

Trong bước này, chúng ta cần tạo IAM Role cho Lambda function và API Gateway

1. Tạo **Lambda replicate policy**
   - Truy cập vào [Giao diện quản trị OpenSearch](https://ap-southeast-1.console.aws.amazon.com/aos/home?region=ap-southeast-1#opensearch/collections)
   - Chọn **Collections**
   - Chọn **Create collection**

![OpenSearch Console](/images/2.prerequisite/021-createaoss.png)

- trong bước **Configure collection settings**
- Nhập **lab-collection**
- Tại **Collection type**, chọn **Search**

![Create AOSS](/images/2.prerequisite/022-createaoss.png)

- Tại **Security**, chọn **Easy create**
- Kéo xuống và click **Next**

![Create AOSS](/images/2.prerequisite/023-createaoss.png)

- Trong bước **Review and create collection**
- Click **Create collection**
