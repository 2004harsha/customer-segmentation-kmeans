# Customer Segmentation with K‑Means (RFM)

This project performs **customer segmentation** on the classic *Online Retail* dataset using the **RFM framework** (Recency, Frequency, Monetary) and **K‑Means clustering**. It includes outlier handling, feature scaling, elbow & silhouette analysis, and clear visualizations for segment interpretation.

> Built from the notebook: `Clustering and K-means.ipynb`

---

## ✨ What this project does

- Loads the *Online Retail* dataset (`Online_Retail.xlsx`).
- Cleans the data (drops null customer IDs, removes returns/cancellations and non-positive quantities/prices).
- Builds **RFM features** per customer:
  - **Recency** – days since the last purchase
  - **Frequency** – number of unique invoices
  - **Monetary** – total spend (Quantity × Price)
- Handles outliers with **IQR filtering** (separates outliers and non‑outliers).
- Scales features with **StandardScaler**.
- Determines **optimal `k`** via **Elbow (inertia)** and reports **Silhouette scores**.
- Fits **K‑Means** to non‑outliers (and separately to outliers), then **combines** results.
- Optionally maps numeric clusters to **human‑friendly segment names**.
- Produces helpful plots (boxplots, elbow curve, bar charts, 3D scatter) and exports `rfm_combined.csv`.

---

## 🗂️ Repository structure 

```
.
├── data/                # ( place dataset here)
├── notebooks/
│   └── Clustering and K-means.ipynb
├── outputs/             # csv + figures generated
├── requirements.txt
├── .gitignore
└── README.md

---

## ⚙️ Setup

1. **Clone** this repository and move into it.
2. (Recommended) **Create a virtual environment**:
   ```bash
   python -m venv .venv
   # Windows
   .venv\Scripts\activate
   # macOS/Linux
   source .venv/bin/activate
   ```
3. **Install dependencies**:
   ```bash
   pip install -r requirements.txt
   ```
4. **Place the dataset** at `data/Online_Retail.xlsx` (or update the path in the notebook).

---

## ▶️ How to run

Open Jupyter and run the notebook top‑to‑bottom.

```bash
jupyter lab
# or
jupyter notebook
```

Key outputs you’ll see:
- **Boxplot** of RFM features (to visualize spread and outliers).
- **Elbow curve** (inertia vs. number of clusters).
- **Silhouette scores** for `k=2..10`.
- **K‑Means** labels for non‑outliers and outliers (outlier clusters use negative labels).
- **3D scatter** and **bar charts** to interpret segments.
- Final CSV export: `outputs/rfm_combined.csv`.

---

## 🧠 Methodology 

- **RFM construction**
  - Snapshot date: one day after the max invoice date.
  - Group by `Customer ID` to compute `Recency`, `Frequency`, `Monetary`.
- **Outlier handling**
  - IQR rule per feature (1.5×IQR beyond Q1/Q3).
  - Split into `rfm_non_outliers` and `rfm_outliers`.
- **Scaling**
  - Standardize features with `StandardScaler` before K‑Means.
- **Model selection**
  - Use **Elbow** for a visual knee point.
  - Print **Silhouette score** for added validation.
- **Clustering**
  - Fit K‑Means on non‑outliers (e.g., `k=4` per elbow).
  - Fit a separate K‑Means on outliers; assign **negative** labels (e.g., `-1..-4`) to keep them distinguishable.
  - Concatenate back into `rfm_combined`.
- **Segment naming (optional)**
  - Map numeric clusters to business‑friendly names in the notebook (e.g., “Big Players”, “Loyal”, “At‑Risk”, “Cold”, etc.).
  - The mapping depends on the cluster profiles (mean R/F/M per cluster).

---

## 🔍 Interpreting clusters

Use the **cluster profile table** (mean Recency/Frequency/Monetary) to name clusters meaningfully. Typical patterns:
- **Low Recency (recent), High Frequency, High Monetary** → Champions / Big Spenders
- **Moderate Recency, Moderate Frequency, Moderate Monetary** → Good / Potential Loyalists
- **High Recency (stale), Low Frequency, Low Monetary** → Cold / At Risk

Always verify with your actual means/counts from `cluster_profile`.

---

## 🧪 Reproducibility

- Random seed fixed with `random_state=42` wherever K‑Means is used.
- Versions pinned in `requirements.txt` (see below).

---

## 📦 Requirements

These libraries are used in the notebook:

- `pandas`
- `numpy`
- `scikit-learn`
- `matplotlib`
- `seaborn`
- `openpyxl` (Excel reader/engine for `pandas.read_excel`)
- `jupyter` (to run the notebook)

Install via:
```bash
pip install -r requirements.txt
```

---

## 📝 Dataset

This notebook expects the **Online Retail** dataset (`Online_Retail.xlsx`). If you don’t want to commit the raw file, add it to `.gitignore` and provide a link or instructions for others to obtain it (e.g., from the UCI ML Repository).

---

## 🤝 Contributing

Issues and PRs are welcome. Ideas to improve:
- Automate the notebook into a script/CLI.
- Add PCA for 2D visualization.
- Parameterize outlier strategy (IQR vs. z‑score).
- Add tests and a Makefile for repeatable runs.

---

## 📜 License & Dataset Disclaimer

- This project is released for **educational and research purposes only**.  
- The dataset (`Online_Retail.xlsx`) is owned by its original provider (UCI Machine Learning Repository).  
- Please ensure you have rights to use the dataset before applying it in commercial or production settings.  
- The code in this repository is provided **as-is, without warranty**, under a permissive license of your choice (MIT recommended).  .

---

## 👤 Author

- **Harsha M Purohit** — feel free to connect or open an issue for questions.
