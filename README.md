# Ecommerce-sales-analysis-Microsoft-fabric
End-to-end E-Commerce Sales Analytics using Microsoft Fabric, Notebook including PySpark, Medallion Architecture, Semantic Modal incu DAX, and Power BI.
#  Project Overview

- This project demonstrates an end-to-end data analytics workflow for an e-commerce business using Microsoft Fabric and Power BI.

- The objective is to transform raw e-commerce data into a structured analytical solution that supports business reporting and sales performance analysis.

-The project follows the Medallion Architecture, organising data into Bronze, Silver, and Gold layers. PySpark is used in Fabric Notebooks for data preparation and transformation, while the Gold layer provides fact and dimension tables for semantic modelling.

-A Power BI semantic model connects the analytical tables and supports DAX calculations, including sales performance metrics and time intelligence.

-  The final dashboard provides an interactive view of business performance across regions, product categories, brands, customer segments, and sales channels.

 #  Business Objectives

- The project aims to answer important business questions:

- What are the total net sales and total profit?
- Which regions generate the highest sales?
- Which product categories contribute most to revenue?
- How do brands compare in sales performance?
- Which customer segments contribute most to profit?
- How are sales performing month-to-date, quarter-to-date, and year-to-date?
- How does sales performance compare with the previous year?
- How can an analytical data model support consistent reporting?

 # Technology Stack
- Technology	Purpose
- Microsoft Fabric|	Unified data engineering and analytics environment
- Fabric Lakehouse	|Storage for Bronze, Silver, and Gold tables
- Fabric Notebooks|	Data processing and transformation
- Python	|Programming language for the notebook workflow
- PySpark	Distributed data transformation and processing
- Delta tables	Structured storage for Lakehouse data
- Medallion Architecture	Organising data into progressive quality layers
- Power BI Semantic Model	Data relationships and analytical calculations
- DAX	Measures and time intelligence
- GitHub	Source code, documentation, and portfolio presentation

# Project Architecture

The project follows a Medallion Architecture.

                SOURCE DATA
                     |
                     ▼
          ┌─────────────────────┐
          │    BRONZE LAYER     │
          │   Raw Source Tables │
          └─────────────────────┘
                     |
                     ▼
          ┌─────────────────────┐
          │    SILVER LAYER     │
          │ Cleaned and Prepared│
          │        Data         │
          └─────────────────────┘
                     |
                     ▼
          ┌─────────────────────┐
          │     GOLD LAYER      │
          │ Fact and Dimension  │
          │       Tables        │
          └─────────────────────┘
                     |
                     ▼
          ┌─────────────────────┐
          │  SEMANTIC MODEL     │
          │ Relationships + DAX │
          └─────────────────────┘
                     |
                     ▼
          ┌─────────────────────┐
          │   POWER BI REPORT   │
          │ Interactive Analysis│
          └─────────────────────┘

# Bronze Layer — Raw Data

- The Bronze layer stores the source data in separate Lakehouse tables.

- The source tables used in this project include:

- dbo.customer_master
- dbo.dataset_statistics_fixed
- dbo.ecommerce_sales_customer_analytics_150k
- dbo.order_items
- dbo.product_catalog

# Corresponding Bronze tables include:

- bronze_customer_master
- bronze_dataset_statistics
- bronze_ecommerce_sales
- bronze_order_items
- bronze_product_catalog

## Purpose:

- Preserve the source data in structured tables.
- Establish a starting point for downstream transformations.
- Organise the data for further processing.
- Silver Layer — Data Cleaning and Transformation

# The Silver layer contains prepared versions of the source tables.

## Examples include:

- silver_customer_master
- silver_dataset_statistics
- silver_ecommerce_sales
- silver_order_items
- silver_product_catalog

# Data preparation activities include:

- Handling duplicate records.
- Cleaning text fields.
- Checking missing values.
- Preparing identifiers for data modelling.
- Validating table structure and data quality.

# The cleaning rules should be documented for each table, including how missing values and duplicates are handled.

- Gold Layer — Analytical Data Model

- The Gold layer provides tables designed for reporting and analysis.

## Table	Purpose
- gold_dim_customer	Customer attributes and segmentation
- gold_dim_product	Product categories, brands, and product attributes
- gold_dim_date	Calendar attributes for time-based analysis
- gold_fact_sales	Sales and order-level analytical attributes
- gold_fact_order_items	Order-item quantities, revenue, costs, discounts, and profit
- gold_bridge_order	Distinct order identifiers used to connect the two fact tables

# Semantic Model

- The semantic model organises the Gold tables for business reporting.

## The main components are:

