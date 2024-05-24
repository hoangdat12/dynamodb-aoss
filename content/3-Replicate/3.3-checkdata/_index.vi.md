---
title: 'Kiểm tra Data'
date: '`r Sys.Date()`'
weight: 3
chapter: false
pre: ' <b> 3.3 </b> '
---

1. Kiểm tra Data

- Truy cập vào [Giao diện quản trị OpenSearch Serverless](https://ap-southeast-1.console.aws.amazon.com/aos/home?region=ap-southeast-1#opensearch/dashboard)
- Chọn **Network policies**
- Click **Edit**

![Open DashBoard](/images/3.replicate/009-openaossdashboard.png)

- Trong **Edit network access policy**
- Click **Enable access to OpenSearch Dashboards**
- Click **Select collection** và chọn **lab-collection**
- Click **Update**

![Open DashBoard](/images/3.replicate/010-openaossdashboard.png)

- Bây giờ chúng ta cần chọn **Collections**
- Click **Dashboard**

![Open DashBoard](/images/3.replicate/011-openaossdashboard.png)

- Click **Menu icon**
- Click **Dev Tools**

![OpenSearch Dev Tools](/images/3.replicate/012-aossdevtool.png)

Chúng ta có thể thực hiện truy vấn mặc định trong OpenSearch Dev Tools để kiểm tra xem index mapping có được tạo và dữ liệu đã được ghi hay chưa.

![OpenSearch Command Result](/images/3.replicate/013-aosscheckdata.png)
