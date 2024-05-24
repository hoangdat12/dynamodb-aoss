---
title: 'Check Data'
date: '`r Sys.Date()`'
weight: 3
chapter: false
pre: ' <b> 3.3 </b> '
---

1. Check Data

- Go to the [AWS OpenSearch Serverless Console](https://ap-southeast-1.console.aws.amazon.com/aos/home?region=ap-southeast-1#opensearch/dashboard)
- Select **Network policies**
- Click **Edit**

![Open DashBoard](/images/3.replicate/009-openaossdashboard.png)

- In **Edit network access policy**
- Click **Enable access to OpenSearch Dashboards**
- Click **Select collection** and choose **lab-collection**
- Click **Update**

![Open DashBoard](/images/3.replicate/010-openaossdashboard.png)

- Now we need select **Collections**
- Click **Dashboard**

![Open DashBoard](/images/3.replicate/011-openaossdashboard.png)

- Click **Menu icon**
- Click **Dev Tools**

![OpenSearch Dev Tools](/images/3.replicate/012-aossdevtool.png)

We can perform the default OpenSearch command to verify whether my index and document have been created successfully or not.

![OpenSearch Command Result](/images/3.replicate/013-aosscheckdata.png)
