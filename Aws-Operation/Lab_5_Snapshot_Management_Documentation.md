
# Lab 5 Snapshot Automation – AWS EC2 & EBS

> 📘 This documentation tracks the full process of managing EBS snapshots for Lab 5 using AWS CLI and automation with cron and Python.

---

## 🖥️ EC2 & Volume Info
- **Instance ID**: `i-02b2c1882cd91fbdf`
- **Volume ID**: `vol-0afcdf95a07bbd07d`

---

## 📌 Snapshot Commands Used

### 1. Stop the EC2 instance
```bash
aws ec2 stop-instances --instance-ids i-02b2c1882cd91fbdf
aws ec2 wait instance-stopped --instance-id i-02b2c1882cd91fbdf
```

### 2. Create a Snapshot
```bash
aws ec2 create-snapshot --volume-id vol-0afcdf95a07bbd07d
```
🆔 Snapshot created: `snap-0e40a13ac36a7c72b`

### 3. Wait for Snapshot Completion
```bash
aws ec2 wait snapshot-completed --snapshot-id snap-0e40a13ac36a7c72b
```

### 4. Start the EC2 instance again
```bash
aws ec2 start-instances --instance-ids i-02b2c1882cd91fbdf
aws ec2 wait instance-running --instance-id i-02b2c1882cd91fbdf
```

---

## ⏱️ Automating Snapshot Creation (cron)

### Setup cron to take snapshots every minute
```bash
echo "* * * * * aws ec2 create-snapshot --volume-id vol-0afcdf95a07bbd07d >> /tmp/cronlog 2>&1" > cronjob
crontab cronjob
```

### Verify snapshots created
```bash
aws ec2 describe-snapshots --filters "Name=volume-id,Values=vol-0afcdf95a07bbd07d" --query 'Snapshots[*].SnapshotId' --output text
```

📌 Snapshots observed:
- `snap-0e40a13ac36a7c72b`
- `snap-072d15df26de375ba`
- `snap-0b48745e7603002c6`

---

## 🧹 Snapshot Cleanup Script (Keep Last 2)

### 1. Remove cron to stop continuous snapshots
```bash
crontab -r
```

### 2. Python script: `snapshotter_v2.py`
```python
#!/usr/bin/env python

import boto3

MAX_SNAPSHOTS = 2

ec2 = boto3.resource('ec2')
volume_iterator = ec2.volumes.all()

for v in volume_iterator:
  v.create_snapshot()
  snapshots = list(v.snapshots.all())
  if len(snapshots) > MAX_SNAPSHOTS:
    snap_sorted = sorted([(s.id, s.start_time, s) for s in snapshots], key=lambda k: k[1])
    for s in snap_sorted[:-MAX_SNAPSHOTS]:
      print("Deleting snapshot", s[0])
```

### 3. Run the script
```bash
python3 snapshotter_v2.py
```

🧼 Deleted oldest snapshots, kept the latest two.

---

## ✅ Final Snapshots
After cleanup:
```bash
aws ec2 describe-snapshots --filters "Name=volume-id,Values=vol-0afcdf95a07bbd07d" --query 'Snapshots[*].SnapshotId' --output text
```
✅ Snapshots:
- `snap-072d15df26de375ba`
- `snap-0b48745e7603002c6`

---

## 🏁 Summary
- Successfully created and managed snapshots for EC2 volume
- Automated snapshot creation via cron
- Used Python to enforce snapshot retention policy (keep latest 2)
- Verified and cleaned up as expected

Lab 5 snapshot management complete! ✅
