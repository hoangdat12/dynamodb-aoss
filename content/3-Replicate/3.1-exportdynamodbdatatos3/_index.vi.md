---
title: 'Export DynamoDB Data'
date: '`r Sys.Date()`'
weight: 1
chapter: false
pre: ' <b> 3.1 </b> '
---

Để replicate data từ DynamoDB đến Amazon OpenSearch Serverless, chúng ta cần phải export data từ DynamoDB đến Amazon S3 bucket.

1. Export DynamoDB data

- Truy cập vào [Giao diện quản trị S3](https://ap-southeast-1.console.aws.amazon.com/dynamodbv2/home?region=ap-southeast-1#dashboard)
- Chọn **Exports to S3**
- Click **Export to S3**

![Export DynamoDB Data](/images/2.prerequisite/023-exportdynamodbdata.png)

Để export data from DynamoDB đến S3, chúng ta cần phải bật **PITR (Point-in-time recovery)**.
**PITR (Point-in-time recovery)** giúp bảo vệ DynamoDB table của bạn khỏi những thao tác ghi hoặc xóa ngẫu nhiên. Với point-in-time recovery, bạn không cần phải lo lắng về việc tạo, duy trì hoặc tạo một lịch để backup dữ liệu.

- Trong bước **Export to S3**
- Tại **Source table**, chọn **lab-table**
- Click **Turn on PITR**
- Nhập S3 destination `s3://dynamodb-export-data-lab`
- Chọn **This AWS account**

![Export DynamoDB Data](/images/2.prerequisite/024-exportdynamodbdata.png)

- Chọn **Full export**
- Chọn **Current time**
- Chọn **DynamoDB JSON**
- Click **Export**

![Export DynamoDB Data](/images/2.prerequisite/025-exportdynamodbdata.png)

Sau khi export thành công, chúng ta có thể click **Destination S3 bucket** để kiểm tra.

![Export DynamoDB Data](/images/2.prerequisite/026-exportdynamodbdata.png)

Chúng ta sẽ thấy file export trong **AWSDynamoDB/** folder.

![Export DynamoDB Data](/images/2.prerequisite/027-exportdynamodbdata.png)
