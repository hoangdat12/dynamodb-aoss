---
title: 'Khởi tạo tài nguyên với Terraform'
date: '`r Sys.Date()`'
weight: 1
chapter: false
pre: ' <b> 6.1 </b> '
---

#### Chuẩn bị code

1. [Click ở đây](https://github.com/hoangdat12/eks-workshop.git) để tải code về
2. Mở code và di chuyển đến thư mục terraform

#### Cài đặt môi trường

1. Tải **Terraform**

- Thực hiện câu lệnh để cài đặt Terraform

```bash
sudo yum install -y yum-utils shadow-utils
sudo yum-config-manager --add-repo https://rpm.releases.hashicorp.com/AmazonLinux/hashicorp.repo
sudo yum -y install terraform
```

- Cấu hình ở local để tương tác với AWS, thực thi lệnh `aws configure`

#### Khởi tạo tài nguyên

1. Thực thi câu lệnh `terraform init` để khởi tại tài nguyên cho **Terraform**

![Init terraform](/images/6.terraform/6.1-initterraform.png)

2. Thực thi câu lệnh `terraform validate` để kiểm tra cú pháp của Terraform file

![Validate terraform](/images/6.terraform/6.1-validateterraform.png)

3. Thực thi câu lệnh `terraform plan` để tạo hoặc xem sự thay đổi của Terraform

![Plan terraform](/images/6.terraform/6.1-planterraform.png)

4. Thực thi câu lệnh `terraform apply -auto-approve` để apply cho aws

![Apply terraform](/images/6.terraform/6.1-applyterraform.png)

5. Tiến hành kiểm tra trong AWS Console

- Vào EC2 kiểm tra xem có đủ 3 Instance là **jenkins instance** và 2 **nodes**
- Vào EKS kiểm tra xem có **eks-cluster**

#### Chuẩn bị

1. Tạo **Github Access Token**

- Truy cập [Github Setting](https://github.com/settings/profile) -> Cuộn xuống và chọn **Developing Setting** -> chọn **Personal access token** và click **Token (classic)**
- Chọn **Generate new token** và click **Generate new token (classic)**

![Github Access Token](/images/4.cicd/4.1-generategittoken.png)

- Nhập `jenkins-access`
- Tích tất cả các button ở dưới
- Click **Generate token**

{{%notice tip%}}
Để hiểu hơn về cấu hình Github Access Token, chúng ta có thể tham khảo [tại đây](https://docs.cloudbees.com/docs/cloudbees-ci-kb/latest/client-and-managed-controllers/github-user-scopes-and-organization-permissions-overview)
{{%/notice%}}

![Generate token](/images/4.cicd/4.1-generatetoken.png)

- Sao chép **Token** vừa tạo
- Truy cập **Jenkins Browser**
- Tại **Dashboard** chọn **Manages Jenkins**

![Manage Jenkins](/images/4.cicd/4.1-managejenkins.png)

- Chọn **Credentials**

![Credentials Jenkins](/images/4.cicd/4.1-credentials.png)

- Click **globals**
- Click **Add credentials**
- Chọn **Kind** is **Username with password**
- Nhập Github **username** và **token** vừa tạo
- Nhập ID `jenkins-git`
- Chọn **Create**

![Create Github Token](/images/4.cicd/4.1-creategittoken.png)

2. Tạo **Docker hub Access Token**

- Truy cập [Docker hub](https://hub.docker.com/) -> chọn **My Account**
- Chọn **Security**
- Chọn **New Access Token**

![Create Docker Access Token](/images/4.cicd/4.1-createdockertoken.png)

- Nhập **Access token Description**
- Click **Generate**

![Generate Docker Token](/images/4.cicd/4.1-generatedockertoken.png)

- Sao chép **Docker Access Token** và làm tương tự với **Github Access Token** trong Jenkins
- Truy cập **Jenkins Browser**
- Tại **Dashboard** chọn **Manages Jenkins**
- Chọn **Credentials**
- Click **globals**
- Click **Add credentials**
- Chọn **Kind** là **Username with password**
- Nhập Docker **username** và **token** vừa tạo
- Nhập ID `docker-crd`
- Chọn **Create**

![Complete Generate Docker Token](/images/4.cicd/4.1-completegeneratedockertoken.png)

3. Tạo **AWS_ACCESS_KEY_ID**

- Truy cập **Jenkins Browser**
- Tại **Dashboard** chọn **Manages Jenkins**
- Chọn **Credentials**
- Click **globals**
- Click **Add credentials**
- Chọn **Kind** là **Secret text**
- Nhập Docker **secret** của aws_access_key_id trong file.cvs
- Nhập ID `AWS_ACCESS_KEY_ID`
- Chọn **Create**

![Create AWS access key](/images/6.terraform/6.1-awsaccesskey.png)

4. Tạo **AWS_SECRET_ACCESS_KEY**

- Truy cập **Jenkins Browser**
- Tại **Dashboard** chọn **Manages Jenkins**
- Chọn **Credentials**
- Click **globals**
- Click **Add credentials**
- Chọn **Kind** is **Secret text**
- Nhập Docker **secret** của aws_secret_access_key trong file.cvs
- Nhập ID `AWS_SECRET_ACCESS_KEY`
- Chọn **Create**

![Create AWS access key](/images/6.terraform/6.1-awssecretaccesskey.png)

5. Hoàn tất

- Sau khi hoàn thành, **Jenkins Credentails** của chúng ta sẽ có 4 file như bên dưới

![Complete](/images/6.terraform/6.1-completecredentails.png)

#### CICD

Triển khai CICD như [Phần 4](4-cicd/)
