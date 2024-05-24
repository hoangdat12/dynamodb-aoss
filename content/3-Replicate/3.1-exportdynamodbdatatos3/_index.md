---
title: 'Export DynamoDB Data'
date: '`r Sys.Date()`'
weight: 1
chapter: false
pre: ' <b> 3.1 </b> '
---

To replicate data from DynamoDB to Amazon OpenSearch Serverless, a preliminary step involves exporting the data to an Amazon S3 bucket.

1. Export DynamoDB data

- Go to the [AWS S3 Console](https://ap-southeast-1.console.aws.amazon.com/dynamodbv2/home?region=ap-southeast-1#dashboard)
- Select **Exports to S3**
- Click **Export to S3**

![Export DynamoDB Data](/images/2.prerequisite/023-exportdynamodbdata.png)

To export data from DynamoDB to S3, we need enabled **PITR (Point-in-time recovery)**.
**Point-in-time recovery** helps protect your DynamoDB tables from accidental write or delete operations. With point-in-time recovery, you don't have to worry about creating, maintaining, or scheduling on-demand backups.

- In **Export to S3** step
- At **Source table**, choose **lab-table**
- Click **Turn on PITR**
- Enter S3 destination `s3://dynamodb-export-data-lab`
- Select **This AWS account**

![Export DynamoDB Data](/images/2.prerequisite/024-exportdynamodbdata.png)

- Select **Full export**
- Select **Current time**
- Select **DynamoDB JSON**
- Click **Export**

![Export DynamoDB Data](/images/2.prerequisite/025-exportdynamodbdata.png)

After exporting success, we can click **Destination S3 bucket** to check.

![Export DynamoDB Data](/images/2.prerequisite/026-exportdynamodbdata.png)

We will see file export in **AWSDynamoDB/** folder.

![Export DynamoDB Data](/images/2.prerequisite/027-exportdynamodbdata.png)
