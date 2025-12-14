Trufflow Data Product Insight Recommendation


Trufflow 1B

Project Type

Group Assignment

Company Context

Trufflow

1. Project Overview

This project focuses on analyzing app-to-app transaction data to automatically detect important insights for data and AI product owners.

Trufflow monitors how applications communicate with each other and tracks metrics such as:

request volume

cost

data usage

errors

Our goal was to build machine learning models that can:

detect unusual or risky behavior

identify similar services

provide early warnings

summarize anomalies using LLMs

This work follows the official project instructions and milestones provided by Trufflow 

Trufflow Project Instructions

2. Objectives

The project includes two core tasks and two bonus tasks:

Core Tasks

Insight (Anomaly) Detection
Identify abnormal app-to-app behavior that may indicate issues or high cost impact.

Service Similarity & Clustering
Group applications that behave similarly using unsupervised learning.

Bonus Tasks

Bonus A: Early Warning Detection
Predict whether an anomaly will happen in the near future.

Bonus B: LLM-Based Value Chain Analyst
Generate short, human-readable summaries explaining anomalies.

3. Data Used

We worked with realistic transaction-level data provided for the project.

Raw Data

7,254,656 transaction records

Aggregation

Aggregated into 1,540,175 hourly windows

Each window contains 6 numeric features:

Feature	Description
req_count	Number of requests
error_rate	Error percentage
cost_sum	Total cost
cost_mean	Average cost
data_sum	Total data transferred
data_mean	Average data per transaction

This data preparation work is described in Milestone 1 

Milestone One

4. Milestone Breakdown
 milestone 1 — Baseline Models & Feature Analysis

Goal: Build baseline models and understand the data.

What We Did

Performed data exploration

Built Naive Bayes baseline model for anomaly detection

Applied cosine similarity for service similarity

Used KMeans clustering

Identified feature redundancy

Key Result

Reduced feature set (3 features) performed better than all 6

Strong redundancy between:

cost_sum & data_sum

cost_mean & data_mean

Best baseline anomaly model:
Naive Bayes (Top-3 features)

PR-AUC ≈ 0.991

F1 ≈ 0.971
 

 Milestone 2 — Model Comparison & Optimization

Goal: Improve models and compare alternatives.

Models Tested
Naive Bayes

Logistic Regression

K-Nearest Neighbors (KNN)

Key Findings

KNN achieved near-perfect performance

Top-3 features matched or outperformed all-6 features

Logistic Regression balanced speed + accuracy

 Best performing model:

KNN (k=7)

F1 ≈ 0.999

Accuracy ≈ 1.000

 Best clustering:

KMeans (k=8)

Silhouette score ≈ 0.983


 Milestone 3 — Final Models, Early Warning & LLM Analysis

Goal: Finalize models and complete bonus tasks.

Task 1 — Final Anomaly Detection

Best model: KNN (k=5, all 6 features)

F1 Score: 0.99945

Bonus Task A — Early Warning

Predicts if an anomaly will occur in the next few hours

Best model: Logistic Regression (Top-3)

Recall: 0.903
(High recall is critical for warnings)

Task 2 — Clustering

Confirmed k = 8 clusters

Clear separation of service roles

Bonus Task B — LLM Analyst

Used T5-small

Generated short summaries explaining anomalies

Low overlap score expected due to short outputs


5. Team Collaboration

This was a group project with shared responsibilities:

Model experimentation and comparison

Feature engineering

Clustering analysis

Documentation and reporting

Each team member contributed across tasks, and results were reviewed collectively.

6. Tools & Libraries Used

Only approved libraries were used:

numpy

scikit-learn

PyTorch

toolz

transformers

Work was organized following the data science lifecycle:

Scope → Prototype → Refine

7. Key Takeaways

Fewer, well-chosen features can outperform larger feature sets

KNN provides excellent accuracy for anomaly detection

Logistic Regression is ideal for real-time early warnings

Clustering reveals meaningful service roles

LLMs help translate technical anomalies into human-readable insights

8. License

All work in this project is licensed under the Apache 2.0 License, allowing reuse, modification, and commercial use with attribution