- Customer dimension: Supports analysis by customer attributes and segments.
- Product dimension: Supports analysis by product, category, and brand.
- Date dimension: Enables calendar-based reporting.
- Sales fact: Provides sales-related measures and attributes.
- Order-item fact: Provides item-level quantities and financial metrics.
- Order bridge: Connects sales and order-item records through order_id.
# The date relationship connects:

- gold_dim_date[date] → gold_fact_sales[order_date]

- The model also includes order relationships through gold_bridge_order.

- Relationship cardinality and filter direction should be validated in the semantic model. Avoid duplicate or ambiguous filtering paths, and ensure each measure uses the correct fact table.

# DAX Measures

## The project includes measures for sales, profitability, orders, and time-based performance.

### Total Net Sales
-Total Net Sales =
-SUM ( 'gold_fact_order_items'[net_sales] )
### Total Profit
-Total Profit =
- SUM ( 'gold_fact_order_items'[profit] )
  ### Total Quantity Sold
Total Quantity Sold =
SUM ( 'gold_fact_order_items'[quantity] )
### Total Orders
Total Orders =
DISTINCTCOUNT ( 'gold_bridge_order'[order_id] )
### Average Order Value
Average Order Value =
DIVIDE (
    [Total Net Sales],
    [Total Orders],
    0
)
### Profit Margin
Profit Margin % =
DIVIDE (
    [Total Profit],
    [Total Net Sales],
    0
)

# Format the profit margin measure as a percentage.

Year-to-Date Sales
Sales YTD =
TOTALYTD (
    [Total Net Sales],
    'gold_dim_date'[date]
)
### Month-to-Date Sales
Sales MTD =
TOTALMTD (
    [Total Net Sales],
    'gold_dim_date'[date]
)
### Quarter-to-Date Sales
Sales QTD =
TOTALQTD (
    [Total Net Sales],
    'gold_dim_date'[date]
)
### Previous Year Sales
Previous Year Sales =
CALCULATE (
    [Total Net Sales],
    SAMEPERIODLASTYEAR (
        'gold_dim_date'[date]
    )
)
### Year-over-Year Growth
YoY Sales Growth % =
DIVIDE (
    [Total Net Sales] - [Previous Year Sales],
    [Previous Year Sales],
    0
)

Note: These measures are examples from the project workflow. Validate the financial calculations against the actual data grain and ensure the date dimension has continuous dates and an active relationship with the relevant fact table.

# Dashboard Features

- The E-Commerce Sales Analysis dashboard provides an interactive view of sales performance.

# The report includes:

- Total net sales.
- Year-over-year sales growth.
- Month-to-date sales.
- Year-to-date sales.
- Quarter-to-date sales.
- Profit margin.
- Sales by region.
- Sales performance by brand.
- Sales by product category.
- Sales by product name.
- Profit by customer segment.
- Filtering by sales channel, year, and supplier.

These visualisations support the exploration of sales trends and comparisons across different business dimensions.



## Dashboard Preview

![E-Commerce Sales Analysis Dashboard](screenshots/ecommerce-sales-dashboard.png)

GitHub will display the screenshot directly in the README.

9. Repository Structure
ecommerce-sales-analysis-microsoft-fabric/
│
├── README.md
├── screenshots/
├── notebooks/
├── dax/
├── documentation/
├── .gitignore
└── LICENSE
notebooks/: Exported Fabric notebook code.
dax/: DAX measures and calculations.
documentation/: Architecture, modelling, and business documentation.
screenshots/: Dashboard images.
# Key Learning Outcomes

This project provides practical experience with:

Implementing a Bronze, Silver, and Gold data architecture.
Processing structured data with Python and PySpark.
Working with Lakehouse tables.
Cleaning and preparing analytical data.
Designing fact and dimension tables.
Building semantic model relationships.
Creating DAX measures.
Applying time intelligence functions.
Designing interactive business intelligence dashboards.
Documenting a data analytics project for version control.
# Future Improvements

- Potential improvements include:

- Automated data quality checks.
- Incremental data processing.
- Data pipeline orchestration.
- Additional validation of financial metrics.
- Performance optimisation for the semantic model.
- More advanced DAX calculations.
- Automated project documentation.
- Deployment and refresh monitoring.

# Dashboard Preview

<img width="1200" height="668" alt="Screenshot 2026-10-11 043311" src="https://github.com/user-attachments/assets/8c263644-908a-42d4-a421-970e234a5371" />

# Author

-Sushma VermaData, DataAnalytics

- This project demonstrates practical learning and implementation across Microsoft Fabric, PySpark, semantic modelling, DAX, and business intelligence.




