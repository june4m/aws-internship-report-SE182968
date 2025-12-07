Here is the translation for Week 7, following the exact format you provided:

---

title: "Week 7 Worklog"
date: "2025-11-03"
weight: 1
chapter: false
pre: " <b> 1.7. </b> "

---

{{% notice warning %}}
⚠️ **Note:** The information below is for reference purposes only. Please **do not copy verbatim** for your report, including this warning.
{{% /notice %}}

### Week 7 Goals:

- Connect and get to know members of the First Cloud Journey.
- Understand basic AWS services, how to use the Console & CLI.

### Tasks to be implemented this week:

# Week 7 – Project Data Engineering & Intelligent Recommendations Deployment with AWS Personalize

| Day   | Task                                                                                                                                                                                                                                   | Start Date | Completion Date | Reference              |
| :---- | :------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | :--------- | :-------------- | :--------------------- |
| **2** | - Design and build data for the project<br>- user<br>- club<br>- court<br>- booking<br>- operation                                                                                                                                     | 03/11/2025 | 03/11/2025      |                        |
| **3** | - Research recommendation system models:<br>- Collaborative Filtering<br>- Content-based Filtering<br>- Matrix Factorization CF                                                                                                        | 04/11/2025 | 04/11/2025      | documents              |
| **4** | - Research required data for training Personalize, how to import data to S3 buckets, how Personalize fetches data from buckets, schema formatting<br>- Understand evaluation metrics, accuracy types, and Hyperparameters of the model | 05/11/2025 | 05/11/2025      | Video 1<br><br>Video 2 |
| **5** | - Process data for the model                                                                                                                                                                                                           | 06/11/2025 | 07/11/2025      |                        |
| **6** |                                                                                                                                                                                                                                        |            |                 |                        |

---

### Week 7 Achievements

**1. Database Schema Design & Data Generation**

- Defined and built data structures for key entities: Users, Clubs, Courts, Bookings, Operations.
- Generated synthetic data simulating a real-world environment.
- Ensured data consistency and accurate reflection of user behavior.

**2. Recommendation System Fundamentals**

- Researched and compared recommendation algorithms:
  - Collaborative Filtering
  - Content-based Filtering
  - Matrix Factorization
- Understood how each model works and its appropriate use cases.

**3. AWS Personalize Implementation**

- Mastered the end-to-end process of Amazon Personalize:
  - **Data Ingestion:** Standardizing JSON schema, uploading data to S3, importing into Personalize.
  - **Model Training:** Understanding Hyperparameters and how to tune them.
  - **Evaluation:** Reading and analyzing evaluation Metrics to determine model accuracy.

**4. Interaction Modeling**

- Solved the "cold start" problem and data scarcity by selecting Booking as the primary interaction signal (high-intent interaction).
- Adjusted the modeling strategy to fit synthetic data, in the context of lacking interactions like view/click.

**5. Data Pre-processing Strategy**

- Built logic to assign scores to interactions.
- Cleaned data, standardized formats, and ensured compliance with Personalize's strict requirements.
- Prepared the complete dataset, ready for model training.
