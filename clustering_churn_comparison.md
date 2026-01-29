# Clustering Comparison: With vs Without Churned Customers

**Rizaldy | CMU Applied ML**

---

## Dataset Overview

| Dataset | Customers | Description |
|---------|-----------|-------------|
| Full | ~10,100 | All customers including churned |
| No-Churn | ~8,500 | Existing customers only |

Churned customers: ~1,600 (16% of total)

---

## Why Compare?

Churned customers behave differently. Including them might:
- Skew clusters toward churn patterns
- Mix "at-risk" signals with normal usage patterns
- Create segments that blend churn prediction with utilization targeting

We want to know: does excluding churn give cleaner segments for marketing?

---

## K-Means Results Comparison

### Full Dataset (with churn)

| Cluster | Name | Util | Trans Amt | Size |
|---------|------|------|-----------|------|
| 0 | Family Savers | 12% | $3,800 | 25% |
| 1 | Credit Dependent | 68% | $4,200 | 20% |
| 2 | Big Spenders | 35% | $5,500 | 30% |
| 3 | Mature Dormant | 8% | $2,900 | 25% |

Silhouette: 0.18

### No-Churn Dataset

| Cluster | Name | Util | Trans Amt | Size |
|---------|------|------|-----------|------|
| 0 | Family Savers | 14% | $4,100 | 27% |
| 1 | Credit Dependent | 65% | $4,500 | 18% |
| 2 | Big Spenders | 38% | $5,800 | 32% |
| 3 | Mature Dormant | 10% | $3,200 | 23% |

Silhouette: 0.19

---

## Key Differences

### 1. Utilization Rates Shift Up

Without churned customers, average utilization increases slightly across all clusters. Churned customers tend to have lower engagement before leaving.

| Cluster | Full | No-Churn | Delta |
|---------|------|----------|-------|
| Family Savers | 12% | 14% | +2% |
| Credit Dependent | 68% | 65% | -3% |
| Big Spenders | 35% | 38% | +3% |
| Mature Dormant | 8% | 10% | +2% |

### 2. Transaction Amounts Higher

Existing customers transact more on average. Churned customers likely reduced spending before leaving.

### 3. Cluster Sizes Adjust

- Big Spenders grows (32% vs 30%): more active customers remain
- Credit Dependent shrinks (18% vs 20%): some high-util churned

### 4. Silhouette Slightly Better

No-churn dataset: 0.19 vs 0.18. Cleaner separation without the noise from churned customers.

---

## Churned Customer Profile

Before deciding which dataset to use, look at who churned:

| Metric | Churned Avg | Existing Avg |
|--------|-------------|--------------|
| Utilization | 15% | 28% |
| Trans Count | 45 | 68 |
| Trans Amount | $3,200 | $4,800 |
| Contacts (12mo) | 2.8 | 2.3 |
| Inactive Months | 2.4 | 2.1 |

Churned customers had:
- Lower utilization
- Fewer transactions
- More contacts with bank (complaints?)
- Slightly more inactive months

---

## Which Dataset to Use?

**It depends on your goal:**

### Use Full Dataset (with churn) when:
- Building churn prediction model
- Want to identify at-risk segments
- Need to understand full customer base

### Use No-Churn Dataset when:
- Targeting utilization growth campaigns
- Designing marketing for active customers
- Want cleaner behavioral segments

---

## Recommendation

For our business goal (increase utilization), use **No-Churn dataset**.

Reasons:
1. Cleaner segments without churn noise
2. Focus marketing on customers who will actually respond
3. Churned customers wont respond to campaigns anyway
4. Slightly better silhouette score

For churn prevention, build a separate classification model using full dataset with Attrition_Flag as target.

---

## Summary Table

| Aspect | Full Dataset | No-Churn |
|--------|--------------|----------|
| Customers | 10,100 | 8,500 |
| Silhouette | 0.18 | 0.19 |
| Avg Utilization | 25% | 28% |
| Best for | Churn analysis | Marketing campaigns |
| Cluster clarity | Mixed signals | Cleaner separation |

---

## Next Steps

1. **Marketing Team**: Use no-churn clusters for campaign targeting
2. **Risk Team**: Build separate churn model on full dataset
3. **Product Team**: Analyze churned customer feedback for improvements
