---
title: 'Create S3 Bucket'
date: '`r Sys.Date()`'
weight: 4
chapter: false
pre: ' <b> 2.4 </b> '
---

**Amazon Simple Storage Service (Amazon S3)** is an object storage service that offers industry-leading scalability, data availability, security, and performance. Amazon S3 provides management features so that you can optimize, organize, and configure access to your data to meet your specific business, organizational, and compliance requirements.

In this step, we will create a S3 Bucket to save file that exported from DynamoDB

1. Create **S3 Bucket**

- Go to the [AWS S3 Console](https://ap-southeast-1.console.aws.amazon.com/s3/home?region=ap-southeast-1)
- Select **Bucket**
- Select **Create role**

![S3 Console](/images/2.prerequisite/021-creates3bucket.png)

- In **Create bucket** step
- Enter `dynamodb-export-data-lab`
- Choose `ACLs disabled`
- Scroll down and click **Create bucket**

![Create Bucket](/images/2.prerequisite/022-creates3bucket.png)
