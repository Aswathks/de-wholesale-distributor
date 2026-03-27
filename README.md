Databricks Lakehouse: End-to-End Data Pipeline
An enterprise-grade data engineering pipeline leveraging a Medallion Architecture to transform raw Azure Blob Storage data into business-ready KPIs.
 Architecture Overview
This project follows the Lakehouse Layered Approach, moving data from raw ingestion to analytical gold cubes.
Raw Layer: Landing zone for Fivetran ingestion (Zero transformation).
Staging Layer: Initial cleaning, schema enforcement, and metadata removal.
Silver Layer: Business logic application, deduplication, and entity cleansing.
Gold Layer: Dimensional modeling (Stars/Snowflakes), Cubes, and KPI calculation.
 Project Structure & Workflow
1. Data Ingestion & Staging
Data is ingested via Fivetran into the azure_blob_storage schema.
Transformations Applied:
Cleanup: Dropped Fivetran metadata (_file, _line, etc.).
Standardization: trim(), upper/lower casing, and null handling (e.g., replacing NULL with NONE).
Validation:
Dates cast to proper DATE types.
Negative values in quantity, unit_price, and payment_amount reset to 0.
Invalid exchange rates defaulted to 1.0.
Audit: Added ingestion_ts and calculated line_total.
2. Silver: The "Source of Truth"
Modular notebooks process individual entities to ensure high data quality:
Customers, Products, Regions, Exchange Rates
Invoices, Invoice Line Items, Payments
3. Gold: Analytics & Insights
The final layer focuses on consumption-ready data structures:
Dimensional Model: dim_customers, dim_products, fact_sales, fact_payments.
The Data Cube: A pre-joined, high-performance dataset optimized for slicing by Status, Region, and Date.
KPI Suite: Automated calculation of business metrics.
📈 Key Business Metrics (KPIs)
The pipeline computes the following metrics automatically:
Financials: Total Revenue, Average Invoice Value, Discount Impact.
Growth: Top Customers/Products by Revenue, Revenue by Region.
Operational: Invoice Cancellation Rate, Order Frequency per Customer.
⚙️ Orchestration & Naming
Pipeline Execution Flow
Raw Validation → 2. Staging/Silver ETL → 3. Gold Modeling → 4. Cube Construction → 5. KPI Generation
Naming Conventions
Layer	Schema	Table Prefix	Notebook Example
Raw	azure_blob_storage	raw_*	nb_01_raw
Staging	staging	stg_*	nb_01_stg_all
Silver	silver	sl_*	nb_01_silver_customers
Gold	gold	dim_* / fact_*	nb_01_gold_dim_fact
Analytics	gold	cube_*	nb_02_gold_cube
🌟 Key Features
 Modular Design: Separate notebooks for specific entities simplify debugging.
 Robust Validation: Handles negative values and invalid rates at the staging gate.
 Analysis Ready: The Gold Cube removes the need for complex joins in BI tools (PowerBI/Tableau).
 Automated: Fully orchestrated via Databricks Jobs.
