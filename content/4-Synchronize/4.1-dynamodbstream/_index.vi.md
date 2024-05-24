---
title: 'Đồng bộ dữ liệu với DynamoDB Stream'
date: '`r Sys.Date()`'
weight: 1
chapter: false
pre: ' <b> 4.1 </b> '
---

**A DynamoDB Stream** là một chuỗi sự kiện được sắp xếp theo thời gian ghi lại tất cả các sửa đổi cho các DynamoDB table near real-time. Tương tự như việc thu thập dữ liệu thay đổi, DynamoDB Streams bao gồm nhiều sự kiện Insert, Update và Delete. Mỗi bản ghi có một số thứ tự duy nhất được sử dụng để sắp xếp.

Tổng quan kiến trúc sau khi hoàn thành bước này sẽ như sau:

![Architecture Overview](/images/4.synchronize/dynamodbsynchronize.png)

1. Create **Lambda function**

- Truy cập vào [Giao diện quản lý Lambda](https://ap-southeast-1.console.aws.amazon.com/lambda/home?region=ap-southeast-1#/discover)
- Chọn **Functions**
- Click **Create function**

![Create Lambda Function](/images/4.synchronize/001-createlambdafunction.png)

- Trong bước **Create function**
- Chọn **Author from scratch**
- Tại **Runtime**, chọn **Python 3.12** (phiên bản mới nhất)
- Nhập `sync-data-with-dynamodb-stream`

![Create Lambda Function](/images/4.synchronize/002-createlambdafunction.png)

- Tại **Change default execution role**, chọn **Use an existing role**
- Chọn **lambda-lab-role**

{{% notice info %}}
**lambda-lab-role** là một IAM Role mà chúng ta đã tạo ở những bước trước
{{% /notice %}}

- Click **Create function**

![Create Lambda Function](/images/4.synchronize/003-createlambdafunction.png)

- [Tải Lambda function](/dynamodb_stream.zip)

- Click **Upload** và chọn file mà bạn vừa tải, sau đó thực hiện upload nó

{{% notice info %}}
Trong DynamoDB stream, event sẽ có 3 loại: INSERT, REMOVE and MODIFY
{{% /notice %}}

2. Bật **Dynamodb Stream**

Để đồng bộ dữ liệu với **DynamoDB Stream**, chúng ta cần bật **DynamoDB Stream** trong DynamoDB table

- Truy cập vào [Giao diện quản trị DynamoDB](https://ap-southeast-1.console.aws.amazon.com/dynamodbv2/home?region=ap-southeast-1#dashboard)
- Chọn **Tables**
- Click **lab-table**

![Access DynamoDB](/images/4.synchronize/001-accessdynamodb.png)

- Trong **lab-table**
- Chọn **Exports and Streams**
- Tại **DynamoDB stream details**, click **Turn on**

![Turn on Stream](/images/4.synchronize/002-turnondynamodbstream.png)

- Trong **Turn on DynamoDB stream**
- Chọn **New image**
- Click **Turn on stream**

![Turn on Stream](/images/4.synchronize/003-turnondynamodbstream.png)

Bây giờ, sau khi bật stream, chúng ta cần tạo một Lambda function để nhận record được gửi đi từ DynamoDB stream, nhưng trước khi làm việc đó, chúng ta cần cập nhật **lambda-lap-role** để cho phép lambda nhận record từ DynamoDB stream.

- Truy cập vào [Giao diện quản trị IAM](https://us-east-1.console.aws.amazon.com/iam/home?region=ap-southeast-1#/home)
- Chọn **Roles**
- Tìm và chọn **006-editlambdarole.png**

![AWS IAM Console](/images/4.synchronize/006-editlambdarole.png)

- Trong **lambda-lap-role**
- Click **Add permissions**
- Click **Create inline policy**

![Attack Policy](/images/4.synchronize/008-createpolicy.png)

- Trong bước **Specify permissions**
- Click **Json**
- Sao chép và dán đoạn cấu hình dưới

```bash
{
	"Version": "2012-10-17",
	"Statement": [
		{
			"Sid": "APIAccessForDynamoDBStreams",
			"Effect": "Allow",
			"Action": [
				"dynamodb:GetRecords",
				"dynamodb:GetShardIterator",
				"dynamodb:DescribeStream",
				"dynamodb:ListStreams"
			],
			"Resource": "arn:aws:dynamodb:<aws-region>:<aws-account-id>:table/<dynamodb-table-name>/stream/*"
		}
	]
}
```

- Kéo xuống và click **Next**

![Create inline policy](/images/4.synchronize/008-createpolicy.png)

- Trong **Review and create**
- Nhập `dynamodb-stream-data`
- Click **Create policy**

![Create inline policy](/images/4.synchronize/009-createpolicy.png)

- Sau khi tạo `dynamodb-stream-data`, chúng ta cần tạo `aoss-policy` với nội dung như bến dưới

```bash
{
	"Version": "2012-10-17",
	"Statement": [
		{
			"Effect": "Allow",
			"Action": [
				"aoss:APIAccessAll",
				"aoss:DeleteCollection",
				"aoss:*"
			],
			"Resource": "*"
		},
		{
			"Sid": "VisualEditor1",
			"Effect": "Allow",
			"Action": "aoss:DashboardsAccessAll",
			"Resource": "*"
		}
	]
}
```

- Sau khi cập nhật IAM Role, chúng ta sẽ quay trở lại [AWS DynamoDB Console](https://ap-southeast-1.console.aws.amazon.com/dynamodbv2/home?region=ap-southeast-1#dashboard)
- Chọn **Exports and Streams**
- Kéo xuống và chọn **Create trigger**

![Turn on Stream](/images/4.synchronize/004-createtrigger.png)

- Trong bước **Create a trigger**
- Chọn **sync-data-with-dynamodb-stream**
- Click **Create trigger**

![Turn on Stream](/images/4.synchronize/005-createtrigger.png)

Trước khi thực hiện kiểm tra, chúng ta cần tạo một vài biến môi trường như những bước trước - khi chúng ta tạo Replicate Lambda function.

![Turn on Stream](/images/4.synchronize/006-createenv.png)

Để kiểm tra DynamoDB Streams pipeline, chúng ta truy cập vào [AWS API Gateway Console](https://ap-southeast-1.console.aws.amazon.com/apigateway/main/apis?api=l9l2p0i2h4&region=ap-southeast-1)

- Chọn **APIs**
- Chọn **dynamodb-api-gw**

![API Gateway Console](/images/4.synchronize/007-apigatewayconsole.png)

- Trong **dynamodb-api-gw**
- Chọn **POST**
- Chọn **Test**
- Tại **Request body**, sao chép và dán

```bash
{
    "name": "Test DynamoDB Stream Pipeline",
    "type": "Test"
}
```

- Kéo xuống và click **Test**

![Test API Gateway Create Object](/images/4.synchronize/008-testcreatedynamodbdata.png)

Chúng tôi có thể xác minh việc tạo dữ liệu thành công. Bằng cách truy cập vào [Giao diện quản trị Cloud Watch](https://ap-southeast-1.console.aws.amazon.com/cloudwatch/home?region=ap-southeast-1#home:) để xem log.

**CloudWatch** cho phép bạn giám sát toàn bộ ngăn xếp của mình (ứng dụng, cơ sở hạ tầng, mạng và dịch vụ) và sử dụng cảnh báo, nhật ký và dữ liệu sự kiện để thực hiện các hành động tự động và giảm thời gian giải quyết trung bình (MTTR). Điều này giải phóng các nguồn lực quan trọng và cho phép bạn tập trung vào việc xây dựng ứng dụng và giá trị kinh doanh.

Bạn có thể thấy như bên dưới:

![Cloud Watch Log](/images/4.synchronize/009-cloudwatchlog.png)

{{% notice info %}}
Bạn cũng có thể xem dữ liệu trong DynamoDB table hoặc OpenSearch Dashboard, bạn cũng có thể thực hiện các thao tác xóa hoặc chỉnh sửa để tìm hiểu thêm về các thao tác trong DynamoDB
{{% /notice %}}
