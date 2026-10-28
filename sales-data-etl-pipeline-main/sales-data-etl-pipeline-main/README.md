# Sales Data ETL Pipeline & Analytics Dashboard

A portfolio-ready data engineering and analytics project that transforms raw retail sales data into a PostgreSQL star schema and exposes business insights through SQL and Streamlit.

## Architecture

**CSV → Python/Pandas ETL → Data Quality Checks → PostgreSQL Star Schema → SQL Analytics → Streamlit Dashboard**

See [docs/architecture.md](docs/architecture.md) for the data-flow and star-schema diagrams.

> **Sample data note:** The included 15,000-row dataset is synthetic. Metrics demonstrate the pipeline and should not be treated as real business results. See [the analytics review](docs/data_quality_and_metric_notes.md) for validation findings and interpretation caveats.

## Key features

- Extracts raw retail sales data from CSV
- Validates schema, nulls, duplicates, data types and business rules
- Transforms and standardizes sales fields with Pandas
- Loads a dimensional model into PostgreSQL
- Uses SQL joins, CTEs and window functions for analytics
- Calculates monthly revenue, running revenue, product rankings, AOV and repeat-customer rate
- Provides an interactive Streamlit dashboard with date/category/region filters, compact KPI formatting, and readable horizontal category/region charts
- Displays warehouse-wide checks for null measures, invalid quantities/prices/discounts, revenue arithmetic mismatches, and broken dimension links
- Includes pipeline tests and logging support

## Data warehouse

| Table | Type | Purpose |
|---|---|---|
| `fact_sales` | Fact | Transaction-level measures |
| `dim_date` | Dimension | Date, month, quarter and year attributes |
| `dim_customer` | Dimension | Customer attributes |
| `dim_product` | Dimension | Product and category attributes |
| `dim_region` | Dimension | Region and city attributes |

## Tech stack

- Python
- Pandas / NumPy
- PostgreSQL
- SQLAlchemy / psycopg2
- SQL
- Streamlit
- Plotly
- Pytest

## Project structure

```text
sales-data-etl-pipeline/
├── data/raw/sales.csv
├── data/processed/
├── etl/
│   ├── extract.py
│   ├── transform.py
│   ├── validate.py
│   ├── load.py
│   └── pipeline.py
├── sql/
│   ├── schema.sql
│   ├── analytics.sql
│   └── create_database.sql
├── dashboard/app.py
├── docs/
│   ├── architecture.md
│   └── interview_questions.md
├── tests/test_etl.py
├── requirements.txt
├── .env.example
└── README.md
```

## Local setup

### 1. Create and activate the virtual environment

```powershell
python -m venv .venv
.venv\Scripts\activate
```

### 2. Install dependencies

```powershell
python -m pip install -r requirements.txt
```

### 3. Create PostgreSQL database

Create a database named `sales_analytics` in PostgreSQL/pgAdmin.

Run `sql/schema.sql` against that database.

### 4. Configure credentials

Copy `.env.example` to `.env` and set your PostgreSQL password:

```text
DB_HOST=localhost
DB_PORT=5432
DB_NAME=sales_analytics
DB_USER=postgres
DB_PASSWORD=your_password
```

### 5. Run the ETL pipeline

```powershell
python etl\pipeline.py
```

### 6. Start the dashboard

```powershell
streamlit run dashboard\app.py
```

Open the local Streamlit URL shown in the terminal, normally `http://localhost:8501`.

## Analytics included

- Monthly revenue and order trend
- Revenue by category
- Revenue by region
- Top 10 products
- Product rank within category
- Average order value
- Repeat-customer rate
- Top customers
- Warehouse/pipeline health checks
