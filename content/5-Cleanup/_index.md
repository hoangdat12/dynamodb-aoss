+++
title = "Clean up resources"
date = 2021
weight = 5
chapter = false
pre = "<b>5. </b>"
+++

We'll take the following steps to clean up the resources we created in this lab.

1. Go to the [AWS OpenSearch Serverless Console](https://ap-southeast-1.console.aws.amazon.com/aos/home?region=ap-southeast-1#opensearch/dashboard)

- Select **Collections**.
- Select **lab-collection**
- Click **Delete** and enter `confirm`

![Clean OpenSearch](/images/5.cleanup/001-deleteopensearch.png)

2. Go to the [AWS Kinesis Console](https://ap-southeast-1.console.aws.amazon.com/kinesis/home?region=ap-southeast-1#/dashboard)

- Select **Data streams**.
- Select **kinesis-stream-data-lab**
- Click **Actions** and select **Delete**, after that enter `delete`

![Clean Kinesis](/images/5.cleanup/002-deletekinesis.png)

3. Go to the [AWS DynamoDB Console](https://ap-southeast-1.console.aws.amazon.com/dynamodbv2/home?region=ap-southeast-1#dashboard)

- Select **Tables**
- Click **lab-table**
- Select **Actions** and click **Delete table**, after that enter `confirm`

![Clean DynamoDB](/images/5.cleanup/003-deletedynamodb.png)

You can do the same for the Lambda function, IAM Role and API Gateway to clearn other resources
