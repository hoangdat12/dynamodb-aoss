---
title: 'Synchronize Data with Kinesis Stream'
date: '`r Sys.Date()`'
weight: 2
chapter: false
pre: ' <b> 4.2 </b> '
---

**Amazon Kinesis Data Streams** là một dịch vụ dữ liệu trực tuyến không cần máy chủ giúp đơn giản hóa việc thu thập, xử lý và lưu trữ dữ liệu trực tuyến ở mọi quy mô.

{{% notice info %}}
Dữ liệu được lưu trong một luồng Kinesis thường được lưu dưới dạng dữ liệu nhị phân, chúng ta cần chuyển đổi nó trước khi comsume.
{{% /notice %}}

Để sử dụng Kinesis Stream để đồng bộ dữ liệu từ DynamoDB đến OpenSearch Serverless, chúng ta cần bật Kinesis Stream trong DynamoDB table và tạo một hàm Lambda để xử lý dữ liệu.

Tổng quan kiến trúc sau khi hoàn thành bước này sẽ như sau:

![Architecture Overview](/images/4.synchronize/kinesissynchronize.png)

1. Tạo **Kinesis Stream**

- Truy cập vào [Giao diện quản lý Kinesis](https://ap-southeast-1.console.aws.amazon.com/kinesis/home?region=ap-southeast-1#/dashboard)
- Chọn **Data streams**
- Click **Create data stream**

![Create Kinesis Stream](/images/4.synchronize/010-createkinesisstream.png)

- Trong bước **Create data stream**
- Nhập `kinesis-stream-data-lab`
- Tại **Capacity mode**, chọn **On-demand**
- Kéo xuống và click **Create data stream**

![Create Kinesis Stream](/images/4.synchronize/011-createkinesisstream.png)

2. Bật **Kinesis Stream**

- Truy cập vào [Giao diện quản lý DynamoDB](https://ap-southeast-1.console.aws.amazon.com/dynamodbv2/home?region=ap-southeast-1#dashboard)
- Chọn **Tables**
- Click **lab-table**

![Access DynamoDB](/images/4.synchronize/001-accessdynamodb.png)

- Trong **lab-table**
- Chọn **Exports and Streams**
- Tại **Amazon Kinesis data stream details**, click **Turn on**

![Enable Kinesis Stream](/images/4.synchronize/010-enablekinesisstream.png)

- Trong bước **Enable Kinesis Stream**
- Chọn **kinesis-stream-data-lab**
- Chọn **Microsecond**
- Click **Turn on stream**

![Enable Kinesis Stream](/images/4.synchronize/011-enablekinesisstream.png)

3. tạo Lambda function

Bạn cần tải [file](/kinesis_stream.zip) này, và làm theo những bước như trong phần 4.1 để tạo ra Lambda function với tên `sync-data-with-kinesis-stream`.

Bạn cũng cần phải cập nhật IAM Role để thêm quyền cho phép Lambda function consume dữ liệu từ Kinesis Streams.

```bash
{
	"Version": "2012-10-17",
	"Statement": [
		{
			"Effect": "Allow",
			"Action": [
				"kinesis:GetRecords",
				"kinesis:GetShardIterator",
				"kinesis:DescribeStream",
				"kinesis:DescribeStreamSummary",
				"kinesis:ListShards",
				"kinesis:ListStreams"
			],
			"Resource": "arn:aws:kinesis:<aws-region>:<aws-account-id>:stream/<kinesis-stream-name>"
		}
	]
}
```

Sau khi tạo Lambda function, bạn cần **Add trigger** cho nó

- Truy cập vào **sync-data-with-kinesis-stream** function
- Click **Add trigger**

![Add trigger](/images/4.synchronize/012-addtrigger.png)

- Trong bước **Add trigger**
- Chọn **Kinesis**
- Chọn **kinesis-stream-data-lab**
- Kéo xuống và click **Add**

![Add trigger](/images/4.synchronize/013-addtrigger.png)

Để kiểm tra Kinesis Streams pipeline, truy cập vào [AWS API Gateway Console](https://ap-southeast-1.console.aws.amazon.com/apigateway/main/apis?api=l9l2p0i2h4&region=ap-southeast-1)

- Chọn **APIs**
- Chọn **dynamodb-api-gw**

![API Gateway Console](/images/4.synchronize/007-apigatewayconsole.png)

- Trong **dynamodb-api-gw**
- Chọn **POST**
- Chọn **Test**
- Tại **Request body**, sao chép và dán

```bash
{
    "name": "Test Kinesis Stream Pipeline",
    "type": "Test"
}
```

- Kéo xuống và click **Test**

![Test API Gateway Create Object](/images/4.synchronize/008-testcreatedynamodbdata.png)

Bạn có thể kiểm tra tạo dữ liệu thành công. Bằng cách, Truy cập vào [Giao diện quản trị Cloud Watch](https://ap-southeast-1.console.aws.amazon.com/cloudwatch/home?region=ap-southeast-1#home:) để xem log.

**CloudWatch** cho phép bạn giám sát toàn bộ ngăn xếp của mình (ứng dụng, cơ sở hạ tầng, mạng và dịch vụ) và sử dụng cảnh báo, nhật ký và dữ liệu sự kiện để thực hiện các hành động tự động và giảm thời gian giải quyết trung bình (MTTR). Điều này giải phóng các nguồn lực quan trọng và cho phép bạn tập trung vào việc xây dựng ứng dụng và giá trị kinh doanh.

Bạn có thể thấy như bên dưới:

![Cloud Watch Log](/images/4.synchronize/009-cloudwatchlog2.png)

{{% notice info %}}
Bạn cũng có thể xem dữ liệu trong DynamoDB table hoặc OpenSearch Dashboard, bạn cũng có thể thực hiện các thao tác xóa hoặc chỉnh sửa để tìm hiểu thêm về các thao tác trong DynamoDB
{{% /notice %}}
