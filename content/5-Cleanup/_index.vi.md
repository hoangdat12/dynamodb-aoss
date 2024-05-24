+++
title = "Dọn dẹp tài nguyên"
date = 2021
weight = 5
chapter = false
pre = "<b>5. </b>"
+++

Chúng ta sẽ làm theo những bước bên dưới để dọn dẹp tài nguyên mà chúng ta đã tạo trong bài lap này.

1. Truy cập vào [Giao diện quản trị OpenSearch Serverless](https://ap-southeast-1.console.aws.amazon.com/aos/home?region=ap-southeast-1#opensearch/dashboard)

- Chọn **Collections**.
- Chọn **lab-collection**
- Click **Delete** và nhập `confirm`

![Clean OpenSearch](/images/5.cleanup/001-deleteopensearch.png)

2. Truy cập vào [Giao diện quản trị Kinesis](https://ap-southeast-1.console.aws.amazon.com/kinesis/home?region=ap-southeast-1#/dashboard)

- Chọn **Data streams**.
- Chọn **kinesis-stream-data-lab**
- Click **Actions** và chọn **Delete**, sau đó nhập `delete`

![Clean Kinesis](/images/5.cleanup/002-deletekinesis.png)

3. Truy cập vào [Giao diện quản trị DynamoDB](https://ap-southeast-1.console.aws.amazon.com/dynamodbv2/home?region=ap-southeast-1#dashboard)

- Chọn **Tables**
- Click **lab-table**
- Chọn **Actions** và click **Delete table**, sau đó nhập `confirm`

![Clean DynamoDB](/images/5.cleanup/003-deletedynamodb.png)

Bạn cũng làm tương tự cho Lambda function, IAM Role và API Gateway để dọn dẹp những tài nguyên còn lại
