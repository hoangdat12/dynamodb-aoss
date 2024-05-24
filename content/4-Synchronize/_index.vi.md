---
title: 'Đồng bộ dữ liệu'
date: '`r Sys.Date()`'
weight: 4
chapter: false
pre: ' <b> 4 </b> '
---

### Tổng quan

**Amazon DynamoDB Streams** theo dõi những thay đổi xảy ra bên trong DynamoDB table và lưu những thông tin này tỏng một log file trong vòng 25 giờ. Ứng dụng có thể truy cập những log này và quan sát những dữ liệu trước khi chúng xuất hiện và sau khi được sửa đổi gần như real-time. Bất cứ khi nào một ứng dụng tạo, cập nhật hoặc sửa dữ liệu trong table, DynamoDB Streams sẽ viết một stream record với thuộc tính primary key của dữ liệu mà được sửa đổi.

**Amazon Kinesis** là một bộ dịch vụ cung cấp cách thu thập, xử lý và phân tích dữ liệu thời gian thực một cách dễ dàng. Nó cũng rất tiết kiệm chi phí ở mọi quy mô. Một số trường hợp sử dụng của Amazon Kinesis bao gồm các giải pháp video/audio, luồng click trên trang web và dữ liệu IoT. Trong bài này, chúng ta sẽ sử dụng nó để truyền dữ liệu từ DynamoDB đến OpenSearch Serverless.

{{% notice info %}}
DynamoDB Streams phù hợp nhất để ghi lại các thay đổi cấp độ chi tiết được thực hiện đối với DynamoDB table và xử lý các bản ghi bằng AWS Lambda, trong khi Kinesis Stream tốt hơn cho việc tạo và sử dụng khối lượng lớn dữ liệu cũng như sử dụng Kinesis Analytics, Kinesis Firehose và Lambda để xử lý các bản ghi.
{{% /notice %}}

Trong bước này, chúng ta sẽ đồng bộ dữ liệu từ DynamoDB đến OpenSearch Serverless

Tổng quan kiến trúc sau khi hoàn thành bước này sẽ giống như:

![Basic Architecture](/images/4.synchronize/synchronizearchitecture.png)

### Nội dung

- [Đồng bộ dữ liệu với DynamoDB Stream](4.1-dynamodbstream//)
- [Đồng bộ dữ liệu với Kinesis Stream](4.2-kinesisstream/)
