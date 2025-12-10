---
title: "Week 11 Worklog"
date: "2025-11-17"
weight: 11
chapter: false
pre: " <b> 1.11. </b> "
---

### Week 11 Objectives:

- **Deploy Core Services:** Configure Compute and Database resources.
- **Automation (DevOps):** Build a CI/CD pipeline to automate code building and deployment to the AWS environment.

### Tasks to be implemented this week:

| Day     | Task                                                                                                                                                                                                                                                           | Start Date | Completion Date | Reference |
| :------ | :------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | :--------- | :-------------- | :-------- |
| **2-3** | **Compute & DB Configuration:**<br>- Initialize RDS with Single-AZ.<br>- Connect to MySQL.                                                                                                                                                                     | 17/11/2025 | 18/11/2025      |           |
| **4-5** | **Build CI/CD Pipeline:**<br>- Setup Repo on CodeCommit/GitHub.<br>- Configure CodeBuild to package the application.<br>- Use CodeDeploy/CodePipeline to automatically deploy code to EC2 Instances.<br>- Verify the code deployment flow from Local to Cloud. | 19/11/2025 | 20/11/2025      |           |

---

### Week 11 Achievements: Operational System & Automated Pipeline

#### 1. High Availability Application Deployment

- **Database:** RDS is operating stably in Multi-AZ mode, ensuring data safety in case of failure in one AZ. Successfully connected the App Server to the DB.

#### 2. DevOps Process (CI/CD)

- **Complete Pipeline:** Successfully built a CI/CD (Continuous Integration/Continuous Deployment) flow.
  - **Source:** Code is pushed to GitHub/CodeCommit.
  - **Build:** Automatically install dependencies and package artifacts.
  - **Deploy:** Automatically update the new version to all EC2 instances in the ASG without service interruption (Zero Downtime Deployment - using Rolling Update strategy).
