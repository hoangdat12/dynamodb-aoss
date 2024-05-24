---
title: 'Prepre IAM Role'
date: '`r Sys.Date()`'
weight: 5
chapter: false
pre: ' <b> 2.5 </b> '
---

In this step, we need to create IAM Role for Lambda function and API Gateway

1. Create **Lambda replicate Role**
   - Go to the [AWS IAM Console](https://us-east-1.console.aws.amazon.com/iam/home?region=ap-southeast-1#/home)
   - Select **Roles**
   - Select **Create role**

![IAM Console](/images/2.prerequisite/001-iamconsole.png)

- In **Select trusted entity** step
- Select **AWS service**
- At **Service or use case**, Select **Lambda**
- At **Use case**, select **Lambda**
- Click **Create**

![Create Lambda Replicate Role](/images/2.prerequisite/002-lambdareplicaterole.png)

- In **Add permissions** step
- Search and select **AWSLambdaExecute**
- Click **Next**

![Create Lambda Replicate Role](/images/2.prerequisite/003-createlambdareplicaterole.png)

- In **Name, review, and create** step
- Enter `lambda-lab-role`
- Scroll down and click **Create role**

![Create Lambda Replicate Role](/images/2.prerequisite/004-createlambdareplicaterole.png)
