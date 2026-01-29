# Credit Card Customer Segmentation
**Rizaldy | CMU Applied ML**

Business Goal: Increase utilization rate to drive revenue

---

## EDA: Transactional Features

### Features Overview

| Feature | What it means |
|---------|---------------|
| Credit_Limit | Max credit available |
| Total_Revolving_Bal | Balance carried month to month |
| Avg_Open_To_Buy | Available credit (Limit - Revolving) |
| Total_Trans_Amt | Total transaction amount (12 months) |
| Total_Trans_Ct | Total transaction count (12 months) |
| Total_Amt_Chng_Q4_Q1 | Change in amount Q4 vs Q1 |
| Total_Ct_Chng_Q4_Q1 | Change in count Q4 vs Q1 |
| Avg_Utilization_Ratio | Revolving / Limit |

### Distribution Findings

1. **Credit_Limit & Avg_Open_To_Buy**: Right-skewed, most customers have lower limits. Few premium customers.

2. **Total_Revolving_Bal**: Bimodal (peaks at 0 and ~1500). Two groups: pay full balance vs carry debt.

3. **Avg_Utilization_Ratio**: Right-skewed, many at 0. Lots of under-utilized customers, thats our target.

4. **Total_Trans_Amt & Total_Trans_Ct**: Good variation, useful for differentiating engagement levels.

### Utilization Segments

| Segment | Threshold | Action |
|---------|-----------|--------|
| Low | ≤30% | TARGET, increase engagement |
| Medium | 30-70% | Nurture, maintain |
| High | >70% | Monitor, risk of overleveraging |

Most customers fall in low utilization. Primary targets for campaigns.

### Correlation Check

- Credit_Limit vs Avg_Open_To_Buy: 0.99 correlation, basically same info. Keep only Credit_Limit.
- Total_Trans_Amt vs Total_Trans_Ct: High correlation but measure different things (amount vs frequency), keep both.

### Transaction Behavior Quadrants

| Quadrant | Description | Strategy |
|----------|-------------|----------|
| High Amt / High Ct | Power users | Loyalty rewards, retain |
| High Amt / Low Ct | Big spenders, infrequent | Increase frequency |
| Low Amt / High Ct | Frequent, low value | Upsell opportunity |
| Low Amt / Low Ct | Under-utilized | PRIMARY TARGET |

---

## Preprocessing

### Feature Selection

**Included (15 features):**
- Transactional: Credit_Limit, Total_Revolving_Bal, Total_Trans_Amt, Total_Trans_Ct, Avg_Utilization_Ratio
- Behavioral: Total_Amt_Chng_Q4_Q1, Total_Ct_Chng_Q4_Q1
- Demographic: Customer_Age, Dependent_count, Gender, Education_Level, Marital_Status, Income_Category
- Engineered: avg_transaction_value, contact_inactivity_ratio

**Excluded:**
- Avg_Open_To_Buy: Redundant with Credit_Limit
- CLIENTNUM: Just an ID

### Preprocessing Steps

1. StandardScaler for all numerical features (different scales)
2. LabelEncoder for categoricals (Gender, Education, Marital, Income)
3. No outlier removal, kept full dataset

---

## Q1: K-Means Clustering

### Feature Selection

We picked 15 features after some trial and error. Dropped Avg_Open_To_Buy because it has 0.99 correlation with Credit_Limit.

First iteration with only transactional features gave 3 clusters. After adding demographics, we got 4 clusters that made more business sense.

### Optimal K

Used two methods:
1. **Elbow method**: Plotted SSE vs k, elbow at k=4
2. **Silhouette score**: k=4 scored ~0.18, better than k=3 or k=5

Both pointed to 4 clusters. When we added more features, the optimal k shifted from 3 to 4.

### Cluster Profiles

| Cluster | Name | Util | Trans Amt | Dependents |
|---------|------|------|-----------|------------|
| 0 | Family Savers | 12% | $3,800 | 2.1 |
| 1 | Credit Dependent | 68% | $4,200 | 1.8 |
| 2 | Big Spenders | 35% | $5,500 | 1.5 |
| 3 | Mature Dormant | 8% | $2,900 | 0.8 |

Main differentiator is utilization ratio (8% to 68% range). Family Savers have kids but low usage, thats our target segment.

---

## Q2: Hierarchical vs HDBSCAN

### Hierarchical Clustering

Used Ward linkage, cut dendrogram at 4 clusters. Results similar to K-Means with silhouette 0.14.

Challenges:
- Slow on 10k rows
- Dendrogram messy at scale
- Memory heavy

### HDBSCAN

Regular DBSCAN failed because 15 features is too high dimensional. Distance metrics dont work well.

Fix: PCA to 5 dims first, then HDBSCAN.

| | K-Means | HDBSCAN |
|--|---------|---------|
| Outliers | Forced into cluster | Labeled noise |
| K required | Yes | No |
| Interpretability | Easy | Harder |

HDBSCAN gave 5-10% noise points. Good for anomaly detection but not for marketing segments.

### Which is best?

K-Means for our use case. Clean segments, easy to explain, fast. Hierarchical similar quality but slower. HDBSCAN useful for validation only.

---

## Q3: Recommendations

### Best Model: K-Means (k=4)

### Cluster Descriptions

**Family Savers** (TARGET)
- Low util 12%, has 2+ kids
- Have credit but dont use it
- Biggest growth opportunity

**Credit Dependent**
- High util 68%, carries balance
- Revenue driver but risky

**Big Spenders**
- High transaction volume, pays regularly
- Already valuable, focus retention

**Mature Dormant**
- Older, no kids, barely uses card
- Hard to activate

### Marketing Strategy

**Family Savers** (Priority 1):
- Groceries cashback
- School fee installment
- Family dining rewards

**Mature Dormant** (Priority 2):
- Travel rewards
- Healthcare discounts

**Big Spenders** (Retention):
- Premium card upgrade
- Lounge access

**Credit Dependent** (Risk):
- Balance transfer offers
- Monitor for default signs

### Expected Impact

If Family Savers utilization goes from 12% to 25%, thats roughly 2x revenue from that segment.

---

## Technical Notes

- StandardScaler for all features
- LabelEncoder for categoricals
- Silhouette: K-Means 0.18, Hierarchical 0.14, HDBSCAN 0.21 (excl noise)
