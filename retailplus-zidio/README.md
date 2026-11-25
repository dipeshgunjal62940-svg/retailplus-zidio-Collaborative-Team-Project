RetailPulse is a multi-page Streamlit platform that turns retail transaction data into decisions. It covers sales, regional and profit analysis, customer segmentation, churn prediction, demand forecasting, inventory optimization, live order monitoring and alerts. It has a dark theme and a sign-up / log-in gate.


Features
12 dashboard pages grouped into Understand, Customers, Plan and Operate
Customer analytics: RFM + K-Means segmentation with an action playbook per segment, and a churn model with revenue-at-risk
Forecasting: 7-60 day forecasts with what-if scenarios, comparing Holt-Winters, Gradient Boosting, an MLP, optional Prophet and an inverse-error weighted hybrid
Inventory: safety stock, reorder points, EOQ, ABC-XYZ classification and suggested purchase orders
Real-time analytics: a replayed live order stream with revenue per minute, demand-spike detection, live stock depletion and automatic purchase orders
MLOps: experiment log and model registry (MLflow optional), PSI / KS drift detection with a drift simulator, and a weekly Airflow retraining DAG
Accounts: salted PBKDF2-SHA256 password hashing and a 5-minute lockout after 5 failed attempts
Global filters for date range, region and channel
Exports to Excel and CSV
CI: GitHub Actions runs lint, tests and a Docker build



RetailPulse uses the public UCI / Kaggle "Online Retail II" transactions and extends them with engineered columns in src/data.py.

Download the dataset (UCI Online Retail, or Kaggle Online Retail / Online Retail II).
Place the .xlsx or .csv in data/raw/ (for example data/raw/online_retail_II.xlsx).
Run python -m src.data --build. This writes data/processed/retailpulse_enriched.csv.gz, which is what the app loads.

Engineered columns: product category (parsed from descriptions), region, channel, payment method, promotion calendar and discounts, holiday and season flags, unit cost, profit, supplier, lead time, satisfaction rating, customer demographics, acquisition channel, loyalty tier, margin %, price band, basket value, customer type, days since previous order, shipping cost, delivery days / status and return reason.

Important: columns that do not exist in the public data (channel, supplier, lead time, satisfaction, demographics, promotions, stock on hand, shipping and returns) are simulated. Generation is deterministic (hash-based), so results are reproducible. They demonstrate the full pipeline and must not be presented as real business facts.

Accounts

The app opens on a Log in / Sign up page, and no dashboard page is reachable until you are signed in.

Accounts live in a local SQLite file, data/users.db, which is git-ignored and excluded from Docker images.
Only a salted PBKDF2-SHA256 hash of each password is stored.
An account locks for 5 minutes after 5 wrong passwords.
Sessions last until you log out or refresh the browser tab.
There is no password-reset flow. Delete the user's row (or the file) to start again.

Project structure

retailpulse/
├── app.py                    # login gate, navigation (12 pages), global filters
├── views/                    # one file per page (login.py is the sign-in page)
├── src/
│   ├── data.py               # load, clean, enrich, quality checks
│   ├── auth.py               # sign-up / log-in (SQLite, hashed passwords, lockout)
│   ├── features.py           # RFM, K-Means segmentation, churn model
│   ├── forecast.py           # forecasting models and hybrid blend
│   ├── inventory.py          # safety stock, reorder point, EOQ, ABC-XYZ
│   ├── realtime.py           # live order stream engine
│   ├── drift.py              # PSI, KS tests, drift simulator
│   ├── tracking.py           # experiment log and model registry (MLflow optional)
│   ├── services.py           # cached wrappers around the heavy computations
│   └── ui.py                 # shared styling and data loading
├── airflow/dags/             # weekly retraining DAG
├── artifacts/                # experiment log (runs.json)
├── data/
│   ├── raw/                  # place the source dataset here
│   └── processed/            # enriched dataset used by the app
├── tests/                    # pipeline, auth, realtime tests and a page smoke test
├── .streamlit/config.toml    # dark theme and server settings
└── .github/workflows/ci.yml  # lint, test, Docker build

Method notes
Churn: features are computed at a cutoff date and the label is "no purchase in the following N days" (default 90). Recency is measured before the label window, so there is no target leakage.
Forecast metrics: reported on a 30-day holdout. MAPE skips zero-sales days, and WAPE is also shown because it is more stable for daily data.
Hybrid forecast: inverse-error weighted blend of the two best non-baseline models.
Inventory: safety stock = z × σ × √(lead time); reorder point = demand × lead time + safety stock; EOQ comes from annual demand, order cost and holding cost.
Drift: PSI below 0.10 is stable, 0.10-0.25 is a warning, and 0.25 or above is drift. A drift alert recommends retraining.
MLOps and retraining

airflow/dags/retailpulse_dag.py defines a weekly DAG (retailpulse_weekly, Sundays at 02:00):

ingest_clean  ->  check_drift  ->  train_models
                              \->  skip_training

It rebuilds the dataset, compares the last 60 days against earlier history, and retrains the churn and forecast models when PSI reaches 0.25 or on Sundays. Every run is logged through src/tracking.py. The DAG is a reference implementation: deploy it to an Airflow instance to run it.



Deploy to Streamlit Community Cloud
Commit data/processed/retailpulse_enriched.csv.gz (built from your raw file) so the app starts in seconds.
Push the repo to GitHub and open share.streamlit.io.
Choose the repo and set the main file to app.py.

Note that user accounts and the live stream state are stored locally or in memory, so they reset when the app is redeployed or restarted.

Tech stack

Streamlit · pandas · NumPy · scikit-learn · SciPy · statsmodels · Plotly · Matplotlib · SHAP · openpyxl · SQLite · Airflow (reference DAG) · Docker · GitHub Actions
