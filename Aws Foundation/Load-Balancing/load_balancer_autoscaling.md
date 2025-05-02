# Load Balancer and Auto Scaling - Documentation

**Company Name:** RMIT  
**Project Name:** Assessment Task 2 (Load Balancer and Auto Scaling)  
**Consultant Name:** Benz Seal  
**Environment:** AWS Cloud  
**Date:** 2.05.2025

---

## Overview

This documentation explains how to plan and set up a web infrastructure that can be deployed and work even when something goes wrong on AWS. It shows how to set up Elastic Load Balancing and EC2 Auto Scaling so that traffic is spread out and computing power is changed based on demand.

---

## 1. Interfaces and Tools Required

- **AWS Management Console** – for visual setup of all services  
- **EC2** – to launch, monitor, and manage instances  
- **Elastic Load Balancing (ELB)** – to distribute traffic  
- **Auto Scaling Groups (ASG)** – to automatically scale EC2 capacity  
- **Amazon CloudWatch** – to monitor metrics and trigger scaling  
- **User Data (Bash script)** – to auto-install Apache and simulate load  

---

## 2. Configuration Steps

### A. Network Setup

- Create a new VPC (10.0.0.0/16)
- Create:
  - 2 public subnets (e.g., 10.0.1.0/24, 10.0.2.0/24)
  - 2 private subnets (e.g., 10.0.3.0/24, 10.0.4.0/24)
- Create and attach an Internet Gateway
- Setup route tables (public → IGW)
- Create Web Security Group:
  - Allow HTTP (80) and SSH (22) from your IP

### B. Launch EC2

- Launch instance
- Name: Benz-Web
- OS: Amazon Linux AMI
- Type: t3.small
- Attach to public subnet in VPC
- User Data:
```bash
#!/bin/bash
yum update -y
yum install -y httpd
systemctl start httpd
systemctl enable httpd
echo "Web Server 1" > /var/www/html/index.html
```

### C. Create AMI

- Create image from Web Server 1  
- Name: Benz-Web  
- Attach the SG  

### D. Create Target Group

- Name: Benz-TG  
- Type: Instances  
- VPC: Benz-LAB  
- Protocol: HTTP  

### E. Create Load Balancer

- Type: Application Load Balancer  
- Name: BenzLB  
- Subnets: Public Subnet 1 & 2  
- Security Group: Web Security Group  
- Forward HTTP:80 traffic to Benz-TG  

### F. Create Launch Template

- Name: Benz-Template  
- AMI: WebServerAMI  
- Instance type: t2.micro  
- Security Group: Web Security Group  
- Enable Detailed CloudWatch monitoring  

### G. Create Auto Scaling Group

- Name: Benz Auto Scaling  
- Launch template: Benz-Template  
- VPC: Benz-LAB  
- Subnets: Private Subnet 1 & 2  
- Attach to target group: Benz-TG  
- Desired = 2, Min = 2, Max = 6  
- Scaling policy:
  - Target tracking: CPU Utilization = 60%  
- Enable group metrics  
- Tag: `Name = Lab Instance`  

---

## 3. Test Plan

| Step | What to Verify                     | How to Test                  |
|------|-----------------------------------|------------------------------|
| 1    | Auto Scaling creates 2 instances  | EC2 dashboard shows 2 running |
| 2    | Scaling triggers with high CPU    | Use stress tool or user data |
| 3    | New instances appear              | Auto Scaling adds instances |

---

## 4. Troubleshooting Methodology

| Issue                          | Solution                                             |
|--------------------------------|------------------------------------------------------|
| Load Balancer not serving      | Check SG rules, target health, HTTP service status   |
| No new instances during load   | Confirm scaling policy, CloudWatch alarms, user data |
| User data not applied          | Rebuild launch template with correct syntax          |
| Can't access EC2 (SSH)         | Add bastion/public subnet or use User Data testing   |

---

## Additional Notes

- Two additional instances were successfully created after stress test
- CloudWatch alarm spiked during the test, verifying scaling trigger
