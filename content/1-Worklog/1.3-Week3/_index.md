---
title: "Week 3 Worklog"
date: "2025-09-22"
weight: 3
chapter: false
pre: " <b> 1.3. </b> "
---

### Week 3 Goals:

- **Mastering Compute Fundamentals:** Deep dive into Amazon EC2 architecture, understanding the distinct use cases for different Instance Types.
- **Network Security:** Implementing robust firewall rules using Security Groups to control traffic flow.
- **Cross-Platform Deployment:** Hands-on practice deploying instances on both Linux and Windows environments using SSH and RDP.
- **Application Setup:** Configuring web server environments (LAMP/XAMPP) to simulate real-world application hosting.

### Tasks to be implemented this week:

| Day   | Task                                                                                                                                                                                           | Start Date | Completion Date | Reference                  |
| :---- | :--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | :--------- | :-------------- | :------------------------- |
| **2** | **Theory & Network Security:**<br>- Research EC2 core concepts (Instance Types, AMI, EBS, User Data).<br>- Configure VPC Security Groups (Inbound/Outbound rules).                             | 22/09/2025 | 22/09/2025      | Module 02-Lab03-04.1       |
| **3** | **Basic Operations:**<br>- Practice launching EC2 instances in public subnets.<br>- Establish connectivity via SSH key pairs.                                                                  | 22/09/2025 | 23/09/2025      | Introduction to Amazon EC2 |
| **4** | **Linux Environment Deep Dive:**<br>- Deploy Amazon Linux EC2.<br>- **Advanced Lab:** Recover access to an instance after losing the Key Pair.<br>- Install LAMP Stack (Apache, MariaDB, PHP). | 24/09/2025 | 24/09/2025      | EC2 Linux Guide            |
| **5** | **Windows Environment Setup:**<br>- Deploy Windows Server instance.<br>- Connect via RDP (Remote Desktop Protocol).<br>- Install XAMPP solution.                                               | 25/09/2025 | 25/09/2025      | EC2 Windows Guide          |

---

### Week 3 Achievement: Mastering EC2 Infrastructure, Security Configuration & Web Server Deployment

#### 1. Deep Dive into EC2 Architecture & Components

- **Compute Service Model:** Gained a solid understanding of EC2 as an IaaS solution for on-demand scalable computing.
- **Core Components:** Analyzed the ecosystem required to run an instance effectively:
  - **Instance Types:** Selection based on workload (CPU, Memory, Storage optimized).
  - **AMI (Amazon Machine Image):** Using pre-configured templates for rapid deployment.
  - **Storage:** Distinguished between EBS (persistent block storage) and Instance Store.
  - **Bootstrapping:** Utilized **User Data** to run automated scripts upon instance initialization.

#### 2. Network Security Implementation

- **Security Group Management:**
  - Configured Security Groups as a **stateful firewall** at the instance level.
  - Defined precise **Inbound rules** to allow necessary services (SSH port 22, HTTP port 80, RDP port 3389).
  - Understood the default behavior of Outbound rules and connection tracking.

#### 3. Linux Administration & Troubleshooting

- **Connectivity:** Mastered SSH connections using private keys (`.pem`).
- **Disaster Recovery (Key Pair Loss):** Successfully performed a manual recovery of a "lost" instance by:
  1. Creating a fresh Key Pair.
  2. Utilizing `PuTTYgen` to extract the Public Key.
  3. Injecting the new Public Key into the instance's `user-data` script (`cloud-config`) while the instance is stopped.
  4. Validating access restoration after a reboot.
- **Web Stack Deployment:** Successfully installed and configured the LAMP stack (Linux, Apache, MariaDB, PHP) to host a test web page.

#### 4. Windows Server Administration

- **Access Management:** Retrieved the Administrator password by decrypting it with the Key Pair file.
- **Remote Access:** Used Microsoft Remote Desktop (RDP/mstsc) for GUI-based management.
- **Application Layer:** Deployed XAMPP to verify web server functionality on a Windows environment.

#### 5. Challenges & Solutions

- **Issue 1 (Connectivity):** SSH connection timed out initially.
  - _Solution:_ Audited the Security Group and added an Inbound Rule for port 22 from "My IP".
- **Issue 2 (Software Installation):** Apache installation failed on **Amazon Linux 2023** because the `amazon-linux-extras` command is deprecated.
  - _Solution:_ Researched documentation and switched to using `dnf` and specific distro-stream repositories compatible with AL2023.
