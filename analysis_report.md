# Credit Card Customer Segmentation Analysis

**CMU Applied ML — Group 10**
**Rizaldy, Diyouva, Utami**

---

## Executive Summary

**Objective:** Identify customer segments to increase credit card utilization.

**Approach:** K-Means clustering on behavioral features, validated with Hierarchical clustering, enhanced with RFM scoring and Card Category analysis.

**Key Finding:** Four distinct behavioral segments identified. "Dolphin" and "Barnacle" segments (~50% of customer base) represent primary growth opportunity, particularly Blue/Silver cardholders with low utilization but demonstrated spending capacity.

---

## Question 1: K-Means Clustering

### 1.1 Feature Selection

**Hypothesis:** Customers can be segmented by spending and credit usage behavior. Under-utilized segments can be targeted for growth.

**Feature Selection Process:**

| Step | Method | Outcome |
|------|--------|---------|
| Correlation Analysis | Identify multicollinearity | Dropped `Credit_Limit` (derived from Util × Balance) |
| PCA | Variance contribution | Spending + Utilization drive PC1/PC2 |
| Silhouette Testing | Optimal feature set | 4 features yield best separation |

**Final Features (4):**

| Feature | Business Meaning |
|---------|------------------|
| `Total_Trans_Amt` | Spending volume |
| `Total_Trans_Ct` | Transaction frequency |
| `Avg_Utilization_Ratio` | Credit line usage |
| `Total_Revolving_Bal` | Balance carried |

> **📊 VIZ 1: Correlation Matrix Heatmap**
> Shows feature relationships and justifies dropping Credit_Limit

> **📊 VIZ 2: PCA Variance Explained**
> Cumulative variance plot showing 2 components capture majority of variance

> **📊 VIZ 3: Silhouette Score by Feature Set**
> Bar chart comparing silhouette scores across feature combinations

---

### 1.2 Optimal Cluster Size

**Methods Used:**

| Method | Optimal k | Rationale |
|--------|-----------|-----------|
| Elbow Method | 4 | Clear inflection point in inertia curve |
| Silhouette Analysis | 4 | Highest average silhouette score |

> **📊 VIZ 4: Elbow Plot**
> Inertia vs k with elbow at k=4 marked

> **📊 VIZ 5: Silhouette Score vs k**
> Line plot showing silhouette peaks at k=4

---

### 1.3 Cluster Exploration

**Behavioral Segmentation (2×2 Framework):**

|  | High Spend | Low Spend |
|--|------------|-----------|
| **High Utilization** | 🐋 Whale | 🦈 Shark |
| **Low Utilization** | 🐬 Dolphin | 🦪 Barnacle |

**Cluster Profiles:**

| Segment | Size | Avg Spend | Avg Trans | Utilization | Revolving Bal |
|---------|------|-----------|-----------|-------------|---------------|
| Whale | ~20% | High | High | High | High |
| Shark | ~15% | Low | Low | High | High |
| Dolphin | ~25% | High | High | Low | Low |
| Barnacle | ~40% | Low | Low | Low | Low |

**Key Differences:**
- **Whale** vs **Dolphin**: Similar spending, but Whale carries balances (revenue)
- **Shark** vs **Barnacle**: Both low spend, but Shark uses credit line
- **Dolphin**: High capacity, pay-in-full behavior — conversion opportunity

> **📊 VIZ 6: 2×2 Scatter Plot**
> Spend vs Utilization with quadrant labels and cluster colors

> **📊 VIZ 7: Cluster Boxplots**
> 4-panel boxplot showing each feature distribution by cluster

> **📊 VIZ 8: PCA Cluster Visualization**
> 2D scatter of PC1 vs PC2 with cluster centroids marked

---

## Question 2: Alternative Clustering (Hierarchical)

### 2.1 Method Selection

**Chosen:** Hierarchical Clustering (Ward linkage)

**Why Hierarchical over DBSCAN:**
- Dendrogram provides visual validation of cluster count
- Ward linkage minimizes within-cluster variance (similar objective to K-Means)
- DBSCAN challenged by uniform density in behavioral data

### 2.2 Results

| Metric | K-Means | Hierarchical |
|--------|---------|--------------|
| Silhouette | ~0.35 | ~0.33 |
| Adjusted Rand Index | — | 0.75+ (vs K-Means) |

**Interpretation:** High ARI indicates strong agreement between methods, validating K-Means results.

> **📊 VIZ 9: Dendrogram**
> Hierarchical tree with k=4 cut line marked

