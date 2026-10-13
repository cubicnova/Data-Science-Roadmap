# 02 - Data Science Lifecycle

The Data Science Lifecycle is a **systematic sequence of activities** used to solve problems with data: defining the problem, collecting and preparing data, building and evaluating models, and deploying and maintaining the solution.

> ⏱️ **Estimated time:** 4-6 hours  
> 📌 **Prerequisites:** [01 - Introduction to Data Science](../01-Introduction-to-Data-Science/)  
> 🎯 **Level:** Beginner

---

## 📖 What You'll Learn

- [ ] Describe the 10 stages of the data science lifecycle
- [ ] Identify the key activities and deliverables of each stage
- [ ] Explain why the lifecycle is iterative
- [ ] Apply the lifecycle to a real-world problem
- [ ] Recognize common methodologies (CRISP-DM, OSEMN)

---

## 1. Lifecycle at a Glance

```
01 Problem Definition
 ↓
02 Data Collection
 ↓
03 Cleaning & Preprocessing
 ↓
04 Exploratory Data Analysis
 ↓
05 Feature Engineering
 ↓
06 Model Development
 ↓
07 Evaluation & Validation
 ↓
08 Deployment
 ↓
09 Monitoring & Maintenance
 ↓
10 Communication & Improvement
```

> 🔁 **The lifecycle is iterative.** Evaluation, monitoring, and feedback often send you back to earlier stages. Communication also happens throughout the project, not only at the end.

---

## 2. The Stages in Detail

> Running example: an **e-commerce company** wants to identify customers likely to stop purchasing.

### Stage 1: Problem Definition and Business Understanding
Identify the objective, requirements, and expected outcomes.
- Define the problem statement
- Identify stakeholders and their requirements
- Set project objectives, success criteria, and constraints
- Formulate the questions the analysis should answer

**Example:** Predict which customers are likely to leave.

### Stage 2: Data Collection and Acquisition
Gather relevant data from different sources.
- **Sources:** relational databases, CSV/Excel files, web apps and APIs, sensors and IoT devices, websites, system logs, transaction records

**Example:** Collect purchase history, browsing activity, payment records, and customer feedback.

### Stage 3: Data Cleaning and Preprocessing
Improve data quality and suitability for analysis.
- Handle missing values and remove duplicates
- Correct inconsistent data and treat outliers
- Convert data into appropriate formats
- Encode categorical variables and scale numerical features when appropriate

**Example:** Remove duplicate customer records and fill missing purchase history values.

### Stage 4: Exploratory Data Analysis (EDA)
Understand the data's characteristics, patterns, and relationships.
- Calculate descriptive statistics and analyze distributions
- Identify correlations, anomalies, and outliers
- Create charts and visualizations
- Develop initial hypotheses

**Example:** Check whether customers who buy less frequently are more likely to leave.

### Stage 5: Feature Engineering and Selection
Create useful variables and keep only the relevant ones.
- Create new features from existing data
- Select relevant inputs and remove redundant features
- Encode, transform, or scale features where necessary
- **Prevent data leakage** by using only information available at prediction time

**Example:** Calculate the number of purchases per customer in the last 90 days.

### Stage 6: Model Development and Training
Select and train statistical or machine learning models.
- **Common techniques:** linear regression, logistic regression, decision trees, random forests, SVM, K-means clustering, neural networks
- Split data into training, validation, and test sets
- Tune parameters and compare candidate models

**Example:** Train a classification model to predict customer churn.

### Stage 7: Model Evaluation and Validation
Check performance on unseen data against project objectives.

| Problem type | Evaluation metrics |
|--------------|--------------------|
| **Classification** | Accuracy, precision, recall, F1-score, ROC-AUC |
| **Regression** | MAE, MSE, RMSE, R² |
| **Clustering** | Silhouette score, cluster quality |

- Check for overfitting and underfitting
- Compare results with a baseline
- Assess business impact and limitations

**Example:** Does the churn model catch enough at-risk customers without too many false alerts?

