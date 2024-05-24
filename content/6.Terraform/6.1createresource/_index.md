---
title: 'Initialize resources with Terraform'
date: '`r Sys.Date()`'
weight: 1
chapter: false
pre: ' <b> 6.1 </b> '
---

#### Prepare code

1. [Click here](https://github.com/hoangdat12/eks-workshop.git) to download code
2. Open code and go to the terraform file

#### Environment settings

1. Install **Terraform**

- Execute the command to install Terraform

```bash
sudo yum install -y yum-utils shadow-utils
sudo yum-config-manager --add-repo https://rpm.releases.hashicorp.com/AmazonLinux/hashicorp.repo
sudo yum -y install terraform
```

- Configure local to interact with AWS, execute command `aws configure`

#### Initialize Resource

1. Execute the command `terraform init` to initialize resources for **Terraform**

![Init terraform](/images/6.terraform/6.1-initterraform.png)

2. Execute the command `terraform validate` to check the syntax of Terraform configuration files

![Validate terraform](/images/6.terraform/6.1-validateterraform.png)

3. Execute the command `terraform plan` to view created resoures or change of the terraform infrastructure

![Plan terraform](/images/6.terraform/6.1-planterraform.png)

4. Execute the command `terraform apply -auto-approve` to apply to aws

![Apply terraform](/images/6.terraform/6.1-applyterraform.png)

5. Conduct testing in AWS Console

- Go to EC2 and check to see if there are 3 Instances, **jenkins instance** and 2 **nodes**
- Go to the EKS and check to **eks-cluster**

#### Preparation

1. Create **Github Access Token**

- Go to the [Github Setting](https://github.com/settings/profile) -> Scroll down and select **Developing Setting** -> Select **Personal access token** and click **Token (classic)**
- Select **Generate new token** and click **Generate new token (classic)**

![Github Access Token](/images/4.cicd/4.1-generategittoken.png)

- Enter `jenkins-access`
- Check all the buttons below
- Click **Generate token**

{{%notice tip%}}
To better understand Github Access Token configuration, we can refer to [here](https://docs.cloudbees.com/docs/cloudbees-ci-kb/latest/client-and-managed-controllers/github-user-scopes-and-organization-permissions-overview)
{{%/notice%}}

![Generate token](/images/4.cicd/4.1-generatetoken.png)

- Coppy **Token** was created
- Go to the **Jenkins Browser**
- At **Dashboard** select **Manages Jenkins**

![Manage Jenkins](/images/4.cicd/4.1-managejenkins.png)

- Select **Credentials**

![Credentials Jenkins](/images/4.cicd/4.1-credentials.png)

- Click **globals**
- Click **Add credentials**
- Select **Kind** is **Username with password**
- Enter Github **username** and **token** was created
- Enter ID `jenkins-git`
- Select **Create**

![Create Github Token](/images/4.cicd/4.1-creategittoken.png)

2. Create **Docker hub Access Token**

- Go to the [Docker hub](https://hub.docker.com/) -> Select **My Account**
- Select **Security**
- Select **New Access Token**

![Create Docker Access Token](/images/4.cicd/4.1-createdockertoken.png)

- Enter **Access token Description**
- Click **Generate**

![Generate Docker Token](/images/4.cicd/4.1-generatedockertoken.png)

- Copy **Docker Access Token** and do the same to create **Github Access Token** in Jenkins
- Go to the **Jenkins Browser**
- At **Dashboard** select **Manages Jenkins**
- Select **Credentials**
- Click **globals**
- Click **Add credentials**
- Select **Kind** is **Username with password**
- Nhập Docker **username** and **token** was created
- Enter ID `docker-crd`
- Select **Create**

![Complete Generate Docker Token](/images/4.cicd/4.1-completegeneratedockertoken.png)

3. Create **AWS_ACCESS_KEY_ID**

- Go to the **Jenkins Browser**
- At page **Dashboard** select **Manages Jenkins**
- Select **Credentials**
- Click **globals**
- Click **Add credentials**
- Select **Kind** is **Secret text**
- Enter Docker **secret** of aws_access_key_id in file.cvs
- Enter ID `AWS_ACCESS_KEY_ID`
- Select **Create**

![Create AWS access key](/images/6.terraform/6.1-awsaccesskey.png)

4. Tạo **AWS_SECRET_ACCESS_KEY**

- Go to the **Jenkins Browser**
- At page **Dashboard** select **Manages Jenkins**
- Select **Credentials**
- Click **globals**
- Click **Add credentials**
- Select **Kind** is **Secret text**
- Enter Docker **secret** of aws_secret_access_key in file.cvs
- Enter ID `AWS_SECRET_ACCESS_KEY`
- Chọn **Create**

![Create AWS access key](/images/6.terraform/6.1-awssecretaccesskey.png)

5. Complete

- After finishing, Our **Jenkins Credentails** will have 4 files as follows

![Complete](/images/6.terraform/6.1-completecredentails.png)

#### CICD

Implement CICD as [Chapter 4](4-cicd/)