> **📊 VIZ 10: Side-by-Side Comparison**
> K-Means vs Hierarchical cluster assignments in PCA space

### 2.3 Challenges & Differences

| Challenge | Impact | Resolution |
|-----------|--------|------------|
| Computational cost | Slow on full dataset | Used sampling for dendrogram |
| No centroids | Harder to interpret "typical" customer | Used K-Means for final profiling |
| Hierarchical structure | Reveals sub-segments | Useful for future deep-dives |

**Decision:** K-Means selected as primary method — comparable performance, interpretable centroids, faster execution.

---

## Question 3: Recommendations

### 3.1 Segment Descriptions

#### 🐋 Whale (Revenue Drivers)
- **Profile:** High spend, high utilization, carries revolving balance
- **Demographics:** Established families, higher income
- **Value:** Highest revenue customers (interest + interchange)
- **Risk:** Monitor for over-extension

#### 🦈 Shark (Credit Reliant)
- **Profile:** Lower spend, but high utilization
- **Demographics:** Mixed, some financial stress indicators
- **Value:** Interest revenue
- **Risk:** Default risk, requires monitoring

#### 🐬 Dolphin (Conversion Opportunity)
- **Profile:** High spend, low utilization, pays in full
- **Demographics:** Established professionals, higher income
- **Value:** Interchange fees only — significant conversion opportunity
- **Opportunity:** Convert to revolving behavior

#### 🦪 Barnacle (Re-activation Target)
- **Profile:** Low engagement across all metrics
- **Demographics:** Skews younger, entry-level cards
- **Value:** Currently low
- **Opportunity:** Re-activation campaigns

> **📊 VIZ 11: Segment Profile Heatmap**
> Normalized metrics by cluster showing relative strengths

---

### 3.2 Card Category Analysis

| Segment | Dominant Card | Utilization | Opportunity |
|---------|---------------|-------------|-------------|
| Dolphin | Blue/Silver | 15-20% | High — upgrade + intro APR |
| Barnacle | Blue | 10-15% | High — re-activation |
| Barnacle | Gold/Platinum | 12-18% | Medium — retention risk |

> **📊 VIZ 12: Utilization Heatmap (Segment × Card)**
> Shows where utilization gaps exist by product

> **📊 VIZ 13: Card Mix Bar Chart**
> Card category distribution within each segment

---

### 3.3 RFM Enhancement

**RFM Scoring (1-5 each):**
- R (Recency): Inverse of months inactive
- F (Frequency): Transaction count
- M (Monetary): Transaction amount

| RFM Segment | Score | % of Dolphin | % of Barnacle |
|-------------|-------|--------------|---------------|
| Champions | 12-15 | High | Low |
| Loyal | 9-11 | Medium | Low |
| Potential | 6-8 | Low | Medium |
| At Risk | 3-5 | Low | High |

**Insight:** Dolphin skews Champions/Loyal (engaged but not using credit). Barnacle skews At Risk (disengaged).

> **📊 VIZ 14: Behavioral × RFM Cross-tab**
> Heatmap showing RFM distribution within each behavioral segment

---

### 3.4 Targeting Matrix

**Opportunity Score** = (Utilization Gap) × Segment Size × RFM Score

| Priority | Segment × Card | Opportunity | Recommended Action |
|----------|----------------|-------------|-------------------|
| 1 | Dolphin × Blue | High | Intro APR + Silver upgrade |
| 2 | Dolphin × Silver | High | Gold upgrade path |
| 3 | Barnacle × Blue | Medium | Cashback re-activation |
| 4 | Barnacle × Gold/Platinum | Medium | Personalized retention |
| 5 | Shark × All | Monitor | Balance tools, risk alerts |

> **📊 VIZ 15: Targeting Priority Bar Chart**
> Opportunity score by Segment × Card combination

---


### 3.5 Card Upgrade Strategy

> **📊 VIZ 16: Segment × Card Distribution (Stacked Bar)**
> Shows card tier mix within each behavioral segment

> **📊 VIZ 17: Upgrade Opportunity Map (Bubble Chart)**
> X = Segment, Y = Card Tier, Size = Count, Color = Utilization
> Low utilization + high count = upgrade opportunity

**Upgrade Logic (Segment-Driven):**

| Segment | Action | Rationale |
|---------|--------|-----------|
| 🐋 Whale | Upgrade +1 tier | High value, proven engagement |
| 🐬 Dolphin | Upgrade +1 tier | Convert to revolving via better rewards |
| 🦪 Barnacle | Hold | Re-activate first before upgrade |
| 🦈 Shark | Hold | Monitor credit risk |

