# Milestone 2 – Task 1 & Task 1B Report
**Project:** Data Product Insight Recommendation  
**Team:** Trufflow 1B  
**Objective:** Train contender models using different feature combinations for anomaly detection (Task 1) and service similarity (Task 1B), expanding beyond the baseline Naive Bayes.

---

## Tools & Libraries Used
| Purpose | Tools / Libraries |
|:--|:--|
| Core environment | Python 3.12 + Jupyter / VS Code |
| Data manipulation | numpy, csv, json, collections.defaultdict |
| Date/time handling | datetime |
| Machine learning & metrics | scikit-learn (Pipeline, StandardScaler, GaussianNB, LogisticRegression, KNeighborsClassifier, DecisionTreeClassifier, KMeans, AgglomerativeClustering, PCA) |
| Visualization / analysis | precision_recall_curve, average_precision_score, f1_score, accuracy_score, silhouette_score |
| Similarity & clustering | cosine_similarity |

---

## Dataset Summary
| Dataset | Purpose | Rows | Unique Units |
|:--|:--|--:|--:|
| transactions.jsonl | Build hourly transaction-level features for anomaly detection | 7,254,656 | 1,540,175 aggregated windows |
| daily_metrics.jsonl | Daily app-level metrics | 444,570 | 87 apps × 365 days |
| monthly_metrics.jsonl | Monthly summaries per app | 18,270 | 10 months × 87 apps |

**Feature matrix shape:** (1,540,175 × 6)  
**Features:** [req_count, error_rate, cost_sum, cost_mean, data_sum, data_mean]

---

## Weak Label Definition
An anomaly label (y = 1) is assigned when any of the following hold:  
- error_rate > 0.10  
- robust_z(cost_mean) > 3.0  
- robust_z(data_mean) > 3.0  
Else y = 0.

---

## Task 1 – Anomaly Detection (Insight Detection)

### Baseline & Contender Models
All models were trained on an 80/20 time-based split and evaluated using PR-AUC, Precision, Recall, F1, and Accuracy.

| Model | Feature Set | PR-AUC | Precision | Recall | F1 | Accuracy | Threshold | Valid Size | Pos Rate Valid |
|:--|:--|--:|--:|--:|--:|--:|--:|--:|--:|
| GaussianNB | All 6 | 0.977 | 0.903 | 0.945 | 0.924 | 0.987 | 0.981 | 308 035 | 0.0849 |
| Logistic Regression | All 6 | 0.9987 | 0.987 | 0.985 | 0.986 | 0.9976 | 0.539 | 308 035 | 0.0849 |
| KNN | All 6 | 0.9999 | 0.9997 | 0.9992 | 0.9994 | 0.9999 | 0.600 | 308 035 | 0.0849 |
| Decision Tree | All 6 | 1.000 | 1.000 | 1.000 | 0.156 | 0.0849 | 0.000 | 308 035 | 0.0849 |
| GaussianNB | Top 3 | 0.991 | 0.977 | 0.967 | 0.971 | 0.995 | 0.969 | 308 035 | 0.0849 |
| Logistic Regression | Top 3 | 0.998 | 1.000 | 0.988 | 0.993 | 0.999 | 0.547 | 308 035 | 0.0849 |
| KNN | Top 3 | 0.990 | 1.000 | 0.989 | 0.994 | 0.999 | 0.800 | 308 035 | 0.0849 |
| Decision Tree | Top 3 | 0.999 | 1.000 | 0.989 | 0.979 | 0.996 | 0.050 | 308 035 | 0.0849 |

File saved: task1_contender_results.csv

---

### Best Contender Summary
✅ KNN (Top 3 features) achieved the best balance of PR-AUC = 0.9909 and F1 = 0.9943.  
It outperformed the baseline Naive Bayes in both precision and recall with minimal overfitting.

---

### Confusion Matrix – 3 Feature Naive Bayes (Baseline)
| | Pred 0 | Pred 1 |
|:--|--:|--:|
| Actual 0 | 281 251 | 639 |
| Actual 1 | 858 | 25 287 |

Accuracy 0.9951  Precision 0.9754  Recall 0.9672  F1 0.9713  
Files task1_confusion_matrix_selected.csv, task1_confusion_summary_selected.json

---

### Top Anomalous Incidents
| time_bucket | consumer_id | supplier_id | probability | predicted_anomaly | req_count | cost_sum | cost_mean |
|:--|:--|:--|--:|--:|--:|--:|--:|
| 2025-05-12 02:00 | ECOMLP | MCSCBT | 1.0 | 1 | 6 | 3 539.95 | 589.99 |
| 2025-03-30 01:00 | CUSTPRT | MCSCBT | 1.0 | 1 | 6 | 3 465.25 | 577.54 |
| 2025-03-25 05:00 | ECOMLP | MCSCBT | 1.0 | 1 | 5 | 2 921.6 | 584.32 |

File task1_top_incidents_selected.csv

---

## Task 1B – Service Similarity (Task 2 Baseline Preparation)

### Baseline Method
- Cosine Similarity between 8-dimensional role vectors derived from monthly metrics.  
- Output: pairwise service similarity matrix.

### Contender Models
| Model / Method | Description | Goal |
|:--|:--|:--|
| Baseline Cosine | Raw cosine similarity of 8-dim service vectors | Establish similarity relationships |
| PCA(3)+Cosine NN | Reduce to 3 components before cosine comparison | Improve generalization + noise reduction |
| KMeans (3–10 clusters) | Partition normalized service vectors | Identify logical service groupings |
| Agglomerative (Average linkage) | Hierarchical clustering on precomputed cosine distance | Capture nested service hierarchies |

---

### Outputs & Files
| Output File | Description |
|:--|:--|
| task2_neighbors.csv | Baseline cosine nearest neighbors |
| task2_neighbors_pca3.csv | PCA(3) neighbor comparisons |
| task2_clustering_results.csv | KMeans + Agglomerative cluster assignments (3 → 10 clusters) |

**Result Summary:**  
Services 87 Vector shape (87 × 8) Silhouette scores evaluated per k to select best cluster count.

---

## Key Takeaways
✅ Task 1 – Baseline NB solid, KNN and LogReg outperformed.  
✅ Task 1B – Cosine baseline + PCA clustering captured key service patterns.  
✅ All results ready for Task 2 comparison.

---

### Artifacts Generated
task1_contender_results.csv  
task1_confusion_matrix_selected.csv  
task1_confusion_summary_selected.json  
task1_top_incidents_selected.csv  
task2_neighbors.csv  
task2_neighbors_pca3.csv  
task2_clustering_results.csv  

---

✅ **Task 1 and 1B Completed**  
Next → Task 2 – Contender / Baseline Comparison & Analysis
