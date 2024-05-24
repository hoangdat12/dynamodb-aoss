---
title: 'Replicate To OpenSearch Serverless'
date: '`r Sys.Date()`'
weight: 2
chapter: false
pre: ' <b> 3.2 </b> '
---

To stream data from S3 Bucket to OpenSearch Serverless, we need to create a Lambda function to index data into OpenSearch Serverless.

**Lambda** is a serverless compute service, executes your code in response to events, handling compute resources for you.

1. Create **Lambda function**

- Go to the [AWS Lambda Console](https://ap-southeast-1.console.aws.amazon.com/lambda/home?region=ap-southeast-1#/functions)
- Select **Functions**
- Click **Create function**

![Create Lambda Function](/images/3.replicate/001-createlambdafunction.png)

- Select **Author from scratch**
- Enter `replicate-data-handler`
- At **Runtime**, select **Python 3.12** (newest version)

![Create Lambda Function](/images/3.replicate/002-createlambdafunction.png)

- Click **Change default execution role**
- Select **Use an existing role**
- Choose **lambda-lab-role**
- Click **Create function**

![Create Lambda Function](/images/3.replicate/003-createlambdafunction.png)

- [Download Lambda function](/replciate_data.zip)
- In **Lambda function**
- Click **Upload from**, **.zip file** and select zip file you're installed

![Create Lambda Function](/images/3.replicate/004-replicatedata.png)

After uploading the Lambda code, you'll need to configure some environment variables for the code work.

- Select **Configuration**
- Select **Environment**
- Click **Edit**

![Create Lambda Function](/images/3.replicate/005-replicatedata.png)

We need to add these Environment variables.

{{% notice info %}}
With **OPENSEARCH_HOST**, we can access OpenSearch Serverless Collection that you have just created to get it.
{{% /notice %}}

![Create Lambda Function](/images/3.replicate/006-replicatedata.png)

Before we perform replicate data, we need to edit **Data access policies** of Collection to allow Lambda IAM Role have permissions in OpenSearch Serverless Collection.

**Data access policies** define how your users access the data within your Collections. Data access policies help you manage Collections at scale by automatically assigning access permissions to Collections and indexes that match a specific pattern. Multiple policies can apply to a single resource. It consist of a set of rules, each with three components: a resource type, granted resources, and a set of permissions.

To update **Data access policies**

- Go to the [AWS OpenSearch Serverless Console](https://ap-southeast-1.console.aws.amazon.com/aos/home?region=ap-southeast-1#opensearch/dashboard)
- Select **Data access policies**
- Click **easy-lab-collection**

![Create Lambda Function](/images/3.replicate/007-editdataaccesspolicy.png)

- In **Edit access policy** step
- Click **Add principals**
- Add Lambda role `arn:aws:iam::<your-aws-account-id>:role/<lambda-role-name>`
- Click **Save**

![Create Lambda Function](/images/3.replicate/008-editdataaccesspolicy.png)

To begin replicating data

- Go to the [AWS Lambda Console](https://ap-southeast-1.console.aws.amazon.com/lambda/home?region=ap-southeast-1#/discover)
- Select **Functions** and click **replicate-data-handler** funciton
- Click **Test**

- In **Configure test event** step
- Select **Create new event**
- Enter `replicate-event`
- Paste

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
If you encounter a timeout error, it might be because initially attempted to index document without create a predefined index mapping. In this case, you can rerun the Test after creating the index mapping to ensure that subsequent operations proceed smoothly. Additionally, you can increase the timeout setting for the Lambda function to provide more time for the operations to complete successfully.
{{% /notice %}}

![Create Lambda Function](/images/3.replicate/007-replicatedata.png)
