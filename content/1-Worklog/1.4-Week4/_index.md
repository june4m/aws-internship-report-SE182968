---
title: "Week 4 Worklog"
date: "2025-09-29"
weight: 4
chapter: false
pre: " <b> 1.4. </b> "
---



### Week 4 Goals:

- **Storage & Content Delivery:** Understand and practice deploying static website hosting using Amazon S3 combined with the CloudFront content delivery network.
- **Developer Tools (DevTools):** Get familiar with using AWS CLI via VSCode to interact with AWS services more professionally and efficiently.
- **Databases:** Practice creating and managing relational databases with Amazon RDS (MySQL).

### Tasks to be implemented this week:

| Day   | Task                                                                                                                                                                                                                                      | Start Date | Completion Date | Reference             |
| :---- | :---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | :--------- | :-------------- | :-------------------- |
| **2** | **S3 & CloudFront:**<br>- Research core concepts of S3 and CloudFront.<br>- **Practice:** Configure an S3 bucket for static website hosting, manage access permissions, create a CloudFront distribution, and practice Bucket Versioning. | 29/09/2025 | 30/09/2025      | Amazon S3 Guide       |
| **3** | **AWS CLI & VSCode:**<br>- Setup development environment: Connect AWS with VSCode.<br>- Use AWS CLI to interact with services instead of using the Console.                                                                               | 30/09/2025 | 30/09/2025      | AWS CLI Configuration |
| **4** | **Databases:**<br>- Practice creating an RDS Instance.<br>- Connect and work with MySQL on AWS infrastructure.                                                                                                                            | 01/10/2025 | 01/10/2025      | Amazon RDS (MySQL)    |

---

### Week 4 Achievement: Static Website, CDN & Database Administration

#### 1. S3 Storage Mechanism and Static Website Hosting

- **Understanding S3:** Mastered S3 as an Object Storage service, identifying it as an ideal solution for hosting static web resources (HTML/CSS/JS).
- **Deployment Process:**
  - Enabled the **Static Website Hosting** feature on the Bucket.
  - Configured key documents: `index.html` (entry point) and `error.html` (error page).
  - **Access Management:** Configured Bucket Policy to allow public read access or restricted access via CloudFront for enhanced security.
  - Successfully tested website access via the S3 Endpoint URL.

#### 2. Optimizing Content Delivery with CloudFront

- **Distribution Configuration:**
  - **Origin:** Pointed to the S3 bucket containing the web source code.
  - **Behavior:** Configured caching policies, Time-to-Live (TTL), and HTTP/HTTPS protocols.
- **Key Benefits:**
  - **Performance:** Accelerated page load speeds leveraging the Edge Location network.
  - **Security:** Hidden the origin S3 bucket from end-users (users interact only with CloudFront), supporting HTTPS via ACM Certificates.
- **Operations:** Understood how to use **Invalidations** to clear old cache when updating new website content.

#### 3. Professional Interaction with AWS CLI & VSCode

- **Environment Setup:**
  - Successfully installed the AWS Toolkit extension for VSCode.
  - Configured user authentication (`aws configure`) using Access Key and Secret Key.
- **Command Line Operations:** Transitioned from using the graphical interface (Console) to using CLI commands for managing resources like S3, EC2, and RDS, significantly speeding up workflow.

#### 4. Amazon RDS (MySQL) Administration

- **Database Deployment:**
  - Successfully launched an RDS MySQL instance.
  - **Network Configuration:** Set up **Subnet Groups** and **Security Groups** to control traffic flow to the database.
- **Connection & Querying:**
  - Used MySQL Workbench to connect to the RDS endpoint.
  - Performed basic database administration tasks (Creating DBs, executing SQL commands).
