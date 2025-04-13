
# Lab 4 Documentation: Working with Amazon EBS

> ⚠️ **Personal Use Only**  
> This document is part of my AWS learning journey. It includes notes and configurations I used during Lab 4 for personal reference. Please do not reuse this as your own lab submission.

---

## 📅 Lab Overview
This lab introduced the use of **Amazon Elastic Block Store (EBS)** with Amazon EC2. It covered the process of creating an EBS volume, attaching it to an instance, formatting it, creating snapshots, and restoring data.

---

## 📌 Lab Objectives
- Create an Amazon EBS volume
- Attach and mount it to an EC2 instance
- Create a snapshot of the volume
- Restore a new volume from the snapshot
- Mount the restored volume and verify data

---

## ⏱ Duration
Approximately **30 minutes**

---

## ✅ Tasks Performed

### Task 1: Create a New EBS Volume
- Created a **1 GiB gp2 volume** in the same Availability Zone as the EC2 instance
- Tagged as `Name=My Volume`

### Task 2: Attach the Volume to EC2 Instance
- Attached `My Volume` to the instance under **Device** `/dev/sdf`
- EC2 Console showed it as **in-use**

### Task 3: Connect to EC2 Instance
- Used **EC2 Instance Connect** from the AWS console to open a terminal session

### Task 4: Create and Configure File System
- Checked storage with `df -h`
- Formatted volume with: `sudo mkfs -t ext3 /dev/sdf`
- Created mount directory: `sudo mkdir /mnt/data-store`
- Mounted: `sudo mount /dev/sdf /mnt/data-store`
- Added to `/etc/fstab` for persistence:
  ```bash
  echo "/dev/sdf /mnt/data-store ext3 defaults,noatime 1 2" | sudo tee -a /etc/fstab
  ```
- Verified mount and created file:
  ```bash
  sudo sh -c "echo some text has been written > /mnt/data-store/file.txt"
  cat /mnt/data-store/file.txt
  ```

### Task 5: Create an Amazon EBS Snapshot
- Selected `My Volume` and created a snapshot
- Tagged as `Name=My Snapshot`
- Deleted file from EC2:
  ```bash
  sudo rm /mnt/data-store/file.txt
  ls /mnt/data-store/  # Confirmed deletion
  ```

### Task 6: Restore the Amazon EBS Snapshot
- Created a volume from `My Snapshot` in same AZ
- Tagged as `Name=Restored Volume`
- Attached to EC2 instance as `/dev/sdg`
- Mounted it:
  ```bash
  sudo mkdir /mnt/data-store2
  sudo mount /dev/sdg /mnt/data-store2
  ls /mnt/data-store2/  # Confirmed file.txt was restored
  ```

---

## ⚠️ Mistakes & Fixes

| Step         | Issue                                     | Solution                                           |
|--------------|--------------------------------------------|----------------------------------------------------|
| Volume Mount | Initially forgot to format before mount   | Used `mkfs -t ext3` before mounting                |
| Snapshot     | Took snapshot before tagging volume       | Edited tags after snapshot was created             |
| Device Name  | Mounted as `/dev/sdf` but appeared as `/dev/xvdf` | Used `lsblk` to confirm correct name          |

---

## 📝 Submission & Results
- Submitted lab using AWS lab interface
- Confirmed all tasks completed successfully
- Snapshot, restore, and mount tested and verified

---

## 🎉 Conclusion
Successfully completed:
- EBS volume creation
- EC2 volume attachment
- File system setup and file write
- Snapshot and restore
- Verification of restored data on new volume

**Lab Complete!**
