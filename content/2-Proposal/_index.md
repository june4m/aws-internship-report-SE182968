---
title: "Proposal"
date: "2025-09-08"
weight: 2
chapter: false
pre: " <b> 2. </b> "
---

# Dai Viet Blood Donation & Emergency System (DaiVietBlood)

**Implemented by:** Skyline Team – FPT University Ho Chi Minh City

**Date:** December 7, 2025

[Download PDF](/images/2-Proposal/AWS-Proposal.pdf)

---

## TABLE OF CONTENTS

1. **BACKGROUND AND MOTIVATION**
   - 1.1 Executive Summary
   - 1.2 Project Success Criteria
   - 1.3 Assumptions
2. **SOLUTION ARCHITECTURE / ARCHITECTURAL DIAGRAM**
   - 2.1 Technical Architecture Diagram
   - 2.2 Technical Plan
   - 2.3 Project Plan
   - 2.4 Security Considerations
3. **ACTIVITIES AND DELIVERABLES**
   - 3.1 Activities and Deliverables
   - 3.2 Out of Scope
   - 3.3 Path to Production
4. **EXPECTED AWS COST BREAKDOWN**
5. **IMPLEMENTATION TEAM**
6. **RESOURCES & ESTIMATED PERSONNEL COSTS**
7. **ACCEPTANCE**

---

## 1. BACKGROUND AND MOTIVATION

### 1.1 EXECUTIVE SUMMARY

**Customer Background:**
The DaiVietBlood system is designed to serve the community, including voluntary blood donors, patients in need of emergency blood, and healthcare professionals in Vietnam. The primary customers are blood donors, patient families, and medical staff responsible for managing blood inventory and donation schedules. They require a centralized, reliable platform to optimize the matching process between donors and recipients and improve communication during emergencies. In the context of digital health transformation, DaiVietBlood provides a secure, accessible solution to address localized blood shortages.

**Business and Technical Objectives:**
Migrating the DaiVietBlood system from a local/on-premise environment to AWS offers superior advantages:

- **Business:** AWS allows the application to scale flexibly according to the user base, reduces hardware infrastructure operational costs, and ensures consistent performance nationwide.
- **Technical:** AWS provides High Availability and medical data security. Adopting a **Serverless Architecture** (AWS Lambda, API Gateway, Cognito, RDS) simplifies backend management, accelerates development, and reduces maintenance costs. The system integrates comprehensive monitoring (CloudWatch) and adheres to strict security standards.

**Summary of Key Use Cases:**

| Role                      | Key Function                       | Short Description                                                                                  |
| :------------------------ | :--------------------------------- | :------------------------------------------------------------------------------------------------- |
| **Guest**                 | Access Public Information          | View donation guidelines, blood compatibility charts, and educational articles without logging in. |
| **Member**                | Register/Login, Profile Management | Create accounts, update personal information and blood type.                                       |
|                           | Book Blood Donation                | Select time slots and locations for donation.                                                      |
|                           | Submit Emergency Request           | Submit urgent blood requests; the system automatically finds suitable donors.                      |
| **Staff**                 | Manage Requests & Inventory        | Approve emergency requests, confirm donation schedules, update blood stock.                        |
| **Administrator (Admin)** | System Administration              | Manage user accounts, configure donation slots, view overview reports.                             |

**Summary of Partner’s Professional Services:**
The Skyline Team will provide comprehensive digital transformation services, including assessing the current local application, designing a Cloud-native architecture, and executing the migration of the system to an AWS Serverless environment. We commit to delivering a secure, scalable system accompanied by automated CI/CD pipelines and detailed operational documentation.

### 1.2 PROJECT SUCCESS CRITERIA

1.  **Functionality:** 100% of core functions (registration, scheduling, emergency requests, administration) operate stably on AWS with no regression errors.
2.  **Availability:** System achieves Uptime ≥ 99.9%, ensuring continuous 24/7 access.
3.  **Performance:** Application response time improves by at least 30% compared to the local version. Emergency request processing time is reduced by 40%.
4.  **Cost:** Infrastructure costs are optimized by at least 20% thanks to the Serverless model and Auto-scaling.
5.  **User Experience:** UAT acceptance rate reaches a minimum of 95% for all user roles.
6.  **Security:** Full compliance with data encryption, access management (IAM), and API security requirements.
7.  **Operations:** CI/CD pipeline is fully automated with deployment time < 10 minutes. Monitoring system covers 100% of critical services.

### 1.3 ASSUMPTIONS

**Technical & Architectural Assumptions:**

