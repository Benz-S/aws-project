
# Lab 5 Documentation: Managing Storage

> ⚠️ **Personal Use Only**  
> This document is a personal record of my work in AWS Module 8: Lab 5 - Managing Storage. It includes steps, commands, and key observations from both the Task and Challenge sections.

---

## 📅 Overview
This lab focused on managing Amazon EC2 storage using EBS snapshots and Amazon S3 buckets. It was divided into two parts:

1. **Task Section**: Set up resources, use CLI for snapshots, and upload files to S3.
2. **Challenge Section**: Sync files with Amazon S3, enable versioning, and recover deleted versions.

---

## 🎯 Objectives
- Create and maintain snapshots for EC2 instances using AWS CLI
- Upload, sync, and manage files with Amazon S3 using AWS CLI

---

## ⏱ Duration
Estimated: 45 minutes

---

## ✅ Task Section

### Task 1: Creating and Configuring Resources
- **Created an S3 bucket** using S3 Console
  - Unique bucket name noted as `s3-bucket-name`
- **Attached IAM Role** `S3BucketAccess` to EC2 instance "Processor"
  - Gave permissions to interact with S3

### Task 2: Taking Snapshots via AWS CLI

#### Connected to EC2 (Command Host)
- **SSH via Terminal (Linux/Mac)** or **PuTTY (Windows)** using `.pem` or `.ppk` key

#### Snapshot Preparation
- Obtained Volume ID:
  ```bash
  aws ec2 describe-instances --filter 'Name=tag:Name,Values=Processor' --query 'Reservations[0].Instances[0].BlockDeviceMappings[0].Ebs.{VolumeId:VolumeId}'
  ```
- Stopped instance:
  ```bash
  aws ec2 stop-instances --instance-ids INSTANCE-ID
  aws ec2 wait instance-stopped --instance-id INSTANCE-ID
  ```

#### Created Snapshot:
```bash
aws ec2 create-snapshot --volume-id VOLUME-ID
aws ec2 wait snapshot-completed --snapshot-id SNAPSHOT-ID
```

#### Restarted instance:
```bash
aws ec2 start-instances --instance-ids INSTANCE-ID
aws ec2 wait instance-running --instance-id INSTANCE-ID
```

#### Scheduled Recurring Snapshots (cron job)
```bash
echo "* * * * *  aws ec2 create-snapshot --volume-id VOLUME-ID >> /tmp/cronlog 2>&1" > cronjob
crontab cronjob
```

#### Verified Snapshots:
```bash
aws ec2 describe-snapshots --filters "Name=volume-id,Values=VOLUME-ID"
```

#### Removed cron:
```bash
crontab -r
```

#### Ran snapshot cleanup script:
```bash
python3.8 snapshotter_v2.py
```
- Kept only **2 most recent snapshots**

---

## 🧪 Challenge: Sync Files with Amazon S3

### Step 1: Download & Unzip Files
```bash
wget https://aws-tc-largeobjects.s3-us-west-2.amazonaws.com/CUR-TF-200-RESOPS/lab5vocareum/files.zip
unzip files.zip
```

### Step 2: Enable Versioning
```bash
aws s3api put-bucket-versioning --bucket S3-BUCKET-NAME --versioning-configuration Status=Enabled
```

### Step 3: Initial Sync
```bash
aws s3 sync files s3://S3-BUCKET-NAME/files/
```

### Step 4: Delete a File Locally
```bash
rm files/file1.txt
```

### Step 5: Sync with Delete
```bash
aws s3 sync files s3://S3-BUCKET-NAME/files/ --delete
```

### Step 6: Restore File using Versioning
- List object versions:
```bash
aws s3api list-object-versions --bucket S3-BUCKET-NAME --prefix files/file1.txt
```
- Get old version:
```bash
aws s3api get-object --bucket S3-BUCKET-NAME --key files/file1.txt --version-id VERSION-ID files/file1.txt
```
- Re-sync to S3:
```bash
aws s3 sync files s3://S3-BUCKET-NAME/files/
```

---

## ⚠️ Common Issues & Fixes

| Issue | Cause | Solution |
|-------|-------|----------|
| Snapshot returns error | Instance still running | Used `stop-instances` and `wait` command before snapshot |
| `--delete` gives parsing error | CLI version mismatch | Ignored error, file still deleted successfully |
| Permission denied on `.pem` file | Wrong file mode | Used `chmod 400 labsuser.pem` |

---

## 📝 Summary
- Used AWS CLI to automate snapshot creation & management
- Enabled S3 versioning and restored deleted content
- Practiced file syncing between EC2 and S3
- Automated snapshot cleanup using Python & Boto3

**Lab Completed Successfully** ✅
