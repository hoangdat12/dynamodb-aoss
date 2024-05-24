---
title: 'Synchronize Data'
date: '`r Sys.Date()`'
weight: 4
chapter: false
pre: ' <b> 4 </b> '
---

### Overview

**Amazon DynamoDB Streams** keeps track of changes made in a DynamoDB table and stores this information in a log for up to 24 hours. Applications can access this log and view the data items as they appeared before and after they were modified, in near-real time.
Whenever an application creates, updates, or deletes items in the table, DynamoDB Streams writes a stream record with the primary key attributes of the items that were modified.

**Amazon Kinesis** is a suite of services that provide a way to easily collect, process, and analyze real-time data. It is also extremely cost-effective at any scale. Some use cases of Amazon Kinesis include video/audio solutions, website clickstreams, and IoT data. In this lap, we will use it to streams data from DynamoDB to OpenSearch Serverless

{{% notice info %}}
DynamoDB Streams is best suited for capturing granular level changes made to DynamoDB tables and processing stream records using AWS Lambda, while Kinesis Streams is better for producing and consuming large volumes of data and using Kinesis Analytics, Kinesis Firehose, and Lambda to process stream records
{{% /notice %}}

In this step, we will synchronize Data from DynamoDB to OpenSearch Serverless

The overall architecture of this step will look like this:

![Basic Architecture](/images/4.synchronize/synchronizearchitecture.png)

### Content

- [Synchronize Data With DynamoDB Stream](4.1-dynamodbstream//)
- [Synchronize Data With Kinesis Stream](4.2-kinesisstream/)
