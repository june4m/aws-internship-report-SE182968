---
title: "Week 12 Worklog"
date: "2025-11-24"
weight: 12
chapter: false
pre: " <b> 1.12. </b> "
---

### Week 12 Objectives:

- **Security & Monitoring:** Scan for security vulnerabilities and set up a system monitoring Dashboard.
- **Project Wrap-up:** Finalize documentation, perform final optimizations, and prepare presentation slides.

### Tasks to be implemented this week:

| Day | Task | Start Date | Completion Date | Reference |
| :--- | :--- | :--- | :--- | :--- |
| **2-3** | - Configure CloudWatch Alarms and Dashboard.<br>- Perform security review using AWS Trusted Advisor and GuardDuty. | 24/11/2025 | 25/11/2025 | |
| **4-5** | **Optimization & Packaging:**<br>- Review and delete redundant resources for cost optimization.<br>- Write operational documentation (Runbook).<br>- Draft final report slides (Architecture, Challenges, Lessons Learned). | 26/11/2025 | 27/11/2025 | |

---

### Week 12 Achievements: System Finalization & Handover Readiness

#### 1. Performance Evaluation & Tuning

- **Monitoring:**
  - Established a **CloudWatch Dashboard** to visually display key metrics: CPU Utilization, Request Count, Database Connections, Error Rate (4xx, 5xx).
  - Configured **SNS Alerts** to send email notifications to Admins when system issues occur.

#### 2. Optimization & Advanced Security

- **Security:**
  - Enabled **WAF (Web Application Firewall)** to block common attacks (SQL Injection, XSS).
  - Reviewed IAM Roles and revoked unnecessary privileges.
- **Cost:** Based on data from the 2-week pilot run, resized Instances from t3.medium to t3.micro for the Dev environment to save costs without impacting performance.

#### 3. Project Completion

- **Documentation:** Completed the project documentation suite including: Updated Architecture Diagram, Redeployment Guide, and Cost Report.
- **Lessons Learned:** Gained insights into handling "Cold Start" issues during Auto Scaling and the importance of accurate Health Check configuration.