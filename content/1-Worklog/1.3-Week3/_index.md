---
title: "Week 3 Worklog"
date: "2025-09-22"
weight: 1
chapter: false
pre: " <b> 1.3. </b> "
---

{{% notice warning %}}
⚠️ **Note:** The information below is for reference purposes only. Please **do not copy verbatim** for your report, including this warning.
{{% /notice %}}

### Week 3 Goals:

### Tasks to be implemented this week:

| Day   | Task                                                                                                                                                                                                                                                                                                                                                                 | Start Date | Completion Date | Reference                                            |
| :---- | :------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | :--------- | :-------------- | :--------------------------------------------------- |
| **2** | Setup Transit Gateway                                                                                                                                                                                                                                                                                                                                                | 22/09/2025 | 22/09/2025      | [Transit Gateway](https://000020.awsstudygroup.com/) |
| **3** | - Research EC2 theory<br>- Instance type<br>- AMI/ Backup/ Keypair<br>- Elastic Block Store (EBS)<br>- Instance Store<br>- User data/ Meta data<br>- EC2 Auto Scaling<br>- EFS/FSx, Lightsail, MGN<br>- Practice Lab 13                                                                                                                                              | 23/09/2025 | 23/09/2025      | [EC2](https://000004.awsstudygroup.com/)             |
| **4** | - Lab 20 Implementation: Successfully deployed AWS Transit Gateway to establish a central hub for inter-VPC connectivity<br> + Troubleshooting: Encountered deployment failures due to an outdated CloudFormation template. I diagnosed the issue and patched the YAML file by updating the EC2 instance type to t3.micro, ensuring successful stack creation.<br> - | 24/09/2025 | 24/09/2025      |                                                      |
| **5** | Practice Module 4 Lab 57                                                                                                                                                                                                                                                                                                                                             | 25/09/2025 | 25/09/2025      |                                                      |
| **6** |                                                                                                                                                                                                                                                                                                                                                                      | 26/09/2025 | 26/09/2025      |                                                      |

---

### Week 3 achievement: Solid Compute Foundation and Diverse Storage Management

- **Advanced Network Connectivity:** Finalized Transit Gateway (TGW) configuration, reinforcing the hub-and-spoke network model for future scalability.
- **Amazon EC2 & Scalability:**
  - Mastered EC2 architecture, from selecting Instance Types to automation mechanisms using User Data.
  - Specifically, mastered the deployment process of EC2 Auto Scaling Groups (ASG) combined with ALB, ensuring the system has High Availability (HA) and Scalability based on load.
- **Cloud Storage Solutions:** Clearly distinguished and learned when to use EBS (Block Storage for OS/DB), Instance Store (Temporary Cache), and EFS (Shared File System).
