---
title: 'Tạo DynamoDb table'
date: '`r Sys.Date()`'
weight: 1
chapter: false
pre: ' <b> 2.1 </b> '
---

**DynamoDB** DynamoDB là dịch vụ cơ sở dữ liệu NoSQL hoàn toàn được quản lý bởi AWS, cung cấp hiệu suất nhanh chóng và dự đoán được với khả năng mở rộng linh hoạt. DynamoDB giúp bạn giảm bớt gánh nặng quản lý của việc vận hành và mở rộng cơ sở dữ liệu phân tán, giúp bạn không cần lo lắng về việc cung cấp phần cứng, thiết lập và cấu hình, sao chép dữ liệu, việc vá lỗi phần mềm, hoặc việc mở rộng cụm.

Trong bước này, chúng ta cần tạo một DynamoDB table

1. Tạo **DynamoDB table**
   - Truy cập vào [Giao diện quản trị DynamoDB](https://ap-southeast-1.console.aws.amazon.com/dynamodbv2/home?region=ap-southeast-1#dashboard)
   - Chọn **Tables**
   - Click **Create table**

![DynamoDB Console](/images/2.prerequisite/001-dynamodbconsole.png)

Khi tạo một Table và xác định schema cho DynamoDB của bạn, một trong những lựa chọn đầu tiên bạn sẽ được hỏi là xác định Partition Key và Sort Key cho table của bạn. Đây là một quyết định quan trọng có ảnh hưởng đến cách mà các bản ghi trong table của bạn có thể được truy cập.

**A DynamoDB Partition Key** đóng vai trò như là chỉ số chính để phân vùng dữ liệu của bạn trên nhiều storage nodes của DynamoDB. Đó là một thành phần bắt buộc khi thiết lập một DynamoDB table. Partition Key giúp phân phối dữ liệu của bạn trên các Partition khác nhau.

**A DynamoDB Sorted Key** là một thuộc tính tùy chọn cho phép sắp xếp các mục bên trong mỗi Partition. Bằng cách chỉ định một Sort Key, bạn cho phép DynamoDB sắp xếp các bản ghi trong một Partition dựa trên giá trị của Sort Key.

- Trong bước **Create table**
- Tại **Partition key**, nhập `id`
- Tại Table settings, Chọn **Default settings**
- Click **Create**

![Create Table](/images/2.prerequisite/002-createdynamodbtable.png)

2. Kiểm tra tạo DynamoDB table thành công

- Sau khi click **Create**, đợi một vài phút, và bạn sẽ nhìn thấy **lab-table** xuất hiện.

![Create Success](/images/2.prerequisite/003-createdynamodbsuccess.png)

3.Tạo IAM Role cho API Gateway có thể truy cập DynamoDB

Chúng ta cần tạo **IAM Role** cho API Gateway có quyển put Item vào trong DynamoDB
Để tạo **IAM Role**

- Truy cập vào [Giao diện quản trị IAM](https://us-east-1.console.aws.amazon.com/iam/home?region=ap-southeast-1#/home)
- Chọn **Policies**
- Click **Create policy**

![Create Policy](/images/2.prerequisite/011-createpolicy.png)

- Trong bước **Create policy**
- Click **Json**
- Sao chép và dán **Policy editor**

```bash
{
	"Version": "2012-10-17",
	"Statement": [
		{
			"Sid": "Statement1",
			"Effect": "Allow",
			"Action": [
			    "dynamodb:PutItem"
			],
			"Resource": ["arn:aws:dynamodb:<your-region>:<your-aws-account-id>:table/<you-dynamodb-table>"]
		}
	]
}
```

- Click **Next**

![Create Policy](/images/2.prerequisite/012-createpolicy.png)

- Trong bước **Review**
- Nhập `dynamodb-putitem-policy`
- Kéo xuống và chọn click **Create policy**

![Create Policy](/images/2.prerequisite/013-createpolicy.png)

Sao khi tạo policy, chúng ta sẽ tạo một IAM Role cho API Gateway

- Trong [Giao diện quản trị IAM](https://us-east-1.console.aws.amazon.com/iam/home?region=ap-southeast-1#/home)
- Chọn **Roles**
- Click **Create role**

![Create Role](/images/2.prerequisite/014-creategatewayrole.png)

- Trong bước **Select trusted entity**
- Chọn **AWS service**
- Tại **Service or use case**, chọn **API Gateway**
- Tại **Use case**, chọn **API Gateway**

![Create Role](/images/2.prerequisite/015-creategatewayrole.png)

- Trong bước **Add permissions**
- Click **Next**

![Create Role](/images/2.prerequisite/016-creategatewayrole.png)

- Trong bước **Name, review, and create**
- Nhập `dynamodb-api-gw-role`
- Kéo xuống và click **Create role**

![Create Role](/images/2.prerequisite/017-creategatewayrole.png)

Sau khi tạo **dynamodb-api-gw-role**, chúng ta cần thêm **dynamodb-putitem-policy** vào role này

- Chọn **Roles**
- Tìm `dynamodb-api-gw-role`
- Click **dynamodb-api-gw-role**

![Create Role](/images/2.prerequisite/018-creategatewayrole.png)

- Click **Add permissions**
- Click **Attach policies**

![Create Role](/images/2.prerequisite/019-creategatewayrole.png)

- Tìm và chọn `dynamodb-api-gw-role`
- Click **Add permissions**

![Create Role](/images/2.prerequisite/020-creategatewayrole.png)
