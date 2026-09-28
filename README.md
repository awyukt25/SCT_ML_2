# Customer Segmentation using K-Means Clustering

## What this project does

Groups retail-store customers into distinct segments based on their purchase behaviour, so the marketing team knows which customers to target and how.

- **Dataset:** Kaggle *Mall Customers* (200 customers: Age, Gender, Annual Income, Spending Score)
- **Method:** K-Means clustering (unsupervised learning), cross-checked with Gaussian Mixture and Agglomerative clustering
- **Features used:** Annual Income (k$) and Spending Score (1-100), standardised with `StandardScaler`
- **Output:** a segment for every customer, a marketing action for each segment, and a saved model that can place new customers into a segment

---

## About "accuracy"

K-Means is **unsupervised**: there are no true labels, so a normal accuracy percentage does not exist. Model quality is measured with clustering metrics instead:

| Metric | What it tells you | Good value |
|---|---|---|
| **Silhouette Score** | How well separated the clusters are | Closer to 1 (above about 0.5 is generally considered reasonable) |
| **Davies-Bouldin Index** | How similar clusters are to each other | Lower |
| **Calinski-Harabasz Index** | Between-cluster vs within-cluster spread | Higher |
| **Stability (Adjusted Rand Index)** | Whether results stay the same across 20 random seeds | Close to 1.0 |

### Results of my run

| Metric | Value |
|---|---|
| Silhouette Score |0.555|
| Davies-Bouldin Index |0.572|
| Calinski-Harabasz Index |248.6|
| Stability (minimum ARI over 20 seeds) |1.000 mean and minimum|

---

## Customer segments

| Segment | Income | Spending | Suggested action |
|---|---|---|---|
| **Target Customers** | High | High | VIP and loyalty programme, early access, premium upselling |
| **Careful Spenders** | High | Low | Biggest growth opportunity: personalised, quality-focused offers |
| **Impulsive Spenders** | Low | High | Flash sales, limited editions, pay-later options |
| **Budget Conscious** | Low | Low | Discount coupons, bundles, low-cost retention |
| **Standard Customers** | Medium | Medium | General campaigns, cross-selling, seasonal offers |

Segment names are assigned automatically by comparing each cluster's average income and spending with the overall customer average (more than 0.5 standard deviations above is High, below is Low), so the names stay correct even if cluster numbers change between runs.

---

## Important things to know

- **Why Income + Spending Score only:** together they describe purchase behaviour. Age describes the customer, not how they buy, so it is used only to profile segments. The notebook tests this by comparing a model with Age (Model B) against the final model.
- **Why scaling matters:** K-Means is distance-based, so features must be on the same scale.
- **How K was chosen:** four metrics (Elbow, Silhouette, Davies-Bouldin, Calinski-Harabasz) vote on the number of clusters, instead of relying on the elbow plot alone.
- **Reproducible:** fixed `random_state = 42`.
- **Cross-checked:** if Gaussian Mixture and hierarchical clustering give similar groups (high ARI vs K-Means), the segments reflect real structure in the data.

---

## How to run

1. Open Google Colab and choose **File, then Upload notebook**, and select `Customer_Segmentation_KMeans.ipynb`.
2. Click **Runtime, then Run all**.
3. Upload `Mall_Customers.csv` when asked.
4. `customer_segments.csv` and `segment_playbook.csv` download at the end.

To run locally: `pip install numpy pandas matplotlib seaborn plotly scikit-learn joblib jupyter`, put the CSV next to the notebook, and open the notebook in Jupyter.

---

## Output files

| File | Contents |
|---|---|
| `customer_segments.csv` | Every customer with their cluster and segment name |
| `segment_playbook.csv` | Segment sizes, average income and spending, recommended actions |
| `kmeans_customer_segmentation.joblib` | Saved scaler, model and segment names for predicting new customers |

---

## Limitations

- Small dataset (200 customers) that appears synthetic.
- K-Means assumes roughly round clusters of similar size and is sensitive to outliers.
- Real retail segmentation would use transaction history (Recency, Frequency, Monetary value).

---
