---
title: 'Replicate đến OpenSearch Serverless'
date: '`r Sys.Date()`'
weight: 2
chapter: false
pre: ' <b> 3.2 </b> '
---

Để stream data từ S3 Bucket đến OpenSearch Serverless, chúng ta cần tạo một Lambda function để index dữ liệu vào trong OpenSearch Serverless.

**Lambda** là một dịch vụ serverless compute, thực thi code của bạn để phản hồi những event, xử lý tài nguyên tính toán cho bạn.

1. Tạo **Lambda function**

- Truy cập vào [Giao diện quản trị Lambda](https://ap-southeast-1.console.aws.amazon.com/lambda/home?region=ap-southeast-1#/functions)
- Chọn **Functions**
- Click **Create function**

![Create Lambda Function](/images/3.replicate/001-createlambdafunction.png)

- Chọn **Author from scratch**
- Nhập `replicate-data-handler`
- Tại **Runtime**, chọn **Python 3.12** (phiên bản mới nhất)

![Create Lambda Function](/images/3.replicate/002-createlambdafunction.png)

- Click **Change default execution role**
- Chọn **Use an existing role**
- Chọn **lambda-lab-role**
- Click **Create function**

![Create Lambda Function](/images/3.replicate/003-createlambdafunction.png)

- [Download Lambda function](/replciate_data.zip)
- Trong **Lambda function**
- Click **Upload from**, **.zip file** và chọn zip file mà bạn vừa tải

![Create Lambda Function](/images/3.replicate/004-replicatedata.png)

Sau khi tải Lambda code lên, bạn cần phải cấu hình một vài biến môi trường để code có thể hoạt động.

- Chọn **Configuration**
- Chọn **Environment**
- Click **Edit**

![Create Lambda Function](/images/3.replicate/005-replicatedata.png)

Chúng ta cần thêm những biến môi trường như bên dưới.

{{% notice info %}}
With **OPENSEARCH_HOST**, we can access OpenSearch Serverless Collection that you have just created to get it.
{{% /notice %}}

![Create Lambda Function](/images/3.replicate/006-replicatedata.png)

Trước khi thực hiện replicate data, chúng ta cần chỉnh sửa **Data access policies** của Collection để cho phép Lambda IAM role có thể có quyền trong OpenSearch Serverless Collection.

**Data access policies** định nghĩa làm cách nào mà user của bạn có thể truy cập vào dữ liệu bên trong Collection. Data access policies giúp bạn quản lý những Collection ở bất kỳ quy mô nào bằng cách tự động gắn quyển truy cập đến những Collection và những index phù hợp với những mẫu nhất đinhk. Nhiều policies có thể được áp dụng cho một tài nguyên. Nó bao gồm một bộ quy tắc, mỗi quy tắc gồm 3 thành phần: loại tài nguyên, tài nguyên được cấp và một một quyền.

Để cập nhật **Data access policies**

- Truy cập vào [Giao diện quản trị OpenSearch Serverless](https://ap-southeast-1.console.aws.amazon.com/aos/home?region=ap-southeast-1#opensearch/dashboard)
- Chọn **Data access policies**
- Click **easy-lab-collection**

![Create Lambda Function](/images/3.replicate/007-editdataaccesspolicy.png)

- Trong bước **Edit access policy**
- Click **Add principals**
- Thêm Lambda role `arn:aws:iam::<your-aws-account-id>:role/<lambda-role-name>`
- Click **Save**

![Create Lambda Function](/images/3.replicate/008-editdataaccesspolicy.png)

Để bắt đầu replicate data.

- Truy cập vào [Giao diện quản trị Lambda](https://ap-southeast-1.console.aws.amazon.com/lambda/home?region=ap-southeast-1#/discover)
- Chọn **Functions** và click **replicate-data-handler** funciton
- Click **Test**

- Trong bước **Configure test event**
- Chọn **Create new event**
- Nhập `replicate-event`
- Dán cấu hình bên dưới

```bash
{
  "Records": [
    {
      "s3": {
        "bucket": {
          "name": <s3-bucket-name>
        },
        "object": {
          "key": "AWSDynamoDB/<export-folder>/data/<gzip-file>"
        }
      }
    }
  ]
}
```

- Click **Save** và click **Test**

{{% notice info %}}
Nếu bạn bắt gặp lỗi timeout, có thể là vì trong lần đầu tiên thực hiện, OpenSearch sẽ tạo ra một index mapping cho dữ liệu mà chúng ta gửi lên nhưng thời gian thực hiện mặc định trong Lambda là 3s, nếu quá thời gian thì nó sẽ throw lỗi timeout. Trong trường hợp này, bạn có thể chạy test lại một lần nữa để index lại dữ liệu. Thêm vào đó, bạn cũng có thể tăng thời gian timeout mặc định của lambda để giải quyết vấn đề này.
{{% /notice %}}

![Create Lambda Function](/images/3.replicate/007-replicatedata.png)
