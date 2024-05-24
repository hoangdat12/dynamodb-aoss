---
title: 'Synchronize Data With DynamoDB Stream'
date: '`r Sys.Date()`'
weight: 1
chapter: false
pre: ' <b> 4.1 </b> '
---

**A DynamoDB Stream** is a time-ordered sequence of events recording all the modifications for DynamoDB tables in near real-time. Similar to change data capture, DynamoDB Streams consist of multiple Insert, Update, and Delete events. Each record has a unique sequence number which is used for ordering.

The architecture overview after you complete this step will be as follows:

![Architecture Overview](/images/4.synchronize/dynamodbsynchronize.png)

1. Create **Lambda function**

- Go to the [AWS Lambda Console](https://ap-southeast-1.console.aws.amazon.com/lambda/home?region=ap-southeast-1#/discover)
- Select **Functions**
- Click **Create function**

![Create Lambda Function](/images/4.synchronize/001-createlambdafunction.png)

- In **Create function** step
- Select **Author from scratch**
- At **Runtime**, select **Python 3.12** (newest version)
- Enter `sync-data-with-dynamodb-stream`

![Create Lambda Function](/images/4.synchronize/002-createlambdafunction.png)

- At **Change default execution role**, choose **Use an existing role**
- Select **lambda-lab-role**

{{% notice info %}}
**lambda-lab-role** is the IAM Role that we are created in the previous step
{{% /notice %}}

- Click **Create function**

![Create Lambda Function](/images/4.synchronize/003-createlambdafunction.png)

- [Download Lambda function](/dynamodb_stream.zip)

- Click **Upload** and choose the file that you downloaded, then proceed to upload it

{{% notice info %}}
In DynamoDB stream, event will have three types are: INSERT, REMOVE and MODIFY
{{% /notice %}}

2. Turn on **Dynamodb Stream**

To synchronize data with **DynamoDB Stream**, we need enable **DynamoDB Stream** in DynamoDB table

- Go to the [AWS DynamoDB Console](https://ap-southeast-1.console.aws.amazon.com/dynamodbv2/home?region=ap-southeast-1#dashboard)
- Select **Tables**
- Click **lab-table**

![Access DynamoDB](/images/4.synchronize/001-accessdynamodb.png)

- In **lab-table**
- Select **Exports and Streams**
- At **DynamoDB stream details**, click **Turn on**

![Turn on Stream](/images/4.synchronize/002-turnondynamodbstream.png)

- In **Turn on DynamoDB stream**
- Select **New image**
- Click **Turn on stream**

![Turn on Stream](/images/4.synchronize/003-turnondynamodbstream.png)

Now, after Turn on stream, we need to create a lambda function to receive record that triggered from DynamoDB stream, but before do that, we need edit **lambda-lap-role** to allow lambda create received record from DynamoDB stream.

- Go to the [AWS IAM Console](https://us-east-1.console.aws.amazon.com/iam/home?region=ap-southeast-1#/home)
- Select **Roles**
- Search and select **006-editlambdarole.png**

![AWS IAM Console](/images/4.synchronize/006-editlambdarole.png)

- In **lambda-lap-role**
- Click **Add permissions**
- Click **Create inline policy**

![Attack Policy](/images/4.synchronize/008-createpolicy.png)

- In **Specify permissions** step
- Click **Json**
- Coppy and paste

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

- Scroll down and lick **Next**

![Create inline policy](/images/4.synchronize/008-createpolicy.png)

- In **Review and create**
- Enter `dynamodb-stream-data`
- Click **Create policy**

![Create inline policy](/images/4.synchronize/009-createpolicy.png)

- After create `dynamodb-stream-data`, we need create `aoss-policy` with policy like this

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

- After update IAM Role, we will go back to the [AWS DynamoDB Console](https://ap-southeast-1.console.aws.amazon.com/dynamodbv2/home?region=ap-southeast-1#dashboard)
- Select **Exports and Streams**
- Scroll down and click **Create trigger**

![Turn on Stream](/images/4.synchronize/004-createtrigger.png)

- In **Create a trigger** step
- Select **sync-data-with-dynamodb-stream**
- Click **Create trigger**

![Turn on Stream](/images/4.synchronize/005-createtrigger.png)

Before perform testing, we need to create Environment Variable as the previous step - when we create Replicate Lambda function

![Turn on Stream](/images/4.synchronize/006-createenv.png)

To testing DynamoDB stream pipeline, we will go to the [AWS API Gateway Console](https://ap-southeast-1.console.aws.amazon.com/apigateway/main/apis?api=l9l2p0i2h4&region=ap-southeast-1)

- Select **APIs**
- Select **dynamodb-api-gw**

![API Gateway Console](/images/4.synchronize/007-apigatewayconsole.png)

- In **dynamodb-api-gw**
- Select **POST**
- Select **Test**
- At **Request body**, paste

```bash
{
    "name": "Test DynamoDB Stream Pipeline",
    "type": "Test"
}
```

- Scroll down and click **Test**

![Test API Gateway Create Object](/images/4.synchronize/008-testcreatedynamodbdata.png)

We can verify the successful creation of data. By the way, Go to the [AWS Cloud Watch](https://ap-southeast-1.console.aws.amazon.com/cloudwatch/home?region=ap-southeast-1#home:) to view log.

**CloudWatch** enables you to monitor your complete stack (applications, infrastructure, network, and services) and use alarms, logs, and events data to take automated actions and reduce mean time to resolution (MTTR). This frees up important resources and allows you to focus on building applications and business value.

We can see like this:

![Cloud Watch Log](/images/4.synchronize/009-cloudwatchlog.png)

{{% notice info %}}
You can also view in DynamoDB table or OpenSearch Serverless Dashboard, you can also perform delete or modify actions to learn more about DynamoDB operations.
{{% /notice %}}
