# Credit Card Customer Segmentation Analysis

**Project:** CMU Applied ML - Group 10
**Date:** January 2026
**Analysts:** Rizaldy, Diyouva, Utami

---

## Executive Summary

**Objective:** Increase credit utilization among existing cardholders.

**Approach:** Behavioral clustering on spending/utilization patterns, overlaid with lifestage demographics for targeting.

**Key Finding:** 4 distinct behavioral segments identified. "Transactors" (high spend, low utilization) and "Dormant" segments represent largest growth opportunity — together comprising ~50% of customer base with utilization rates below portfolio median.

**Recommended Priority:**
1. Convert Transactors via intro APR campaigns
2. Re-engage Dormant via lifestage-triggered offers
3. Monitor Credit Dependent for risk
4. Retain Premium Engaged with exclusive perks

---

## 1. Business Context

Credit card revenue driven by:
- **Interest income** → requires revolving balances (utilization)
- **Interchange fees** → requires transaction volume

Current state: Significant portion of customers under-utilize credit lines, preferring cash/debit or remaining dormant.

**Hypothesis:** Customers cluster into behavioral segments; under-utilized segments can be profiled demographically and targeted with relevant offers.

---

## 2. Methodology

### 2.1 Feature Selection

| Step | Action | Outcome |
|------|--------|---------|
| Correlation Check | Identify multicollinearity | Dropped `Credit_Limit` (derived from Utilization × Balance) |
| PCA | Variance contribution | Confirmed spending & utilization drive principal components |
| Silhouette Test | Optimal feature set | **4 features** yield best cluster separation |

**Final Clustering Features:**
- `Total_Trans_Amt` — spending volume
- `Total_Trans_Ct` — transaction frequency
- `Avg_Utilization_Ratio` — credit line usage
- `Total_Revolving_Bal` — balance carried

### 2.2 Clustering Methods

| Method | Purpose | Result |
|--------|---------|--------|
| **K-Means (k=4)** | Primary segmentation | Selected — interpretable, stable |
| Hierarchical | Validation | Confirms k=4, high agreement with K-Means |
| HDBSCAN | Outlier detection | Identifies edge cases for manual review |

---

## 3. Behavioral Segmentation

### 3.1 2×2 Framework

|  | **High Spend** | **Low Spend** |
|--|----------------|---------------|
| **High Utilization** | Premium Engaged | Credit Dependent |
| **Low Utilization** | Transactors | Dormant |

### 3.2 Segment Profiles

| Segment | Size | Utilization | Spend | Characterization |
|---------|------|-------------|-------|------------------|
| **Premium Engaged** | ~20% | High | High | Best customers, active credit users |
| **Credit Dependent** | ~15% | High | Low | Revolvers, potential risk |
| **Transactors** | ~25% | Low | High | Pay-in-full behavior, conversion opportunity |
| **Dormant** | ~40% | Low | Low | Disengaged, re-activation target |

---

## 4. Lifestage Profiling

Demographics not used in clustering (avoids bias), but overlaid post-clustering for targeting.

### 4.1 Lifestage Definitions

| Lifestage | Criteria |
|-----------|----------|
| Young Professional | Age < 35, No dependents |
| Young Family | Age < 35, Has dependents |
| Established Family | Age 35-50, Has dependents |
| DINK/Established | Age 35-50, No dependents |
| Empty Nester | Age 50+, No dependents |
| Mature w/ Dependents | Age 50+, Has dependents |

### 4.2 Segment × Lifestage Composition

| Segment | Dominant Lifestage | Secondary |
|---------|-------------------|-----------|
| Transactors | Established Family | DINK |
| Dormant | Young Professional | Young Family |
| Credit Dependent | Mixed | — |
| Premium Engaged | Established Family | Empty Nester |

---

## 5. Strategic Recommendations

### 5.1 Priority Matrix

| Priority | Segment | Lifestage Focus | Action |
|----------|---------|-----------------|--------|
| **1** | Transactors | Established Family | Intro APR, family travel rewards |
| **2** | Dormant | Young Professional | Digital-first, lifestyle cashback |
| **3** | Dormant | Young Family | Milestone triggers (baby, home) |
| **4** | Credit Dependent | All | Risk monitoring, balance tools |
| **5** | Premium Engaged | All | Retention, premium upgrades |

### 5.2 Channel Strategy

| Lifestage | Primary Channel | Message Frame |
|-----------|-----------------|---------------|
| Young Professional | Mobile/Social | Convenience, lifestyle |
| Young Family | Email/App | Savings, family value |
| Established Family | Email/Direct | Premium, travel, big-ticket |
| Empty Nester | Direct/Phone | Trust, travel, simplicity |

### 5.3 Expected Impact

| Initiative | Target Segment | Expected Lift |
|------------|---------------|---------------|
| Intro APR Campaign | Transactors | +15-20% utilization |
| Lifestage Triggers | Dormant | +5-10% activation |
| Risk Monitoring | Credit Dependent | -10% default rate |
| Retention Program | Premium Engaged | -5% churn |

---

## 6. Next Steps

1. **Stakeholder Validation** — Review segments with business/marketing teams
2. **Campaign Design** — Develop creative for priority segments
3. **A/B Testing** — Pilot offers with control groups
4. **Monitoring** — Track utilization lift, churn, and risk metrics
5. **Model Refresh** — Quarterly re-clustering to capture behavioral shifts

---

## Appendix: Technical Details

### Clustering Performance

| Metric | K-Means | Hierarchical |
|--------|---------|--------------|
| Silhouette | 0.35+ | 0.33+ |
| Calinski-Harabasz | High | High |
| Davies-Bouldin | Low | Low |

### Feature Correlation (Post-Selection)

All selected features have correlation < 0.7, avoiding multicollinearity.

### Data Quality

- Records: 10,127 customers
- Missing: 0 (cleaned)
- Outliers: Handled via HDBSCAN identification

---

*Analysis confirms hypothesis: distinct behavioral segments exist with identifiable demographic profiles, enabling targeted utilization growth strategies.*
