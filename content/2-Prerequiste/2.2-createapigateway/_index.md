---
title: 'Create API Gateway'
date: '`r Sys.Date()`'
weight: 2
chapter: false
pre: ' <b> 2.2 </b> '
---

**Amazon API Gateway** is a fully managed service that makes it easy for developers to create, publish, maintain, monitor, and secure APIs at any scale. APIs act as the "front door" for applications to access data, business logic, or functionality from your backend services.

In this step, we need to create a API Gateway

The architecture overview after you complete this step will be as follows

![Architecture overview](/images/2.prerequisite/gatewayarchitecture.png)

In this lap, we will use API Gateway as a Proxy for DynamoDB

1. Create **API Gateway**
   - Go to the [AWS API Gateway Console](https://ap-southeast-1.console.aws.amazon.com/dynamodbv2/home?region=ap-southeast-1#dashboard)
   - Select **APIs**
   - Click **Create API**

![API Gateway Console](/images/2.prerequisite/004-apigatewayconsole.png)

- In **Create API** step
- At **REST API**, click **Build**

![Create API](/images/2.prerequisite/005-buildrestapi.png)

- In **Create REST API** step
- Select **New API**
- At **API name**, enter `dynamodb-api-gw`
- At **API endpoint type**, choose **Regional**
- Click **Create API**

![Create REST API](/images/2.prerequisite/006-createrestapi.png)

- After create success, we will be redirected to **dynamodb-api-gw** that we just created

2. Create **API Gateway Resource** to handle data before saving in **DynamoDB**

- Select **Resources**
- Click **Create resource**

![Create Resources](/images/2.prerequisite/007-createresources.png)

- In **Create resource** step
- At **Resource name**, enter `lab`
- Click **Create resource**

![Complete Create Resource](/images/2.prerequisite/008-completecreateresource.png)

After creating **Resource**, we need to create method

{{% notice info %}}
Method consist of GET, POST, PATCH, PUT, DELETE
With the ANY method, it will match with the above methods
{{% /notice %}}

- Click **lab** resource
- Click **Create method**

![Create Method](/images/2.prerequisite/009-createmethod.png)

- In **Create method** step
- At **Method type**, choose **POST**
- Select **AWS service**
- At **AWS Region**, choose **ap-southeast-1**
- At **AWS service**, choose **DynamoDB**
- At **HTTP method**, choose **POST**

![Create Method Detail](/images/2.prerequisite/010-createmethoddetail.png)

- At **Action type**, select **Use action name**
- At **Action name**, enter `PutItem`
- At **Execution role**, enter `arn:aws:iam::<you-aws-account-id>:role/<your-api-gateway-role>`
- Click **Create method** to complete

![Create Method Detail](/images/2.prerequisite/011-createmethoddetail.png)

To allow API Gateway to directly put an item into DynamoDB without invoking a Lambda function, we can utilize the Velocity Template Language (VTL) within API Gateway. VTL is a powerful template language used in Amazon API Gateway to map and transform API requests and responses between different data formats, such as between JSON and XML, or to process data before it's returned to end users.

We can use VTL to write a put Item command to DynamoDB.

- Select **lab** resource
- Select **POST** method
- Select **Integration request** and click **Edit**

![VTL Put Item](/images/2.prerequisite/012-vtlputitem.png)

- In **Integration request**
- Scroll down to **Mapping templates**
- Enter `application/json`
- Paste

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

After creating **Resource**, you must deploy it to make it callable.

To deploy, we choose **POST** and click **Deploy API**

![Deploy API](/images/2.prerequisite/009-deployapi.png)

- In **Deploy API**
- Select **New Stage**
- At **Stage name**, enter `v1`
- Click **Deploy**

![Deploy API](/images/2.prerequisite/010-deployapi.png)

Now, we can test creating DynamoDB data from API Gateway.

- Click **Test**
- Enter

```bash
{
    "name": "Test name",
    "type": "Test type"
}
```

- Scroll down and click **Test**

![Deploy API](/images/2.prerequisite/011-testapigw.png)

After a successful test, we will see results like this.

![Deploy API](/images/2.prerequisite/012-testapigw.png)

You can repeat this test to create a little bit of data in DynamoDB.
