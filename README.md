#  Email Marketing Campaign Optimization

##  Project Overview

This project analyzes an e-commerce site's email marketing campaign with the goal of improving **click-through rate (CTR)** using **machine learning**. Emails were randomly sent to users, and the company wants to identify:
- How the campaign performed
- Whether emails can be sent in a smarter, more targeted way
- What user segments respond better to emails

---
### 1. `email_table.csv`
| Column             | Description                                                             |
|--------------------|-------------------------------------------------------------------------|
| `email_id`         | Unique ID of the email                                                  |
| `email_text`       | `long_email` (4 paragraphs) or `short_email` (2 paragraphs)             |
| `email_version`    | `personalized` ("Hi John") or `generic` ("Hi,")                         |
| `hour`             | Local time (hour) the email was sent                                    |
| `weekday`          | Day of the week the email was sent                                      |
| `user_country`     | Country of the user based on IP                                         |
| `user_past_purchases` | Number of items purchased by the user before this email was sent     |

### 2. `email_opened_table.csv`
| Column      | Description                           |
|-------------|---------------------------------------|
| `email_id`  | Email ID that was opened by the user  |

### 3. `link_clicked_table.csv`
| Column      | Description                           |
|-------------|---------------------------------------|
| `email_id`  | Email ID where the user clicked a link |

---

##  Objectives

1. **Performance Metrics**  
   - Calculate % of emails **opened** and **clicked**

2. **Predictive Modeling**  
   - Build a model to predict **clicks** (target = `clicked`)
   - Use this model to target users more effectively

3. **CTR Improvement Estimation**  
   - Simulate targeting top X% users by predicted probability
   - Compare with baseline (random send)

4. **Segment Analysis**  
   - Discover patterns in CTR by content, time, country, etc.

---

##  ML Workflow

### 🔹 Preprocessing
- Merge datasets using `email_id`
- Create `opened` and `clicked` flags
- One-hot encode categorical features
- Handle class imbalance with `class_weight='balanced'` and `SMOTE`

### 🔹 Models Tried
- Random Forest (best performing)
- Logistic Regression
- XGBoost
- LightGBM

### 🔹 Final Model Used
```python
RandomForestClassifier(n_estimators=100, class_weight='balanced', random_state=42)

```
##  Performance Metrics (Final Random Forest)

The final model selected was a `RandomForestClassifier` with class balancing and threshold tuning (`threshold = 0.3`) to improve recall for the minority class (clicked = 1).

**Evaluation metrics for class `1` (clicked):**

| Metric       | Value |
|--------------|-------|
| Precision    | 0.04  |
| Recall       | 0.56  |
| F1-score     | 0.08  |
| ROC AUC      | 0.73  |

- **Precision** is low due to many false positives, which is expected in imbalanced datasets.
- **Recall** is prioritized here to **capture more clickers**, which aligns with the business goal of increasing engagement.
- **ROC AUC of 0.73** indicates good model ranking performance.

##  Key Insights by User Segment

| Segment Type     | Insight                                                    |
|------------------|------------------------------------------------------------|
| Email Version    | Personalized emails → **+81% CTR** compared to generic     |
| Email Text       | Short emails outperform long ones                          |
| Weekday          | Best days to send: **Wednesday**, Tuesday, Thursday        |
| Country          | Highest CTR: **UK, US (~2.4%)**; Lowest: France, Spain     |
| Hour             | Midday hours perform better (based on CTR trend)           |
| Purchase History | Users with more past purchases are more likely to click    |

---

##  Author

**Sreyashi Dey**  
Email Marketing Case Study  


---
