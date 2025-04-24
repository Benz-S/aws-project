
# AWS Lambda Stopinator Lab Documentation

> 📘 **Personal Documentation**  
> This document records my steps and key configurations from the AWS Lambda Stopinator lab, which automates stopping an EC2 instance every minute using AWS Lambda and EventBridge.

---

## 🗂️ Overview

In this lab, I:

1. Created a **Lambda function** (`myStopinator`)
2. Set up an **EventBridge** rule to trigger it every minute
3. Used an **IAM role** (`myStopinatorRole`)
4. Stopped an **EC2 instance** automatically through Lambda

---

## 🧑‍💻 Key Configurations

- **Lambda Function Name**: `myStopinator`
- **Runtime**: Python 3.11
- **IAM Role**: `myStopinatorRole`
- **EventBridge Rule**: `everyMinute` (runs every 1 minute)
- **Target EC2 Instance ID**: (retrieved and inserted into Lambda function)

---

## 🛠️ Step-by-Step Commands & Actions

### 1. Created Lambda Function
- Chose **Author from scratch**
- **Function name**: `myStopinator`
- **Runtime**: Python 3.11
- **Execution role**: Used existing role `myStopinatorRole`

---

### 2. Configured EventBridge Trigger
- **Trigger type**: EventBridge (CloudWatch Events)
- **Rule name**: `everyMinute`
- **Schedule expression**: `rate(1 minute)`

---

### 3. Lambda Function Code

```python
import boto3

region = 'us-east-1'  # Replace with actual region
instances = ['i-02b2c1882cd91fbdf']  # Replace with actual instance ID
ec2 = boto3.client('ec2', region_name=region)

def lambda_handler(event, context):
    ec2.stop_instances(InstanceIds=instances)
    print('stopped your instances: ' + str(instances))
```

- **Region**: `'us-east-1'`
- **Instance ID**: `'i-02b2c1882cd91fbdf'`

---

### 4. Deployed the Lambda Code
- Pasted the code into `lambda_function.py`
- Clicked **Deploy**

---

### 5. Monitored Function Invocations
- Navigated to the **Monitor** tab in Lambda
- Verified **invocation count** and **success/error rates**

---

### 6. Verified EC2 Stopping Automatically
- Observed EC2 instance `i-02b2c1882cd91fbdf` moving to **stopped** state every minute.
- Attempted to start the instance manually → Lambda stopped it again within a minute (as expected).

---

## 📝 Summary

- ✅ Created a Lambda function to stop an EC2 instance
- ✅ Automated invocation using EventBridge (every minute)
- ✅ Verified continuous enforcement of EC2 instance stopping
- ✅ Successfully completed the lab

---

## 🏁 Lab Completed

- Submitted the lab for grading.
- Received confirmation after 5 minutes due to event-trigger timing.

🎉 **Lab Success!**

---

© 2023 Amazon Web Services, Inc. All rights reserved.  