- **Source Code:** The current Local application (Frontend & Backend) is functionally complete. The project focuses on Refactoring for the Cloud (Serverless), excluding the development of new features.
- **AWS Region:** The entire infrastructure is deployed in **Singapore (ap-southeast-1)** to optimize latency for users in Vietnam. _Note: During the testing phase, due to limited VPC configurations and Free Tier/Student resources, latency may fluctuate (estimated ~3.5s/request)._
- **Service Limits:** The AWS account uses default limits (Soft limits). Increasing limits to reduce latency will be approved by the Customer when necessary.
- **Third-party Integration:** The system uses the Gemini API for AI support features.
- **Access Rights:** The Skyline Team is granted Admin access (IAM Role) to provision resources.

**Operational & Financial Assumptions:**

- **Domain:** The Customer owns the domain name (e.g., daivietblood.com) and DNS configuration rights.
- **Cost:** The cost estimate is based on an assumption of approximately 50,000 API requests/month. Actual costs depend on usage levels (Pay-as-you-go).

---

## 2. SOLUTION ARCHITECTURE / ARCHITECTURAL DIAGRAM

### 2.1 TECHNICAL ARCHITECTURE DIAGRAM

The DaiVietBlood system utilizes a **Serverless-First** architecture on AWS Cloud, prioritizing scalability, security, and operational optimization.

![AWS architecture picture](/images/AWS_Architecture.png)

**Key Components:**

1.  **Network Infrastructure (VPC):**
    - _Public Subnet:_ Contains Internet Gateway and NAT Gateway.
    - _Private Subnet:_ Contains AWS Lambda and Amazon RDS to isolate and secure data, preventing direct Internet access.
2.  **Application & Data:**
    - _Frontend:_ Hosted on **AWS Amplify**, distributed via **Amazon CloudFront** (CDN), and assets stored on **S3**.
    - _Authentication:_ **Amazon Cognito** manages identity and issues JWT tokens.
    - _API & Compute:_ **Amazon API Gateway** receives requests and routes them to **AWS Lambda** for business logic processing.
    - _Database:_ **Amazon RDS** stores structured data, located in the Private Subnet.
3.  **DevOps & Monitoring:**
    - _CI/CD:_ Uses **AWS CodePipeline, CodeBuild, CodeDeploy** to automate the deployment process.
    - _Monitoring:_ **Amazon CloudWatch** centrally collects logs and metrics.

### 2.2 TECHNICAL PLAN

The technical implementation process follows the Infrastructure-as-Code (IaC) methodology:

- **Infrastructure Automation:** Use **AWS CloudFormation** to provision VPC, Lambda, RDS, and API Gateway, ensuring consistency across environments (Dev/Staging/Prod).
- **Application Development:** Refactor backend into modular Lambda functions (NodeJS/Python). Environment variables and sensitive information (DB credentials) are securely encrypted.
- **CI/CD Process:**
  - Source (GitHub) -> Build (CodeBuild) -> Deploy (CloudFormation/CodeDeploy).
  - Includes a Manual Approval step before deploying to the Production environment.
- **Testing Strategy:** Unit Tests for Lambda, Integration Tests for API, and Load Tests to ensure capacity.

### 2.3 PROJECT PLAN

The project applies the Agile Scrum model over 8 weeks (4 Sprints):

- **Sprint 1 (Foundation):** Set up AWS Account, VPC, RDS.
- **Sprint 2 (Backend Core):** Develop Lambda, API Gateway, Cognito.
- **Sprint 3 (Integration):** Deploy Frontend (Amplify), finalize CI/CD Pipeline.
- **Sprint 4 (Stabilization):** UAT, Performance Optimization, Handover.

### 2.4 SECURITY CONSIDERATIONS

1.  **Access Management:** Use Cognito for user authentication and IAM Roles for service authorization (Least Privilege).
2.  **Network Isolation:** Database and Lambda are located in the Private Subnet, accessing the Internet only via NAT Gateway.
3.  **Data Protection:** Data encryption At-rest (on RDS/S3) and In-transit (via HTTPS).
4.  **Security Monitoring:** CloudWatch Logs record all activities for auditing and intrusion detection.

---

## 3. ACTIVITIES AND DELIVERABLES

### 3.1 ACTIVITIES AND DELIVERABLES

