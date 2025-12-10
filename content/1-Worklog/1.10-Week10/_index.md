---
title: "Week 10 Worklog"
date: "2025-11-10"
weight: 10
chapter: false
pre: " <b> 1.10. </b> "
---

### Week 10 Objectives:

- **Solution Architecture:** Finalize High-Level (HLD) and Low-Level (LLD) architecture diagrams based on the AWS Well-Architected Framework.
- **Resource Planning & Costing:** Estimate Total Cost of Ownership (TCO) and select appropriate services.
- **Network Infrastructure Initialization (Networking):** Establish the foundation for VPC, Subnets, and basic security policies.

### Tasks to be implemented this week:

| Day     | Task                                                                                                                                                                                                                                                   | Start Date | Completion Date | Reference |
| :------ | :----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | :--------- | :-------------- | :-------- |
| **2-3** | **Design & Planning:**<br>- Analyze project business requirements.<br>- Draw system architecture diagram.<br>- Use AWS Pricing Calculator to create a cost estimate.<br>- Review architecture against the 5 pillars of the Well-Architected Framework. | 10/11/2025 | 11/11/2025      |           |
| **4-5** | **Deploy Network Infrastructure (IaC):**<br>- Write code (Terraform/CloudFormation) to initialize VPC, Public/Private Subnets, NAT Gateway, Route Tables.<br>- Design and configure basic Security Groups for Bastion Host, Web Server, and Database.  | 12/11/2025 | 13/11/2025      |           |

---

### Week 10 Achievements: Architecture Diagrams & Infrastructure Foundation

#### 1. System Architecture Design

- **Architecture Diagram:** Completed data flow diagram and resource layout. The system is designed following the **Single-AZ** model.
- **Service Selection:**
  - **Database:** RDS MySQL (Multi-AZ) for relational data.
  - **Storage:** S3 for static assets and backups.
- **Cost Optimization:** Created a monthly budget estimate and identified saving strategies (such as using Spot Instances for Dev environments, Savings Plans for Prod).

#### 2. Network Deployment (VPC)

- **Network Structure:** Successfully established VPC with custom CIDR block, clearly separating Public Subnets and Private Subnets (for App, DB).
- **Network Security:**
  - Established **Security Groups** following the principle of Least Privilege.
