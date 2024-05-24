---
title: 'Giới thiệu'
date: '`r Sys.Date()`'
weight: 1
chapter: false
pre: ' <b> 1. </b> '
---

**Serverless** là một mô hình phát triển dựa trên điện toán đám mây, cho phép các lập trình viên xây dựng và triển khai ứng dụng mà không cần máy chủ cũng như giảm bớt việc quản lý hạ tầng cho lập trình viên.

**DynamoDB** là một dịch vụ fully managed NoSQL databasecung cấp hiệu suất nhanh chóng và dự đoán được với khả năng mở rộng chính xác. DynamoDB cho phép bạn giảm bớt gánh nặng quản lý khi vận hành và mở rộng một distributed database, để bạn không phải lo lắng về việc cung cấp phần cứng, thiết lập và cấu hình, sao chép dữ liệu, cập nhật phần mềm hoặc mở rộng cụm.

**OpenSearch Serverless** Amazon OpenSearch Serverless là một tùy chọn serverless trong dịch vụ Amazon OpenSearch. Là một nhà phát triển, bạn có thể sử dụng OpenSearch Serverless để chạy công việc quy mô petabyte mà không cần cấu hình, quản lý và mở rộng các cụm OpenSearch. Thời gian phản hồi chỉ trong millisecond giống như OpenSearch Service nhưng với sự đơn giản của môi trường serverless.

**Kinesis Stream** là một dịch vụ serverless streaming data giúp đơn giản hóa việc thu thập, xử lý và lưu trữ các luồng dữ liệu ở mọi quy mô, nó được thiết kế để xử lý các luồng dữ liệu quy mô lớn từ nhiều dịch vụ khác nhau trong thời gian thực.

**Lambda** là một dịch vụ serverless compute, thực thi code của bạn dựa trên các event, xử lý tài nguyên tính toán cho bạn.

Trong bài lab này, bạn sẽ thực hành tích hợp DynamoDB và OpenSearch Serverless. Bạn sẽ học cách để replicate và synchronize dữ liệu một cách hiệu quả giữa các dịch vụ này bằng cơ chế stream như DynamoDB Streams và Kinesis Streams.

![Introduce](/images/1.introduce/001-introduce.png)
