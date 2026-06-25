# 🌐 Project: Developing AWS Architecture Using AWS CloudFormation

![AWS](https://img.shields.io/badge/AWS-%23FF9900.svg?style=for-the-badge&logo=amazon-aws&logoColor=white)

## Project Overview

This learning project's aim is to design, deploy and document a Web application using AWS CloudFOrmation following Infrastructure as Code (IaC) principles. The project consists of three phases. In phase 1 the network infrastructure will be set up. In phase 2 the network will be extended with an Amazon EC2 instance hosting a web server. In phase 3 an AWS Auto Scaling and an Application Load Balancer will be added to the infrastructure.

## 📁 Repository Structure

```
aws-cloudformation/
├── diagrams/                    # Folder with all diagrams
├── screenshots/                 # Folder with screenshots
    ├── 01-netork/
        ├── CLI/                 # Folder with CLI screenshots
        └── Management-Console/  # Folder with Management Console screenshots
    └── 02-webserver/
        ├── CLI/                 # Folder with CLI screenshots
        └── Management-Console   # Folder with Management Console screenshots
├── templates/
    ├── network-security.yaml    # Yaml file the network infrastructure
    └── wordpress-server.yaml    # Yaml file for the web server
├── .gitignore                   # Gitignore file
└── README.md                    # This file
```

## Phase 1: Network Infrastructure

### 🚀 Objective

The objective of this project is to design, deploy, and document a highly available AWS infrastructure using AWS CloudFormation following Infrastructure as Code (IaC) principles rather than manual configuration each single resource through the AWS console. The stack was deployed and validated using both the AWS Management Console and the AWS CLI. The infrastructure consists of:

* a VPC
* 2 public and 2 private subnets across 2 Availability Zones
* an Internet Gateway
* a public and a private route table
* a security group allowing HTTP, HTTPS, and SSH traffic

### 🏗️ Architecture Diagram

![Architecture Diagram](diagrams/phase-1.png)

### 📋 Prerequisites

You need the following for deploying this infrastructure:

* basic knowledge of AWS
* an AWS account
* AWS CLI (v2.34.38 or later) - [Installation Guide](https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html)
* git for cloning the repository

### 🔧 Deployment Steps

#### 1. Configure AWS CLI

Configure AWS CLI using the command:
```bash
aws configure
```
Enter your credentials.

#### 2. Clone the Repository
```bash
git clone https://github.com/HamsterHugo/aws-cloudformation.git
cd aws-cloudformation
```

#### 3a. Deployment - AWS CLI

1. Create the stack:
```Bash
aws cloudformation create-stack --stack-name network-infrastructure-cli --template-body file://template.yaml
```

2. Check the status:
```Bash
aws cloudformation describe-stacks --stack-name network-infrastructure-cli
```
Repeat the command until `StackStatus` shows `CREATE_COMPLETE`.

#### 3b. Deployment - AWS Management Console

1. Login to AWS
2. Check the region. It has to be `us-east-1`.
3. Enter `CloudFormation` into the search box and select it.
4. Click on the button `Create Stack`.
5. Choose `With new Resources (Standard)` .
6. In the section `Prerequisite - Prepare template` choose `Template is ready`.
7. In the section `Specify template` choose `Upload a template file` and select the file `template.yaml`.
8. Click on the button `Next`.
9. Enter `network-infrastructure` for the field `Stack name`.
10. Click on the button `Next`.
11. Keep the defaults for `Stack Options`.
12. Click on the button `Next`.
13. Check the summary and finally click on the button `Submit`.
14. Now AWS creates the stack. Click on the `Events` tab and wait until the status of each reasource changed from `CREATE_IN_PROGRESS` to `CREATE_CPMPLETE`.

#### 4a. Validation - AWS CLI

1. To check the creation of the resources enter the following command:
```Bash
aws cloudformation describe-stack-resources --stack-name network-infrastructure-cli --no-cli-pager
```
![Screenshot-2](screenshots/01-network/CLI/02-stack-resources.png)

2. To check the output enter the following command:
```Bash
aws cloudformation describe-stacks --stack-name network-infrastructure-cli --query "Stacks[0].Outputs"
```
![Screenshot-3](screenshots/01-network/CLI/03-outputs.png)
Take a note of the VPC id (`vpc-...`).

3. To check the creation of the subsnets enter the following command:
```
Bash
aws ec2 describe-subnets --filters "Name=vpc-id,Values=<your-vpc-id> -no-cli-pager"
```
![Screenshot-4](screenshots/01-network/CLI/04-subnets.png)

#### 4b. Validation - AWS Management Console

1. View your stack and select the tab `Resources`:
![Screenshot-1](screenshots/01-network/Management-Console/01-stack-resources.png)

2. Select the tab `Outputs`:
![Screenshot-2](screenshots/01-network/Management-Console/02-stack-outputs.png)

3. Enter `VPC` in the search box, select it and click on `Your VPCs`:
![Screenshot-3](screenshots/01-network/Management-Console/03-vpc-overview.png)

4. Click on `Subnets` in left navigation pane:
![Screenshot-4](screenshots/01-network/Management-Console/04-subnets-overview.png)

5. Click on `Route Tables` in the left navigation pane. In the section `Public RT` choose `Routes`:
![Screenshot-5](screenshots/01-network/Management-Console/05-public-route-table-routes.png)

6. Select the tab `Subnet Associations`:
![Screenshot-6](screenshots/01-network/Management-Console/06-public-route-table-associations.png)

7. Click on `Internet Gateways` in the left navigation pane:
![Screenshot-7](screenshots/01-network/Management-Console/07-internet-gateway.png)

8. Click on `Security Groups` in the left navigation pane. Select the Security Group `Webserver-SG`. Choose the tab `Inbound Rules`:
![Screenshot-8](screenshots/01-network/Management-Console/08-security-group-inbound-rules.png)

#### 5a. Cleanup - CLI

1. To delete the whole stack enter the following command:
```Bash
aws cloudformation delete-stack --stack-name network-infrastructure-cli
```

2. Wait a while. To check the correct deletion enter the follwoing command:
```Bash
aws cloudformation describe-stacks --stack-name network-infrastructure-cli
```
If you get an error message the deletion was successful.
![Screenshot-5](screenshots/01-network/CLI/05-stack-deletion.png)

#### 5b. Cleanup - AWS Management Console

1. Enter `Cloudformation` into the search box and select it which opens the CloudFormation console.
2. Select the stack and click on the button `Delete`.
3. In the conformation window click on `Delete`.
4. You can folllow the deletion process in the tab `Events`.
5. View the stack list. You should see the status `DELETE_COMPLETE`.
![Screenshot-9](screenshots/01-network/Management-Console/09-stack-deleted-confirmation.png)

## Phase 2: Web Server Deployment and WordPress Installation

### 🚀 Objective

Deploy an Amazon EC2 instance within the Public Subnet created in Phase 1 and install WordPress to create a functional web application environment.

### 🏗️ Architecture Diagram

### 📋 Prerequisites

### 🔧 Deployment Steps

### Validation

### Cleanup