| Phase                         | Timeline  | Key Activities                                                        | Deliverables                                   | Estimate (Man-days) |
| :---------------------------- | :-------- | :-------------------------------------------------------------------- | :--------------------------------------------- | :------------------ |
| **Analysis & Design**         | Week 1    | Assess Local state, design Cloud architecture, plan migration.        | SRS Document, Architecture Diagram, API Specs. | 5                   |
| **Local Development**         | Week 2-3  | Build backend logic, database schema, local unit tests.               | Backend Prototype, Database Schema.            | 10                  |
| **Frontend & Integration**    | Week 4-5  | Develop Frontend, integrate local APIs, prepare code for refactoring. | Completed Local Application.                   | 10                  |
| **AWS Infrastructure Setup**  | Week 6    | Write CloudFormation scripts, provision VPC, RDS, IAM.                | IaC Templates, Secure VPC Environment.         | 5                   |
| **Refactor & Deploy Backend** | Week 7-8  | Convert to Lambda, configure API Gateway, Cognito.                    | Serverless Backend active on AWS.              | 10                  |
| **Deploy Frontend & CI/CD**   | Week 9-10 | Host Frontend on Amplify, set up automated Pipeline.                  | Production URL, CI/CD Pipeline.                | 10                  |
| **Testing & Go-live**         | Week 11   | UAT, Security Testing, Performance Optimization.                      | UAT Report, Security Report.                   | 5                   |
| **Handover & Training**       | Week 12   | Transfer accounts, operations training, handover documentation.       | Operations Manual, Acceptance.                 | 5                   |

### 3.2 OUT OF SCOPE

Due to time and resource limitations of the MVP phase, the following items are not included:

- Optimal user search algorithm based on real-time Geo-location (currently using simplified logic).
- Complex Auto-scaling for the Database layer (currently using basic RDS).
- Deep Latency Optimization for regions outside Singapore.
- Advanced Security Compliance standards such as HIPAA/PCI-DSS.

### 3.3 PATH TO PRODUCTION

To upgrade from the current MVP to a large-scale Production system, the following are required:

1.  **Environment Strategy:** Strictly separate Dev/Staging/Prod environments across different AWS accounts (Multi-account strategy).
2.  **Database Scaling:** Migrate to Amazon Aurora Serverless or use Read Replicas to increase read/write capacity.
3.  **Enhanced Monitoring:** Integrate AWS X-Ray to trace requests and identify performance bottlenecks.
4.  **Strengthened Security:** Deploy AWS WAF with rules to block DDoS and automated bots; use Amazon Inspector for periodic vulnerability scanning.

---

## 4. EXPECTED AWS COST BREAKDOWN

