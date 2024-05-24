---
title: 'Create OpenSearch Serverless'
date: '`r Sys.Date()`'
weight: 3
chapter: false
pre: ' <b> 2.3 </b> '
---

**OpenSearch Serverless** is a serverless option in Amazon OpenSearch Service. As a developer, you can use OpenSearch Serverless to run petabyte-scale workloads without configuring, managing, and scaling OpenSearch clusters. You get the same interactive millisecond response times as OpenSearch Service with the simplicity of a serverless environment.

In this step, we need to create IAM Role for Lambda function and API Gateway

1. Create **Lambda replicate policy**
   - Go to the [AWS OpenSearch Console](https://ap-southeast-1.console.aws.amazon.com/aos/home?region=ap-southeast-1#opensearch/collections)
   - Select **Collections**
   - Select **Create collection**

![OpenSearch Console](/images/2.prerequisite/021-createaoss.png)

- In **Configure collection settings** step
- Enter **lab-collection**
- At **Collection type**, select **Search**

![Create AOSS](/images/2.prerequisite/022-createaoss.png)

- At **Security**, choose **Easy create**
- Scroll down and click **Next**

![Create AOSS](/images/2.prerequisite/023-createaoss.png)

- In **Review and create collection** step
- Click **Create collection**
