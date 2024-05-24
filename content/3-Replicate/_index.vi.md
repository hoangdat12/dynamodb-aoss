---
title: 'Sao chép dữ liệu'
date: '`r Sys.Date()`'
weight: 3
chapter: false
pre: ' <b> 3. </b> '
---

### Tổng quan

**Amazon OpenSearch Serverless** là một dịch vụ mạnh mẽ cho phép truy vấn tìm kiếm hiệu quả. Khi tích hợp với DynamoDB, chúng ta cần phải replciate dữ liệu đã tồn tại trong DynamoDB đến OpenSearch.

Trong bước này, chúng ta sẽ tìm hiểu làm cách nào để replicate dữ liệu từ Amazon DynamoDB đến Amazon OpenSearch Serverless.

Tổng quan kiến trúc sau khi bạn hoàn thành các bước này:

![Architecture overview](/images/3.replicate/gatewayarchitecture.png)

### Nội dung

- [Export DynamoDB Data](3.1-exportdynamodbdatatos3/)
- [Replicate Đến OpenSearch Serverless](3.2-handledataandsaveinaoss/)
- [Kiểm tra Data](3.3-checkdata/)
