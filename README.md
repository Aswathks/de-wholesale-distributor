---

## Layered Architecture

### 1. Raw Layer
- Data ingested using Fivetran
- No transformations

### 2. Staging Layer
- Removed metadata columns (_file, _line, _modified, _fivetran_synced)
- Cleaned text (lower, upper, trim)
- Handled null values
- Converted date columns
- Validated numeric fields
- Added `ingestion_ts`
- Created `line_total` column

### 3. Silver Layer
- Cleaned and structured data
- Prepared for modeling
- Created separate tables for each entity

### 4. Gold Layer
- Created **dimension tables**
  - dim_customers
  - dim_products
  - dim_regions
- Created **fact tables**
  - fact_sales
  - fact_payments

---

## Data Cube
- Built after fact and dimension tables
- Joined all tables into a single analytical dataset
- Supports slicing by:
  - customer
  - product
  - region
  - date
  - invoice status

---

## KPIs
- Total Revenue
- Total Quantity Sold
- Average Invoice Value
- Top Customers by Revenue
- Top Products by Revenue
- Revenue by Region
- Invoice Cancellation Rate
- Discount Impact

---

## Orchestration
Databricks Job created with task dependencies:

1. Staging
2. Silver
3. Gold (dim + fact)
4. Data Cube
5. KPI

---

## Key Features
- End-to-end pipeline using Azure + Fivetran
- Lakehouse architecture
- Modular notebook design
- Dimensional modeling
- Data cube for analytics
- KPI generation

---

## Naming Conventions
Schemas:
- staging
- silver
- gold

Tables:
- stg_*
- dim_*
- fact_*
- cube_*

---

## Conclusion
This project demonstrates a complete data engineering workflow from ingestion to analytics using modern tools and best practices.
