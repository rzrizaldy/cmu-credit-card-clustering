# Credit Card Customer Segmentation

CMU Applied ML Project - Group 10: Rizaldy, Diyouva, Utami

## Overview

Behavioral clustering of 8,500 credit card customers to drive utilization growth. Churned customers excluded to focus on actionable strategies for the active portfolio.

**Live Dashboard:** [View on GitHub Pages](https://your-username.github.io/cmu_applied_ml_project_1/dashboard/)

## Segments

| Segment | Size | Avg Spend | Utilization | Strategy |
|---------|------|-----------|-------------|----------|
| Whale | 34% | $4,509 | 54% | Retain - upgrade to premium |
| Dolphin | 10% | $13,824 | 17% | Convert - intro APR offers |
| Shark | 25% | $1,932 | 32% | Monitor - credit risk |
| Barnacle | 31% | $3,973 | 6% | Activate - re-engagement |

## Methods

- K-Means clustering on 4 behavioral features
- Hierarchical clustering for validation (ARI > 0.75)
- Silhouette score optimization (k=4)

## Files

```
├── applied_ml_creditcard_group10.ipynb   # Main analysis
├── cluster_data.csv                       # Segmented customer data
├── dashboard/index.html                   # Interactive dashboard
├── credit_card_customers.csv              # Raw data
└── analysis_report.md                     # Detailed findings
```

## Dashboard

Interactive visualization built with Chart.js and D3.js. Features:
- Cluster scatter plot (Spend vs Utilization)
- Segment profiles and metrics
- Card tier upgrade flow (Sankey)
- Demographic breakdowns
- 100% stacked comparisons

## Deploy to GitHub Pages

1. Push your code to GitHub:
```bash
git add .
git commit -m "Add segmentation dashboard"
git push origin main
```

2. Enable GitHub Pages:
   - Go to repository Settings
   - Click "Pages" in left sidebar
   - Source: Deploy from a branch
   - Branch: `main`
   - Folder: `/ (root)`
   - Click Save

3. Access dashboard at:
```
https://<your-username>.github.io/<repo-name>/dashboard/
```

Note: GitHub Pages may take 1-2 minutes to deploy after enabling.

## Requirements

```
pandas
numpy
matplotlib
seaborn
scikit-learn
scipy
hdbscan
kneed
yellowbrick
plotly
```

## License

Educational use only - CMU Applied Machine Learning