### Stage 8: Deployment and Implementation
Make the validated solution available for practical use.
- **Methods:** REST APIs, web applications, BI dashboards, batch prediction pipelines, integration with existing business systems

**Example:** Integrate the churn model into a customer management app for the marketing team.

### Stage 9: Monitoring and Maintenance
Make sure the solution keeps performing as data and conditions change.
- Monitor prediction quality and system performance
- Detect **data drift** and **model drift**
- Review data quality and track failures
- Retrain or update the model when necessary
- Maintain security, privacy, and documentation

**Example:** Retrain the model when customer behavior changes and accuracy declines.

### Stage 10: Communication and Decision-Making
Share findings so stakeholders can make informed decisions.
- Prepare reports and dashboards
- Explain findings in clear language
- Communicate uncertainty and limitations
- Recommend actionable improvements and measure the results

**Example:** Present churn trends and recommend retention campaigns to management.

---

## 3. Complete Example: Fraud Detection at a Bank

| Lifecycle stage | Practical application |
|-----------------|-----------------------|
| Problem definition | Identify potentially fraudulent transactions |
| Data collection | Gather transaction records and verified fraud labels |
| Data preprocessing | Correct data issues and prepare relevant fields |
| Exploratory analysis | Examine unusual transaction patterns |
| Feature engineering | Calculate transaction frequency and amount deviations |
| Model development | Train a classification model |
| Evaluation | Measure precision, recall, and false-positive rate |
| Deployment | Integrate the model into transaction screening |
| Monitoring | Detect changing fraud patterns and model degradation |
| Communication | Report fraud trends and operational performance |

---

## 4. Why the Lifecycle Matters

| Benefit | Description |
|---------|-------------|
| **Structured problem-solving** | A repeatable approach to complex problems |
| **Improved data quality** | Fewer errors and inconsistencies |
| **Reliable results** | Encourages proper model validation |
| **Better decisions** | Converts analysis into actionable insights |
| **Scalability** | Supports deployment and ongoing maintenance |
| **Continuous improvement** | Solutions adapt to changing conditions |

---

## 5. Common Methodologies

| Framework | Steps |
|-----------|-------|
| **CRISP-DM** | Business Understanding → Data Understanding → Data Preparation → Modeling → Evaluation → Deployment |
| **OSEMN** | **O**btain → **S**crub → **E**xplore → **M**odel → i**N**terpret |

Both organize the work differently, but both stress a **structured and iterative** approach.

> 💡 **Key takeaway:** The lifecycle is not just a sequence of machine learning steps. It covers the full journey from understanding a real-world problem to delivering, monitoring, and improving a data-driven solution.

---

## 📝 Practice

1. Draw a flowchart of the 10 lifecycle stages and mark where iteration can happen.
2. Pick a problem (house price prediction, student performance, or customer churn) and write one sentence for what you would do at each stage.
3. For the fraud detection example, list two risks at each of Stages 3, 5, and 9.
4. Choose suitable evaluation metrics for one classification and one regression problem, and justify your choice.
5. Explain in your own words what data leakage is and give one example from Stage 5.
6. Compare CRISP-DM and OSEMN: which stages overlap and which differ?

---

## ✅ Self-Check Questions

- Why can Stage 7 send you back to Stage 3 or Stage 5?
- What is the difference between data drift and model drift?
- Why should communication happen throughout the project?
- What should be defined before any data is collected?

---

## 📚 Further Reading

- CRISP-DM process guide
- *Data Science for Business* by Foster Provost and Tom Fawcett
- *Hands-On Machine Learning with Scikit-Learn, Keras & TensorFlow* by Aurélien Géron (project checklist chapter)

---

## ➡️ Next Up

[03 - Tools and Environment](../03-Tools-and-Environment/): get to know the languages, libraries, and platforms used at each lifecycle stage.

---

[⬅️ Previous: Introduction to Data Science](../01-Introduction-to-Data-Science/) · [⬅️ Back to Getting Started](../README.md) · [🏠 Main Roadmap](../../README.md)
