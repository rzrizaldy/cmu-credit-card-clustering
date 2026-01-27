# Credit Card Customer Clustering

A machine learning project for customer segmentation using clustering algorithms. This project analyzes credit card customer data to identify distinct customer groups for targeted marketing and product strategies.

## Project Overview

This project applies unsupervised learning techniques to segment credit card customers based on their behavioral and demographic attributes. The goal is to provide actionable insights for business decision-making.

### Clustering Methods Used

- **K-Means Clustering**: Primary segmentation method with multiple techniques for optimal k selection
- **Hierarchical Clustering**: Alternative approach using agglomerative clustering
- **DBSCAN**: Density-based clustering for identifying arbitrary-shaped clusters

## Dataset

The dataset contains ~10,000 credit card customers with the following features:

| Category | Features |
|----------|----------|
| Demographics | Customer Age, Gender, Dependent Count, Education Level, Marital Status, Income Category |
| Account Info | Card Category, Months on Book, Total Relationship Count |
| Activity | Months Inactive, Contacts Count, Total Transactions (Amount & Count) |
| Financial | Credit Limit, Revolving Balance, Avg Open to Buy, Utilization Ratio |

## Project Structure

```
.
├── ML_Foundations_Starter_Code_for_Clustering_Credit_Card_Customers.ipynb
├── credit_card_customers.csv
├── instruction.md
└── README.md
```

## Requirements

```
pandas
numpy
matplotlib
seaborn
plotly
scikit-learn
hdbscan
kneed
yellowbrick
scipy
```

## Getting Started

1. Clone this repository
2. Install dependencies: `pip install -r requirements.txt` (or install packages listed above)
3. Open the Jupyter notebook and run the cells

## Methods for Optimal Cluster Selection

- Elbow Method
- Silhouette Analysis
- Dendrogram Analysis (for Hierarchical)

## Course

CMU Applied Machine Learning - ML Foundations

## License

For educational purposes only.
