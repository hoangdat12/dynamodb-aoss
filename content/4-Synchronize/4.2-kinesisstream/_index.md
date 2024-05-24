---
title: 'Synchronize Data with Kinesis Stream'
date: '`r Sys.Date()`'
weight: 2
chapter: false
pre: ' <b> 4.2 </b> '
---

**Amazon Kinesis Data Streams** is a serverless streaming data service that simplifies the capture, processing, and storage of data streams at any scale.

{{% notice info %}}
Data saved in a Kinesis stream is typically stored as binary data, we need to convert it before consume
{{% /notice %}}

To using Kinesis Stream to synchronize data from DynamoDB to OpenSearch Service, we need to enable Kinesis Stream in DynamoDB table and create a Lambda function to handle data.

The architecture overview after you complete this step will be as follows:

![Architecture Overview](/images/4.synchronize/kinesissynchronize.png)

1. Create **Kinesis Stream**

- Go to the [AWS Kinesis Console](https://ap-southeast-1.console.aws.amazon.com/kinesis/home?region=ap-southeast-1#/dashboard)
- Select **Data streams**
- Click **Create data stream**

![Create Kinesis Stream](/images/4.synchronize/010-createkinesisstream.png)

- In **Create data stream** step
- Enter `kinesis-stream-data-lab`
- At **Capacity mode**, select **On-demand**
- Scroll down and click **Create data stream**

![Create Kinesis Stream](/images/4.synchronize/011-createkinesisstream.png)

2. Enable **Kinesis Stream**

- Go to the [AWS DynamoDB Console](https://ap-southeast-1.console.aws.amazon.com/dynamodbv2/home?region=ap-southeast-1#dashboard)
- Select **Tables**
- Click **lab-table**

![Access DynamoDB](/images/4.synchronize/001-accessdynamodb.png)

- In **lab-table**
- Select **Exports and Streams**
- At **Amazon Kinesis data stream details**, click **Turn on**

![Enable Kinesis Stream](/images/4.synchronize/010-enablekinesisstream.png)

- In **Enable Kinesis Stream** step
- Select **kinesis-stream-data-lab**
- Select **Microsecond**
- Click **Turn on stream**

![Enable Kinesis Stream](/images/4.synchronize/011-enablekinesisstream.png)

3. Create Lambda function

You need to download this [file](/kinesis_stream.zip), and follow the steps as done in section 4.1 to create the Lambda function with name `sync-data-with-kinesis-stream`.

You also need to edit the IAM Role to add a policy that allows the Lambda function to consume data from Kinesis Streams.

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

After creating a Lambda function, we need to **Add trigger** for it

- Go to the **sync-data-with-kinesis-stream** function
- Click **Add trigger**

![Add trigger](/images/4.synchronize/012-addtrigger.png)

- In **Add trigger** step
- Select **Kinesis**
- Select **kinesis-stream-data-lab**
- Scroll down and click **Add**

![Add trigger](/images/4.synchronize/013-addtrigger.png)

To testing Kinesis Streams pipeline, we will go to the [AWS API Gateway Console](https://ap-southeast-1.console.aws.amazon.com/apigateway/main/apis?api=l9l2p0i2h4&region=ap-southeast-1)

- Select **APIs**
- Select **dynamodb-api-gw**

![API Gateway Console](/images/4.synchronize/007-apigatewayconsole.png)

- In **dynamodb-api-gw**
- Select **POST**
- Select **Test**
- At **Request body**, paste

```bash
{
    "name": "Test Kinesis Stream Pipeline",
    "type": "Test"
}
```

- Scroll down and click **Test**

![Test API Gateway Create Object](/images/4.synchronize/008-testcreatedynamodbdata.png)

We can verify the successful creation of data. By the way, Go to the [AWS Cloud Watch](https://ap-southeast-1.console.aws.amazon.com/cloudwatch/home?region=ap-southeast-1#home:) to view log.

**CloudWatch** enables you to monitor your complete stack (applications, infrastructure, network, and services) and use alarms, logs, and events data to take automated actions and reduce mean time to resolution (MTTR). This frees up important resources and allows you to focus on building applications and business value.

We can see like this:

![Cloud Watch Log](/images/4.synchronize/009-cloudwatchlog2.png)

{{% notice info %}}
We can also view in DynamoDB table or OpenSearch Serverless Dashboard, you can also perform delete or modify actions to learn more about DynamoDB operations.
{{% /notice %}}
