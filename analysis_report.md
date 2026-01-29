# Credit Card Customer Segmentation

**CMU Applied ML — Group 10**

---

## Executive Summary

**Objective:** Increase credit utilization.

**Approach:** Behavioral clustering → RFM scoring → Card Category analysis → Targeting

**Key Finding:** Transactors and Dormant segments (~50% of base) with Blue/Silver cards show highest opportunity score when weighted by RFM engagement.

---

## 1. Methodology

### Features (4)
| Feature | RFM Mapping |
|---------|-------------|
| Total_Trans_Amt | Monetary |
| Total_Trans_Ct | Frequency |
| Months_Inactive | Recency (inverse) |
| Avg_Utilization_Ratio | Utilization |

### Clustering
K-Means (k=4) validated by Hierarchical + HDBSCAN

---

## 2. Behavioral Segmentation

|  | High Spend | Low Spend |
|--|------------|-----------|
| **High Util** | Premium Engaged | Credit Dependent |
| **Low Util** | Transactors | Dormant |

---

## 3. RFM Analysis

**Scoring:** R + F + M (each 1-5 quintile)

| RFM Segment | Score | Profile |
|-------------|-------|---------|
| Champions | 12-15 | Best customers |
| Loyal | 9-11 | Consistent engagement |
| Potential | 6-8 | Room to grow |
| At Risk | 3-5 | Disengaged |

### Behavioral × RFM Cross-tab

Identifies which behavioral segments have high-value (Champions/Loyal) vs at-risk customers.

---

## 4. Card Category × Segment

| Segment | Card | Utilization | Insight |
|---------|------|-------------|---------|
| Transactors | Blue/Silver | 15-20% | Conversion opportunity |
| Dormant | Blue | 10-15% | Re-activation |
| Dormant | Gold/Platinum | 12-18% | Retention risk |

---

## 5. Targeting Matrix

**Opportunity Score** = (Util Gap) × Size × RFM Score

| Rank | Segment | Card | Util | RFM | N | Opportunity | Action |
|------|---------|------|------|-----|---|-------------|--------|
| 1 | Transactors | Blue | 0.18 | 9.2 | 1,200 | High | Intro APR + Upgrade |
| 2 | Transactors | Silver | 0.20 | 9.5 | 800 | High | Gold upgrade path |
| 3 | Dormant | Blue | 0.12 | 6.1 | 1,500 | Med | Cashback reactivation |
| 4 | Dormant | Gold | 0.15 | 7.8 | 400 | Med | Personalized retention |

---

## 6. Recommended Actions

| Priority | Target | Action |
|----------|--------|--------|
| 1 | Transactors × Blue/Silver | Intro APR, upgrade incentive |
| 2 | Dormant × Blue | Cashback re-activation |
| 3 | Dormant × Premium | VIP retention outreach |
| 4 | Credit Dependent | Balance tools, risk monitor |

---

## 7. Next Steps

1. A/B test priority actions
2. Track: utilization lift, activation rate, churn
3. Quarterly model refresh

---

*Behavioral segmentation + RFM + Card Category = actionable targeting framework.*
