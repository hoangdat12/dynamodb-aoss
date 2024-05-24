---
title: 'Create DynamoDb table'
date: '`r Sys.Date()`'
weight: 1
chapter: false
pre: ' <b> 2.1 </b> '
---

**DynamoDB** is a fully managed NoSQL database service that provides fast and predictable performance with seamless scalability. DynamoDB lets you offload the administrative burdens of operating and scaling a distributed database so that you don't have to worry about hardware provisioning, setup and configuration, replication, software patching, or cluster scaling.

In this step, we need to create a DynamoDB table

1. Create **DynamoDB table**
   - Go to the [AWS DynamoDB Console](https://ap-southeast-1.console.aws.amazon.com/dynamodbv2/home?region=ap-southeast-1#dashboard)
   - Select **Tables**
   - Click **Create table**

![DynamoDB Console](/images/2.prerequisite/001-dynamodbconsole.png)

When creating a Table and defining your DynamoDB schema, one of the first options you’ll be asked is to specify your Table’s Partition Key and Sort Key. This is an important decision that has impact on how your table’s record’s can be accessed.

**A DynamoDB Partition Key** serves as the primary identifier for partitioning your data across DynamoDB's multiple storage nodes. It's a mandatory component when setting up a DynamoDB table. The Partition Key helps distribute your data across different partitions

**A DynamoDB Sorted Key** is an optional attribute that allows for the sorting of items within each partition. By specifying a Sort Key, you enable DynamoDB to order the records within a partition based on the Sort Key's value

- In **Create table** step
- At **Partition key**, enter `id`
- At **Table settings**, select **Default settings**
- Click **Create**

![Create Table](/images/2.prerequisite/002-createdynamodbtable.png)

2. Check create table success

- After clicking **Create**, wait a few moments, and you will see the **lab-table** appear.

![Create Success](/images/2.prerequisite/003-createdynamodbsuccess.png)

3.Create an IAM Role for API Gateway can access DynamoDB

We need to create **IAM Role** for API Gateway have permission put Item into DynamoDB
To create **IAM Role**

- Go to the [AWS IAM Console](https://us-east-1.console.aws.amazon.com/iam/home?region=ap-southeast-1#/home)
- Select **Policies**
- Click **Create policy**

![Create Policy](/images/2.prerequisite/011-createpolicy.png)

- In **Create policy** step
- Click **Json**
- Coppy and paste into **Policy editor**

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

- In **Review** step
- Enter `dynamodb-putitem-policy`
- Scroll down and click **Create policy**

![Create Policy](/images/2.prerequisite/013-createpolicy.png)

After create policy, we will create a IAM Role for API Gateway

- In [AWS IAM Console](https://us-east-1.console.aws.amazon.com/iam/home?region=ap-southeast-1#/home)
- Select **Roles**
- Click **Create role**

![Create Role](/images/2.prerequisite/014-creategatewayrole.png)

- In **Select trusted entity** step
- Select **AWS service**
- At **Service or use case**, select **API Gateway**
- At **Use case**, select **API Gateway**

![Create Role](/images/2.prerequisite/015-creategatewayrole.png)

- In **Add permissions** step
- Click **Next**

![Create Role](/images/2.prerequisite/016-creategatewayrole.png)

- In **Name, review, and create** step
- Enter `dynamodb-api-gw-role`
- Scroll down and click **Create role**

![Create Role](/images/2.prerequisite/017-creategatewayrole.png)

After create **dynamodb-api-gw-role**, we need to add **dynamodb-putitem-policy** to this role

- Select **Roles**
- Search `dynamodb-api-gw-role`
- Click **dynamodb-api-gw-role**

![Create Role](/images/2.prerequisite/018-creategatewayrole.png)

- Click **Add permissions**
- Click **Attach policies**

![Create Role](/images/2.prerequisite/019-creategatewayrole.png)

- Search and select `dynamodb-api-gw-role`
- Click **Add permissions**

![Create Role](/images/2.prerequisite/020-creategatewayrole.png)
