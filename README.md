# 📊 MySQL Data Warehouse Project (Medallion Architecture)

## 🧭 Data Flow

**Bronze → Raw Data Ingestion**  
**Silver → Cleaned + Standardized Data**  
**Gold → Business Ready Star Schema**

---

## 🏗️ Project Overview

This project implements a complete **end-to-end Data Warehouse solution using MySQL**, designed using the **Medallion Architecture (Bronze, Silver, Gold layers)**.

It integrates data from **CRM and ERP systems**, performs multiple transformation steps, and builds a final **Star Schema model optimized for analytics and reporting**.

The goal of this project is to demonstrate real-world **data engineering skills**, including ETL pipeline design, data cleaning, transformation, and dimensional modeling.

---

## ⚙️ Tech Stack

- MySQL
- SQL (Joins, CTEs, Window Functions)
- Data Warehousing Concepts
- Star Schema Modeling
- Data Cleaning & Transformation Logic

---

## 🏛️ Architecture Overview

### 🥉 Bronze Layer (Raw Data)
- Raw data ingestion from CRM and ERP systems
- No transformations applied
- Data stored as-is for traceability

### 🥈 Silver Layer (Cleaned Data)
- Data cleaning and standardization
- Handling duplicates using window functions
- Fixing invalid values and null handling
- Data type corrections and transformations
- Business rule validations applied

### 🥇 Gold Layer (Business Model)
- Final analytical layer
- Star schema design implemented
- Surrogate keys introduced
- Dimension and Fact tables created
- Optimized for BI tools and reporting

---

## 🔄 ETL Process Highlights

- Data extracted from multiple source systems (CRM + ERP)
- Data transformed using SQL logic
- Standardization of:
  - Gender values
  - Country names
  - Product categories
- Date conversions and validations
- Sales correction logic (`sales = quantity × price`)
- Load into structured warehouse tables

---

## 📦 Data Model

### 🧑 Dimension Tables
- Customer Dimension
- Product Dimension

### 💰 Fact Table
- Sales Fact Table

This follows a **Star Schema Design**, where fact table connects to multiple dimension tables.

---

## 🧪 Data Quality & Validation Checks

- Duplicate removal using `ROW_NUMBER()`
- Referential integrity checks between fact and dimensions
- Handling null and invalid values
- Ensuring sales consistency (`sales = quantity × price`)
- Filtering out incorrect or future dates

---

## 📊 Business Use Cases

This data warehouse supports:

- Customer segmentation and analysis
- Sales performance tracking
- Product profitability analysis
- Region-based insights
- KPI dashboards and BI reporting

---

## 📁 Project Structure

```bash
Data Warehouse Project/
├── bronze/              # Raw data layer scripts
├── silver/              # Cleaned transformation scripts
├── gold/                # Star schema (fact & dimension logic)
├── docs/                # Data catalog & documentation
├── init_database.sql    # Database setup script
└── README.md
```

---

## 📘 Documentation

For detailed schema, column-level definitions, and full transformation logic:

👉 See: `docs/data_catalog.md`

---

## 🚀 Key Highlights

- End-to-end Data Warehouse built using MySQL
- Real-world ETL pipeline implementation
- Medallion Architecture applied properly
- Star Schema design for analytics
- Strong focus on data quality and consistency
- Production-style SQL transformation logic

---

## ⭐ Why This Project Matters

This project simulates a real-world enterprise data warehouse where raw CRM & ERP data is transformed into a structured analytical model using industry-standard practices.

It demonstrates:
- ETL pipeline design
- Data modeling (Star Schema)
- Data quality enforcement
- SQL-based transformation logic

---

## 🎯 Learning Outcomes
Through this project, I learned:

- How to design a layered data warehouse architecture
- Writing advanced SQL (Window Functions, Joins, CASE logic)
- Building ETL pipelines using only SQL
- Data cleaning and transformation techniques
- Dimensional modeling and Star Schema design

---

## 🛠️ Project Status

✔ Completed End-to-End ETL Pipeline  
✔ Fully SQL-Based Data Warehouse  
✔ Portfolio & Interview Ready Project  

---

## 👨‍💻 Author

**Amaan Petiwala**  
MySQL | ETL | Data Warehousing
