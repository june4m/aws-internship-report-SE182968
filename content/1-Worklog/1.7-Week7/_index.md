---
title: "Week 7 Worklog"
date: "2025-10-20"
weight: 7
chapter: false
pre: " <b> 1.7. </b> "
---

### Week 7 Goals:

- Systematize all knowledge acquired in the first half of the program.
- Deep dive into two main pillars: Security and Resiliency.
- Prepare a solid foundation for the Midterm Exam.

### Tasks to be implemented this week:

| Day   | Task                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   | Start Date | Completion Date | Reference                                                                     |
| :---- | :------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | :--------- | :-------------- | :---------------------------------------------------------------------------- |
| **2** | **Midterm Review:**<br><br>**Part 1: Secure Architecture**<br>- IAM (Identity & Access Management)<br>- KMS (Key Management Service)<br>- Security Group & NACLs<br>- Secrets Manager<br>- GuardDuty (Threat Detection)<br>- Shield & WAF (DDoS & Web App Protection)<br><br>**Part 2: Resilient Architecture**<br>- Core metrics to memorize (RTO/RPO)<br>- Multi-AZ RDS<br>- Disaster Recovery Strategies<br>- Auto Scaling and Load Balancing<br>- Route 53 and DNS<br>- AWS Backup | 20/10/2025 | 20/10/2025      | [Review Link](https://ruskicoder.github.io/fcj-midterm-2025/)                 |
| **3** | Self-study: Systematize security services and practice simulation labs                                                                                                                                                                                                                                                                                                                                                                                                                 | 21/10/2025 | 21/10/2025      |                                                                               |
| **4** | Self-study: Redraw High Availability (HA) and Disaster Recovery (DR) architecture diagrams                                                                                                                                                                                                                                                                                                                                                                                             | 22/10/2025 | 22/10/2025      |                                                                               |
| **5** | Take Practice Tests to review knowledge                                                                                                                                                                                                                                                                                                                                                                                                                                                | 23/10/2025 | 23/10/2025      | [practice](https://ezse.net/aws/certified-cloud-practitioner/examtopics.html) |
| **6** | Compile questions and review key notes                                                                                                                                                                                                                                                                                                                                                                                                                                                 | 24/10/2025 | 24/10/2025      | [Tips](https://ruskicoder.github.io/fcj-midterm-2025/6-additional-resources/) |

---

### Week 7 achievement: Consolidating Security Mindset and Resilient Architecture

- **Defense in Depth Mindset:**

  - Clearly distinguished the difference and scope between **Security Groups** (Stateful - Instance level) and **NACLs** (Stateless - Subnet level).
  - Deeply understood protection at different layers: **WAF** for applications (Layer 7), **Shield** for infrastructure (Layer 3/4), and **GuardDuty** for intelligent threat detection.
  - Mastered the importance of **KMS** and **Secrets Manager** in protecting sensitive data "at rest" and "in transit".

- **Resiliency & Recovery:**
  - Memorized and understood the nature of **RTO (Recovery Time Objective)** and **RPO (Recovery Point Objective)** metrics to select appropriate DR strategies (Backup & Restore, Pilot Light, Warm Standby, or Multi-site Active/Active).
  - Mastered **Multi-AZ RDS** mechanisms for High Availability compared to Read Replicas (used for scaling reads).
  - Understood the coordination between **Route 53, ELB, and Auto Scaling** to create a flexible, self-healing system capable of handling load effectively.
