# Stock Segmentation with Clustering

## Business problem

Trade & Ahead, a stock-market analysis firm, wants to help clients build diversified portfolios. This project groups 340 stocks into segments by their financial and price characteristics using unsupervised learning, so that stocks from different clusters can be combined to spread risk.

## Data

340 stocks with 15 columns: ticker, company, GICS sector and sub-industry, current price, price change, volatility, ROE, cash ratio, net cash flow, net income, earnings per share, estimated shares outstanding, P/E ratio, and P/B ratio.

## Approach

1. **Exploratory analysis:** distributions, outliers, and relationships between variables, including how price change and volatility vary by sector.
2. **Scaling:** `StandardScaler` on the numeric features so no one variable dominates the distance calculation.
3. **K-means clustering:** the number of clusters was chosen with the elbow method and silhouette scores, and the result profiled by cluster means.
4. **Hierarchical clustering:** compared linkage methods and distance metrics by cophenetic correlation, then cut the dendrogram for the chosen configuration (average linkage, Euclidean distance).
5. **Comparison** of the two approaches on cluster sizes, separation, and run time.

## Results

| Method | Clusters | Cluster sizes |
|--------|----------|---------------|
| K-means | 8 | 264, 27, 20, 11, 8, 5, 3, 2 |
| Hierarchical (average linkage) | 7 | 328 stocks in a single cluster, 12 spread over the rest |

K-means produced more distinct, better-separated groups and ran faster. Hierarchical clustering put nearly every stock in one cluster, which makes it less useful for segmentation. The notebook profiles each K-means cluster, for example separating high-volatility, high-growth names from stable, high-earning ones, and notes the sectors that dominate each.

## Takeaways

- Small clusters are mostly outliers, such as stocks with extreme price changes or volatility. They are worth treating as special cases when building a portfolio.
- The large main cluster holds the typical stock, and the smaller clusters offer contrasting risk and return profiles for diversification.
- Choosing between clustering methods should rest on interpretability and separation as well as on validation scores.

## Files

- `notebook.ipynb`: full analysis with saved outputs
- `data/README.md`: where to put the dataset to re-run the notebook
