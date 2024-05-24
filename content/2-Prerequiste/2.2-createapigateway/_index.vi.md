---
title: 'Tạo API Gateway'
date: '`r Sys.Date()`'
weight: 2
chapter: false
pre: ' <b> 2.2 </b> '
---

**Amazon API Gateway** là một dịch vụ được quản lý hoàn toàn bởi AWS, làm cho dễ dàng để tạo, publish, maintain, monitor, và bảo mật APIs ở bất kỳ quy mô nào. APIs đóng vào trò như một "cửa trước" để truy cập vào ứng dụng, business logic, hoăc chức năng từ phía backend server.

Trong bước này, chúng ta cần tạo một API Gateway.

Tổng quan kiến trúc sau khi các bạn hoàn tất bước này sẽ như sau:

![Architecture overview](/images/2.prerequisite/gatewayarchitecture.png)

Trong bài lap này, chúng ta sẽ sử dụng API Gateway như một Proxy cho DynamoDB.

1. Tạo **API Gateway**
   - Truy cập vào [Giao diện quản trị API Gateway](https://ap-southeast-1.console.aws.amazon.com/dynamodbv2/home?region=ap-southeast-1#dashboard)
   - Chọn **APIs**
   - Click **Create API**

![API Gateway Console](/images/2.prerequisite/004-apigatewayconsole.png)

- Trong bước **Create API**
- Tại **REST API**, click **Build**

![Create API](/images/2.prerequisite/005-buildrestapi.png)

- Trong bước **Create REST API**
- Chọn **New API**
- Tại **API name**, nhập `dynamodb-api-gw`
- Tại **API endpoint type**, chọn **Regional**
- Click **Create API**

![Create REST API](/images/2.prerequisite/006-createrestapi.png)

- Sau khi tạo thành công, Chúng ta sẽ được đưa đến **dynamodb-api-gw** mà chúng ta vừa tạo

2. Tạo **API Gateway Resource** để xử lý data trước khi lưu vào **DynamoDB**

- Chọn **Resources**
- Click **Create resource**

![Create Resources](/images/2.prerequisite/007-createresources.png)

- Trong bước **Create resource**
- Tạo **Resource name**, nhập `lab`
- Click **Create resource**

![Complete Create Resource](/images/2.prerequisite/008-completecreateresource.png)

Sau khi tạo **Resource**, chúng ta cần tạo một method

{{% notice info %}}
Method bao gồm: GET, POST, PATCH, PUT, DELETE
Với ANY method, Nó sẽ match với toàn bộ những method ở trên
{{% /notice %}}

- Click **lab** resource
- Click **Create method**

![Create Method](/images/2.prerequisite/009-createmethod.png)

- Trong bước **Create method**
- Tại **Method type**, chọn **POST**
- Chọn **AWS service**
- Tại **AWS Region**, chọn **ap-southeast-1**
- Tại **AWS service**, chọn **DynamoDB**
- Tại **HTTP method**, chọn **POST**

![Create Method Detail](/images/2.prerequisite/010-createmethoddetail.png)

- Tại **Action type**, chọn **Use action name**
- Tại **Action name**, nhập `Query`
- Tại **Execution role**, nhập `arn:aws:iam::<you-aws-account-id>:role/<your-api-gateway-role>`
- Click **Create method** để hoàn tất

![Create Method Detail](/images/2.prerequisite/011-createmethoddetail.png)

Để cho phép API Gateway có thể trực tiếp put Item vào trong DynamoDB mà không cần gọi một Lambda function, chúng ta có thể sử dụng Velocity Template Language (VTL) trong API Gateway. VTL là một ngôn ngữ mạnh mẻ được sử dụng trong Amazon API Gateway để map và chuyển đối API request và response giữa những định dạng dữ liệu khác nhau, như giữa JSON và XML, hoặc để xử lý dữ liệu trước khi trả về cho User.

Chúng ta có thể sử dụng VTL để put Item vào trong DynamoDB.

- Chọn **lab** resource
- Chọn **POST** method
- Chọn **Integration request** và click **Edit**

![VTL Put Item](/images/2.prerequisite/012-vtlputitem.png)

- Trong **Integration request**
- Kéo xuống **Mapping templates**
- Nhập `application/json`
- Dán đoạn cấu hình dưới vào **Mapping templates**

```bash
#set($timestamp = $context.requestTimeEpoch)
{
    "TableName": "lab-table",
    "Item": {
        "id": {
            "S": "$context.requestId"
        },
        "type": {
            "S": "$input.path('type')"
        },
        "name": {
            "S": "$input.path('name')"
        },
        "createdAt": {
            "S": "$context.requestTimeEpoch"
        }
    }
}
```

- Click **Save**

![VTL Put Item](/images/2.prerequisite/013-vtlputitem.png)

Sau khi tạo **Resource**, bạn phải deploy để làm cho nó có thể gọi được từ phía User.

Để deploy, chúng ta chọn **POST** và click **Deploy API**

![Deploy API](/images/2.prerequisite/009-deployapi.png)

- Trong bước **Deploy API**
- Chọn **New Stage**
- Tại **Stage name**, nhập `v1`
- Click **Deploy**

![Deploy API](/images/2.prerequisite/010-deployapi.png)

Bây giờ, chúng ta có thể kiểm tra việc tạo dữ liệu cho DynamoDB từ API Gateway.

- Click **Test**
- Nhập

```bash
{
    "name": "Test name",
    "type": "Test type"
}
```

- Kéo xuống dưới và chọn **Test**

![Deploy API](/images/2.prerequisite/011-testapigw.png)

Sau khi kiểm tra tạo dữ liệu thành công, chúng ta sẽ thấy kết quả như bên dưới.

![Deploy API](/images/2.prerequisite/012-testapigw.png)

Bạn có thể lặp lại bước này để có thể đặt thêm dữ liệu vào DynamoDB.
