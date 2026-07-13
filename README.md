# Vendor Performance Analysis

An end-to-end data analytics project that analyzes vendor, purchase, and sales data to optimize inventory management, pricing strategy, and vendor relationships for a retail/wholesale business.

---

## 📌 Problem Statement

Effective inventory and sales management are critical for optimizing profitability in the retail and wholesale industry. Companies need to ensure that they are not incurring losses due to inefficient pricing, poor inventory turnover, or vendor dependency.

The goal of this analysis is to:

- Identify underperforming brands that require promotional or pricing adjustments.
- Determine top vendors contributing to sales and gross profit.
- Analyze the impact of bulk purchasing on unit costs.
- Assess inventory turnover to reduce holding costs and improve efficiency.
- Investigate the profitability variance between high-performing and low-performing vendors.

---

## 🏗️ Architecture

<img width="1407" height="910" alt="image" src="https://github.com/user-attachments/assets/2029039a-3a5a-4a4d-82ac-d4a04e9247fa" />

---

## 🛠️ Tech Stack

| Layer | Tools / Technologies |
|---|---|
| Data Ingestion | Python, Pandas, SQLAlchemy, SQLite |
| Data Storage | SQLite (`inventory.db`) |
| Data Analysis / EDA | Python, Pandas, NumPy, SciPy (Statistical Testing) |
| Data Visualization | Matplotlib, Seaborn |
| Business Intelligence Dashboard | Power BI (DAX, Power Query) |
| Reporting | Jupyter Notebook, PDF Report |
| Logging | Python `logging` module |

---

## 📂 Project Structure

```
├── ingestion_db.py                     # Loads raw CSV files into SQLite database
├── get_vendor_summary.py               # Builds & cleans the vendor sales summary table
├── Exploratory_Data_Analysis.ipynb     # EDA: summary stats, distributions, correlations
├── Vendor_Performance_Analysis.ipynb   # Core analysis: contribution, Pareto, bulk pricing, turnover, hypothesis testing
├── vendor_sales_summary.csv            # Final cleaned dataset used for analysis
├── vendor_performance.pbix             # Power BI interactive dashboard
├── Vendor_Performance_Report.pdf       # Final business report with findings & recommendations
└── README.md
```

---

## ⚙️ Data Pipeline

1. **Ingestion (`ingestion_db.py`)** — Reads all raw CSV files from a `data/` folder and loads them into a SQLite database (`inventory.db`), using the filename as the table name.
2. **Vendor Summary (`get_vendor_summary.py`)** — Joins `purchases`, `purchase_prices`, `sales`, and `vendor_invoice` tables via SQL (CTEs) to build a consolidated `vendor_sales_summary` table, and engineers new metrics:
   - `GrossProfit` = TotalSalesDollars − TotalPurchaseDollars
   - `ProfitMargin` = (GrossProfit / TotalSalesDollars) × 100
   - `StockTurnover` = TotalSalesQuantity / TotalPurchaseQuantity
   - `SalesToPurchaseRatio` = TotalSalesDollars / TotalPurchaseDollars
3. **EDA** — Summary statistics, distribution plots, and correlation heatmap to detect outliers, negative values, and zero-sales products.
4. **Data Filtering** — Removed inconsistent records where Gross Profit ≤ 0, Profit Margin ≤ 0, or Total Sales Quantity = 0.
5. **Analysis & Visualization** — Vendor contribution (Pareto analysis), bulk-purchase impact on unit cost, inventory turnover, and statistical hypothesis testing (Two-Sample T-Test).
6. **Dashboard** — Power BI dashboard for interactive exploration of vendor and brand KPIs.

---

## 🔍 Key Findings

### 1. Brands for Promotional or Pricing Adjustments
198 brands show **low sales but high profit margins**, indicating strong potential for targeted marketing, promotions, or price optimization to increase sales volume without hurting profitability.

### 2. Top Vendors by Sales & Purchase Contribution
The **top 10 vendors contribute 65.69%** of total purchases, while the remaining vendors contribute only 34.31%. This over-reliance introduces supply chain risk and highlights the need for vendor diversification.

### 3. Impact of Bulk Purchasing on Cost Savings
Vendors buying in large quantities receive a **72% lower unit cost** ($10.78/unit for large orders vs. $39.06/unit for small orders), confirming that bulk pricing strategies significantly reduce cost per unit.

| Order Size | Unit Purchase Price |
|---|---|
| Small | $39.06 |
| Medium | $15.49 |
| Large | $10.78 |

### 4. Vendors with Low Inventory Turnover
**Total Unsold Inventory Capital: $2.71M.** Several vendors (e.g., Alisa Carr Beverages, Highland Wine Merchants) show stock turnover below 1, indicating slow-moving inventory and tied-up capital.

### 5. Profit Margin: High vs. Low-Performing Vendors
| Vendor Group | 95% Confidence Interval | Mean |
|---|---|---|
| Top-performing vendors | (30.74%, 31.61%) | 31.17% |
| Low-performing vendors | (40.48%, 42.62%) | 41.55% |

Low-performing vendors maintain **higher margins but lower sales volume**, suggesting pricing or market-reach inefficiencies.

### 6. Statistical Validation
A **Two-Sample T-Test** rejected the null hypothesis (p-value ≈ 0.0000), confirming a **statistically significant difference** in profit margins between top and low-performing vendors — the two groups operate under distinctly different profitability models.

---

## ✅ Final Recommendations

- Re-evaluate pricing for low-sales, high-margin brands to boost sales volume without sacrificing profitability.
- Diversify vendor partnerships to reduce dependency on a few suppliers and mitigate supply chain risks.
- Leverage bulk purchasing advantages to maintain competitive pricing while optimizing inventory management.
- Optimize slow-moving inventory by adjusting purchase quantities, launching clearance sales, or revising storage strategies.
- Enhance marketing and distribution strategies for low-performing vendors to drive higher sales volumes without compromising profit margins.

---

## 📊 Power BI Dashboard

The `vendor_performance.pbix` file contains an interactive dashboard covering:
- Total Sales, Total Purchase, Gross Profit, Profit Margin, Unsold Capital (KPI cards)
- Purchase Contribution % (Donut Chart)
- Top Vendors & Top Brands by Sales
- Low Performing Vendors (Stock Turnover)
- Low Performing Brands (Scatter Plot — Sales vs. Profit Margin, with Target Brand flag)

---

## 🚀 How to Run

1. Place raw CSV files in a `data/` folder.
2. Run ingestion script to load data into SQLite:
   ```bash
   python ingestion_db.py
   ```
3. Build the cleaned vendor summary table:
   ```bash
   python get_vendor_summary.py
   ```
4. Open `Exploratory_Data_Analysis.ipynb` and `Vendor_Performance_Analysis.ipynb` in Jupyter to reproduce the analysis and visualizations.
5. Open `vendor_performance.pbix` in Power BI Desktop to explore the interactive dashboard.