**Upgrade Path:**
- Blue → Silver
- Silver → Gold
- Gold → Platinum
- Platinum → Platinum (max tier)

> **📊 VIZ 18: Current vs Recommended Card Distribution**
> Side-by-side bar chart showing card tier shift after applying upgrade logic

> **📊 VIZ 19: Upgrade Volume by Segment × Current Card**
> Bar chart showing number of customers eligible for upgrade by segment and current card tier

> **📊 VIZ 20: PCA Scatter - Recommended Card Coloring**
> Side-by-side PCA scatter plots: Current vs Recommended card tier coloring, showing upgrade impact in cluster space

> **📊 VIZ 21: Card Mix by Segment (Post-Upgrade)**
> Stacked bar chart showing new card tier proportions within each segment after applying upgrade logic

> **📊 VIZ 22: Sankey - Card Tier Movement**
> Flow diagram showing customer movement from Current Card → Recommended Card tier

**Upgrade Summary:**

| Current State | Target | Priority | Rationale |
|---------------|--------|----------|-----------|
| Dolphin × Blue | Silver | High | Proven spend, reward engagement |
| Dolphin × Silver | Gold | Medium | Loyalty recognition |
| Whale × Gold | Platinum | Medium | Upsell premium benefits |
| Barnacle × Blue | Silver | Low | Only post re-activation |
| Shark × Any | Hold | — | Monitor risk first |

### 3.6 Marketing Initiatives

| Segment | Initiative | Channel | Offer |
|---------|------------|---------|-------|
| Dolphin | Conversion Campaign | Email + App | 0% intro APR 12 months |
| Dolphin | Upgrade Path | Direct Mail | Waived annual fee on upgrade |
| Barnacle (Blue) | Re-activation | Digital + SMS | 5% cashback first 3 months |
| Barnacle (Premium) | Retention | Phone | Dedicated concierge, bonus points |
| Shark | Risk Management | App | Budgeting tools, alerts |

### 3.7 Product Improvements

| Segment | Product Enhancement |
|---------|---------------------|
| Dolphin | BNPL integration, installment options |
| Barnacle | Simplified rewards, lower complexity |
| Shark | Balance transfer products, hardship programs |
| Whale | Premium perks, exclusive access |

---

## Summary of Visualizations

| # | Visualization | Question Answered |
|---|---------------|-------------------|
| 1 | Correlation Matrix | Q1 - Feature selection |
| 2 | PCA Variance | Q1 - Feature selection |
| 3 | Silhouette by Features | Q1 - Feature selection |
| 4 | Elbow Plot | Q1 - Optimal k |
| 5 | Silhouette vs k | Q1 - Optimal k |
| 6 | 2×2 Scatter | Q1 - Cluster exploration |
| 7 | Cluster Boxplots | Q1 - Cluster exploration |
| 8 | PCA Clusters | Q1 - Cluster exploration |
| 9 | Dendrogram | Q2 - Hierarchical |
| 10 | K-Means vs Hierarchical | Q2 - Comparison |
| 11 | Segment Heatmap | Q3 - Recommendations |
| 12 | Util × Card Heatmap | Q3 - Recommendations |
| 13 | Card Mix Bar | Q3 - Recommendations |
| 14 | Behavioral × RFM | Q3 - Recommendations |
| 15 | Targeting Priority | Q3 - Recommendations |
| 16 | Segment × Card Stacked | Q3 - Upgrade strategy |
| 17 | Upgrade Opportunity Map | Q3 - Upgrade strategy |
| 18 | Current vs Recommended Card | Q3 - Upgrade strategy |
| 19 | Upgrade Volume by Segment | Q3 - Upgrade strategy |
| 20 | PCA Scatter - Recommended Card | Q3 - Upgrade strategy |
| 21 | Card Mix by Segment (Post-Upgrade) | Q3 - Upgrade strategy |
| 22 | Sankey - Card Tier Movement | Q3 - Upgrade strategy |

---

## Next Steps

1. Stakeholder validation of segment definitions
2. A/B test priority campaigns (Dolphin conversion)
3. Implement risk monitoring for Shark
4. Track KPIs: utilization lift, activation rate, churn
5. Quarterly model refresh

---

*Behavioral clustering + Card Category + RFM = actionable customer segmentation for utilization growth.*
