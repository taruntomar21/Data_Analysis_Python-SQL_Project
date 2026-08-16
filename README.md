# Customer Churn Analysis — OTT Subscription Platform
 
End-to-end **SQL + Python** data analytics project that identifies high-risk, churn-prone subscribers on a simulated OTT (streaming) platform, using multi-table relational data covering customer demographics, subscription details, and support interactions.
 
---
 
## Business Problem
 
In a hyper-competitive OTT market (Netflix, Hotstar, Prime, etc.), customer retention is critical to profitability. This project takes on the role of a Data Analyst tasked with:
- Identifying **who** is likely to churn
- Understanding **why** they churn
- Pinpointing **when** the "danger zone" begins
using demographic, subscription, and customer-support data.
 
---
 
## Dataset
 
Data is stored in a relational **SQLite** database (`customer_churn.db`) with three tables:
 
| Table | Key Fields |
|---|---|
| `db_customer` | customerid, name, country, state, gender, dob |
| `db_subscription` | customerid, subscription_start_date, plan_type, contract_type, cancellation_date, monthly_charges, cltv, churn_score |
| `db_support` | customerid, complaint_date, escalations, csat_score |
 
> **Note:** This is a small, illustrative dataset (21 customer records) built for practicing the end-to-end analytics workflow — not a production-scale dataset. The focus of the project is the *methodology* (SQL↔Python integration, cleaning, feature engineering, EDA, and insight generation), which is fully transferable to larger real-world datasets.
 
---
 
## Tools & Tech Stack
 
- **SQL** — SQLite (`sqlite3`) for relational data storage and extraction
- **Python** — `pandas`, `numpy` for data wrangling and feature engineering
- **Visualization** — `matplotlib`, `seaborn` for EDA and correlation analysis
- **Environment** — Jupyter Notebook
---
 
## Project Workflow
 
1. **Relational Data Extraction** — Connected Python to a SQLite database and pulled multiple related tables using SQL queries.
2. **Data Cleaning** — Fixed data types (e.g., `dob`, date columns), renamed columns, standardized categorical values (e.g., `Men`/`Women` → `Male`/`Female`), removed unused columns, and imputed missing `country` values using a `state → country` mapping derived from existing data.
3. **Table Merging** — Deduplicated the support table (multiple complaints per customer → most recent complaint + complaint count) and joined `customer`, `subscription`, and `support` tables into a single analysis-ready dataframe.
4. **Feature Engineering** — Created new calculated fields:
   - `churn_flag` (derived from `cancellation_date`)
   - `tenure_days` (subscription start → cancellation or today)
   - `churn_risk` (Low / Med / High, binned from `churn_score`)
5. **Exploratory Data Analysis** — Aggregations and group-bys to compute churn rate, retention rate, ARPU, revenue at risk, escalation rate, and churn-by-segment breakdowns.
6. **Correlation Analysis** — Encoded categorical variables **two ways** (naive label encoding vs. business-priority-ordered encoding) to demonstrate how encoding choice affects correlation interpretation, then visualized with a Seaborn heatmap and pairplot.
7. **Insight Reporting** — Summarized findings and recommended action items in a business-facing presentation.
---
 
## Key Metrics Calculated
 
| KPI | Formula |
|---|---|
| Churn Rate | churned customers ÷ total customers |
| Retention Rate | 1 − churn rate |
| Churn by Plan Type | churn rate grouped by `plan_type` |
| Churn by State | churn rate grouped by `state` |
| ARPU | SUM(monthly_charges) ÷ COUNT(active customers) |
| Avg. Customer Tenure | AVG(cancellation_date or today − subscription_start_date) |
| Revenue at Risk | SUM(monthly_charges) where churn_score > 70 |
| Escalation Rate | escalated complaints ÷ total complaints |
| Avg. Complaints per Customer | total complaints ÷ distinct customers |
| Escalation → Churn Correlation | churn rate with ≥1 escalation vs. 0 escalations |
 
---
 
## Key Insights
 
- **Churn Rate: 28.6%** | **Retention Rate: 71.4%**
- Churn is concentrated in the **Basic** plan — limited direct revenue impact
- Highest churn observed in **Karnataka**, with a spike in **September 2024**
- Average customer tenure: **1,451 days**; ARPU: **₹18.8**
- Revenue lost to churn: **~18% of total revenue**; CLTV lost: **2,047**
- **Monthly contracts churn far more than annual contracts** (55.6% vs. 8.3%) — the strongest single signal in the data, suggesting contract type is a key lever for retention
**Recommended Actions**
- Investigate the Karnataka churn spike (pricing changes, complaints, service issues)
- Review any recent price/plan changes around September
- Prioritize outreach (email/SMS/call) to **High** and **Med** churn-risk customers, ranked by CLTV
- Push customers on monthly contracts toward annual plans to improve retention
---
 
## Repository Structure
 
```
├── churn_analysis.ipynb                              # Full analysis notebook (SQL + Python)
├── cleaned_churn_data.csv                             # Cleaned, merged dataset (output of the pipeline)
├── Churn_Analysis_Report.pptx                         # Business-facing insights presentation
└── README.md
```
 
---
 
## How to Run
 
1. Clone the repo and install dependencies:
```bash
   pip install pandas numpy matplotlib seaborn
```
2. Open `churn_analysis.ipynb` in Jupyter and run cells top to bottom.
3. The notebook connects to `customer_churn.db` (SQLite), processes the data, and exports `cleaned_churn_data.csv`.
---
 
## Future Improvements
 
- Build a **predictive churn model** (e.g., logistic regression / random forest) on top of the existing engineered features
- Scale the analysis to a larger, real-world-sized dataset
- Turn the static PPTX insights into an interactive **Power BI / Tableau / Streamlit** dashboard
- Add automated data-quality checks for the ingestion pipeline
---
 
## Author
 
**Tarun Kumar**
📧 taruntomar9084@gmail.com | 🔗 [LinkedIn](https://www.linkedin.com/in/tarun-kumar-a48b642b7/) | 💻 [GitHub](https://github.com/tarun21)
