
# RMIT University - ICTCLD507 Task 1 Deployment Documentation

## Project Name: EC2 and Lambda Deployment
**Consultant Name:** Benz S
**Environment:** AWS Cloud  
**Date:** 17.04.2025  

---

## 1. Overview
This document outlines the steps to deploy Amazon EC2 instances (via AWS Management Console and AWS CLI) and configure an AWS Lambda function that automates instance management. It includes evidence from both deployments, demonstrating practical skills in provisioning and automation within AWS.

---

## 2. Services Used
- **Amazon EC2 (Elastic Compute Cloud):** For launching and managing virtual server instances.
- **AWS CLI:** For automating EC2 instance deployment.
- **Amazon Systems Manager (SSM) Parameter Store:** Retrieves the latest Amazon Linux AMI ID.
- **AWS Lambda:** Serverless function to automate EC2 instance stopping.
- **Amazon EventBridge:** Triggers Lambda based on scheduled intervals.
- **Amazon CloudWatch:** Monitors Lambda metrics and logs.
- **AWS Identity and Access Management (IAM):** Manages permissions and roles.

---

## 3. Configuration Steps

### Network Setup (Pre-existing Lab Environment)
- **VPC (Virtual Private Cloud):** `VPC-A` (10.0.0.0/16)
- **Subnet:** `Public Subnet` (associated with VPC-A)
- **Internet Gateway (IGW):** Attached to VPC-A for internet access.
- **Route Table:** Configured for outbound traffic through IGW.

### EC2 Deployment

#### Instance 1: Bastion Server (via AWS Management Console)
- Name: `Bastion Server`
- AMI: Amazon Linux 2023
- Instance Type: `t2.micro`
- Key Pair: `vockey`
- Network: VPC-A, Public Subnet, Auto-assign public IP enabled
- Security Group: `Bastion security group` (SSH port 22 allowed)
- IAM Role: `Bastion-Role`
- Storage: Default 8 GiB EBS

#### Instance 2: Web Server (via AWS CLI)
1. Connect to the Bastion Server.
2. Retrieve:
   - **AMI ID**: via SSM Parameter Store.
   - **Subnet ID**: Public Subnet.
   - **Security Group ID**: WebSecurityGroup.
3. Download the User Data script to install Apache and deploy the web app.
4. Launch instance via CLI:
```bash
aws ec2 run-instances --image-id $AMI --subnet-id $SUBNET --security-group-ids $SG --user-data file:///home/ec2-user/UserData.txt --instance-type t2.micro --tag-specifications 'ResourceType=instance,Tags=[{Key=Name,Value=Web Server}]'
```
5. Verify instance status and test web server via its public DNS.

### Lambda Deployment
1. **Create IAM Role**: `LambdaEC2StopRole` with `AmazonEC2FullAccess`.
2. **Create Lambda Function**: `myStopinator`.
   - Runtime: Python 3.x
   - Code:
```python
import boto3

def lambda_handler(event, context):
    ec2 = boto3.client('ec2')
    instances = ['<INSTANCE_ID>']
    ec2.stop_instances(InstanceIds=instances)
    print('Stopped your instances: ' + str(instances))
```
3. **Add Trigger**: EventBridge rule `everyMinute` with `rate(1 minute)`.
4. **Monitor Function**: View metrics and logs in CloudWatch.

---

## 4. Security Best Practices Implemented
- **IAM Roles:** Scoped permissions for Lambda (`LambdaEC2StopRole`) and Bastion (`Bastion-Role`).
- **Security Groups:**
  - Bastion Server: SSH only (port 22).
  - Web Server: HTTP only (port 80).
- **Key Pair Authentication:** SSH access with PEM/PPK keys.

---

## 5. Troubleshooting
- **SSH Issues:** Validate key format and security group rules.
- **Web Server Not Accessible:** Confirm instance state and HTTP rules.
- **Lambda Execution Errors:** Check CloudWatch logs and IAM permissions.

---

## 6. Screenshots (Evidence)
### EC2 Deployment:
- VPC-A with subnet, IGW, and route table.
- Key pair (`Benz`) creation.
- Security group rules (SSH and HTTP).
- Instance `WebServer(Benz)` running (IP: 18.232.85.242).
- Instance boot screenshot.
- System log showing Apache installation.

### Lambda Deployment:
- EventBridge trigger `everyMinute`.
- Lambda function code (`myStopinator`).
- CloudWatch metrics and logs for Lambda invocations.

*Refer to attached screenshots for detailed evidence.*
