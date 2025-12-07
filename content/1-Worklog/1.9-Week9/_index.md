---
title: "Week 9 Worklog"
date: "2025-11-10"
weight: 1
chapter: false
pre: " <b> 1.9. </b> "
---

{{% notice warning %}}
⚠️ **Note:** The information below is for reference purposes only. Please **do not copy verbatim** for your report, including this warning.
{{% /notice %}}

### Week 9 Goals:

- Build and train the Recommendation Model.
- Refine data and handle issues arising during the training process.

### Tasks to be implemented this week:

# Week 9: Model Building & Data Refinement

| Day   | Task                                                                                                                                          | Start Date | Completion Date | Reference            |
| :---- | :-------------------------------------------------------------------------------------------------------------------------------------------- | :--------- | :-------------- | :------------------- |
| **2** | **Model Building:**<br>- Configure model recipe<br>- Create dataset import job<br>- Run training process                                      | 10/11/2025 | 11/11/2025      | AWS Personalize Docs |
| **3** | Preliminary model check and error analysis                                                                                                    | 12/11/2025 | 12/11/2025      |                      |
| **4** | **Data Refinement & Update:**<br>- Add new fields to the Schema<br>- Clean and resynchronize the dataset<br>- Retrain model with updated data | 12/11/2025 | 13/11/2025      |                      |
| **5** | Re-evaluate metrics after data update                                                                                                         | 14/11/2025 | 14/11/2025      |                      |
| **6** |                                                                                                                                               |            |                 |                      |

---

### Week 9 achievement: Challenges in Model Training and Data Quality

This week focused heavily on model training techniques, and I faced real-world Data Science challenges:

- **Data Scarcity & Interaction Limitations:**

  - **Issue:** The dataset volume is limited, and critically, there is only one interaction type: "Booking". The lack of high-funnel interactions like "View" or "Click" makes it difficult for the model to capture latent user preferences.
  - **Consequence:** The model's evaluation metrics after training are relatively low, leading to significant skepticism regarding the accuracy and practical effectiveness of the recommendations.

- **Validation Blockers:**

  - Since the Website Frontend is not yet complete, I cannot perform real-world validation (visual checks) to see if the recommendations make sense intuitively. Currently, I rely solely on abstract metrics.

- **Data Schema Evolution:**
  - The addition of new data fields during development caused friction in the data processing pipeline. Every schema change required re-cleaning the data, re-mapping the schema, and retraining the model from scratch, which was time-consuming.