The following [Cost Estimation](https://calculator.aws/#/estimate?id=6f03c76acaabcf7f7d55ccb27d6c2600a04ad472) is based on the _Asia Pacific (Singapore) region_, which is the standard region for low latency access from Vietnam.

| Category     | Service      | Estimated Configuration                                       | Monthly Cost (USD)   |
| :----------- | :----------- | :------------------------------------------------------------ | :------------------- |
| **Network**  | NAT Gateway  | 1 NAT Gateway (Required for Private Subnet) + Data Processing | ~$43.13              |
|              | VPC          | Subnets, Security Groups                                      | ~$13.14              |
|              | CloudFront   | 5GB Data Transfer (Utilizing Free Tier)                       | ~$3.00               |
| **Compute**  | Lambda       | 1,000 requests, 512MB RAM (Free Tier)                         | ~$0.00               |
|              | API Gateway  | 1,000 requests                                                | ~$0.00               |
| **Database** | RDS          | db.t3.micro, 20GB Storage                                     | ~$21.74              |
| **Storage**  | S3           | 5GB Storage, 200 requests                                     | ~$0.14               |
| **Hosting**  | Amplify      | Build & Hosting, WAF enabled                                  | ~$16.77              |
| **Ops**      | CloudWatch   | Logs, Metrics, Alarms                                         | ~$9.41               |
| **CI/CD**    | CodePipeline | 1 Active Pipeline                                             | ~$1.05               |
| **Total**    |              |                                                               | **~$108.38 / Month** |

---

Dưới đây là nội dung mục **5. TEAM** đã được định dạng lại theo đúng cấu trúc bảng biểu trong tài liệu dự án của bạn:

---

### 5. TEAM

**AWS FCJ Program Lead (Mentor)**

| Name                | Title                       | Description                                                                          | Email / Contact Info |
| :------------------ | :-------------------------- | :----------------------------------------------------------------------------------- | :------------------- |
| **Nguyễn Gia Hưng** | Head of Solutions Architect | Provides technical mentorship, architecture review, and AWS best practices guidance. | hunggia@amazon.com   |

**Project Stakeholders**

| Name                | Title              | Stakeholder for                                            | Email / Contact Info |
| :------------------ | :----------------- | :--------------------------------------------------------- | :------------------- |
| **AWS FCJ Mentors** | Program Instructor | Academic oversight, project evaluation, internship credit. | [N/A]                |

**Internship Team (FPT University)**

| Name                       | Title               | Role                                                                                                                   | Email / Contact Info         |
| :------------------------- | :------------------ | :--------------------------------------------------------------------------------------------------------------------- | :--------------------------- |
| **Nguyễn Đức Lân**         | Co-Lead (PM)        | **Project Manager:** Responsible for team coordination, progress tracking, code structure, UAT, and Cost Optimization. | lannguyen68609@gmail.com     |
| **Nguyễn Công Minh**       | Co-Lead (Technical) | **DevOps & Backend:** CI/CD, CodePipeline, CDK Stack, Lambda implementation.                                           | minhncse182968@fpt.edu.vn    |
| **Đỗ Khang**               | Member              | **Cloud Architect:** Architect Design, Service Policy, Co-designed AWS serverless architecture.                        | dokhang307@gmail.com         |
| **Lê Hoàng Anh**           | Member              | **Fullstack Dev:** API implementation, UI/UX design, Security configurations.                                          | anhlhse170327@fpt.edu.vn     |
| **Nguyễn Quách Lam Giang** | Member              | **Data Engineering:** RDS with MySQL connection, VPC & Subnet configuration.                                           | nguyenlamgiang2198@gmail.com |

**Project Escalation Contacts**

| Name               | Title     | Role                                                | Email / Contact Info     |
| :----------------- | :-------- | :-------------------------------------------------- | :----------------------- |
| **Nguyễn Đức Lân** | Team Lead | Primary contact for project status and escalations. | lannguyen68609@gmail.com |

---

Here is the **6. RESOURCES & COST ESTIMATES** section formatted professionally in both English and Vietnamese.

I have adjusted the "Rate" column to reflect that this is an academic internship project (N/A or $0), and included a note explaining the cost efficiency.

---

#### 6. RESOURCES & COST ESTIMATES

**Resource Breakdown**

| Resource                   | Responsibility                                                                                                                                         |
| :------------------------- | :----------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Nguyễn Đức Lân**         | **Coordination & PM:** Project Management, Cost Optimization, Forensic analysis, System configuration, and Full-stack development support.             |
| **Nguyễn Công Minh**       | **DevOps & Infrastructure:** CI/CD pipeline, Security, CDK Stack, Lambda implementation, Co-design API structure, System infrastructure.               |
| **Lê Hoàng Anh**           | **Frontend & UI/UX:** Frontend development, API Integration, UI/UX Design, Application Security.                                                       |
| **Nguyễn Quách Lam Giang** | **Data Engineering:** Data Analysis, RDS to MySQL connection, VPC creation, CloudWatch monitoring, Subnet and NAT Gateway configuration.               |
| **Đỗ Khang**               | **Cloud Architecture:** Architecture design, Service Policies, Documentation, API structure co-design, AI Chatbot integrations, CloudWatch monitoring. |

**Hours by Project Phase**

| Project Phase                | N.Đ. Lân | N.C. Minh | L.H. Anh | N.Q.L. Giang | Đ. Khang | Total Hours |
| :--------------------------- | :------: | :-------: | :------: | :----------: | :------: | :---------: |
| **Foundation**               |    30    |    30     |    30    |      30      |    30    |   **150**   |
| **Core Orchestration**       |    30    |    30     |    30    |      30      |    30    |   **150**   |
| **Analytics Layer**          |    30    |    30     |    30    |      30      |    30    |   **150**   |
| **Testing & Validation**     |    30    |    30     |    30    |      30      |    30    |   **150**   |
| **Documentation & Handover** |    30    |    30     |    30    |      30      |    30    |   **150**   |
| **Total Hours**              | **150**  |  **150**  | **150**  |   **150**    | **150**  |   **750**   |

**Cost Distribution**

| Party                  | Contribution (USD)                | % Contribution of Total |
| :--------------------- | :-------------------------------- | :---------------------- |
| **AWS FCJ Program**    | $0 (Non-profit internship)        | 0%                      |
| **FPT University**     | Student labor (Academic Credit)   | 0%                      |
| **AWS Infrastructure** | ~$15.00 (Testing & Running Costs) | 100%                    |
| **Total Project Cost** | **~$15.00**                       | **100%**                |

**\*Note on Cost Efficiency:** The labor cost for this project is subsidized as part of the FPT University Internship and AWS First Cloud Journey (FCJ) program. The estimated $15 represents the infrastructure running costs (AWS Credits) required for development and testing environments.

---

## 7. ACCEPTANCE

### 7.1 Submission of Deliverables:

Upon completion of the "Handover" phase, the Skyline Team will submit all Source Code, Architecture Documentation, Admin Accounts, and Operations Manuals to the Customer/Mentor.

### 7.2 Acceptance Period & Process:

The Customer has 05 business days to review and perform UAT. If the product meets the Success Criteria (Section 1.2), the Customer will sign the acceptance confirmation.

### 7.3 Defect Remediation:

If critical errors arise or features are missing compared to the committed scope, the Skyline Team is responsible for fixing and resubmitting for acceptance as soon as possible.
