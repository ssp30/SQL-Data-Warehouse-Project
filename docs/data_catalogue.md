# 📊 Data Catalog - MySQL Data Warehouse Project

## 🏗️ Project Overview
This Data Warehouse project follows a **Medallion Architecture (Bronze → Silver → Gold)** built using **MySQL**.

It integrates data from **CRM and ERP systems** and transforms raw data into analytics-ready datasets.

---

# 🥉 BRONZE LAYER (Raw Data)

## 📌 Purpose
The Bronze layer stores raw ingested data **as-is** from source systems without any transformation.

## 📂 Tables

### CRM Tables

#### `bronze_crm_cust_info`
Stores raw customer information from CRM system.

| Column | Data Type | Description |
|--------|----------|-------------|
| cst_id | INT | Customer ID |
| cst_key | VARCHAR | Unique customer key |
| cst_firstname | VARCHAR | First name |
| cst_lastname | VARCHAR | Last name |
| cst_marital_status | VARCHAR | Marital status |
| cst_gndr | VARCHAR | Gender |
| cst_create_date | DATE | Record creation date |

---

#### `bronze_crm_prd_info`
Stores product information.

| Column | Data Type | Description |
|--------|----------|-------------|
| prd_id | INT | Product ID |
| prd_key | VARCHAR | Product unique key |
| prd_nm | VARCHAR | Product name |
| prd_cost | INT | Product cost |
| prd_line | VARCHAR | Product line |
| prd_start_dt | DATETIME | Start date |
| prd_end_dt | DATETIME | End date |

---

#### `bronze_crm_sales_details`
Stores raw sales transactions.

| Column | Data Type | Description |
|--------|----------|-------------|
| sls_ord_num | VARCHAR | Order number |
| sls_prd_key | VARCHAR | Product key |
| sls_cust_id | INT | Customer ID |
| sls_order_dt | INT | Order date (YYYYMMDD) |
| sls_ship_dt | INT | Ship date |
| sls_due_dt | INT | Due date |
| sls_sales | INT | Sales amount |
| sls_quantity | INT | Quantity |
| sls_price | INT | Unit price |

---

### ERP Tables

#### `bronze_erp_cust_az12`
Customer demographic data.

| Column | Data Type | Description |
|--------|----------|-------------|
| cid | VARCHAR | Customer ID (may include prefix NAS) |
| bdate | DATE | Birth date |
| gen | VARCHAR | Gender |

---

#### `bronze_erp_loc_a101`
Customer location data.

| Column | Data Type | Description |
|--------|----------|-------------|
| cid | VARCHAR | Customer ID (may contain '-') |
| cntry | VARCHAR | Country code |

---

#### `bronze_erp_px_cust_cat_g1v2`
Product category mapping table.

| Column | Data Type | Description |
|--------|----------|-------------|
| id | VARCHAR | Category ID |
| cat | VARCHAR | Category name |
| subcat | VARCHAR | Sub category |
| maintenance | VARCHAR | Maintenance flag |

---
## 🏢 ERP TABLES
---

# 🥈 SILVER LAYER (Cleaned Data)

## 📌 Purpose
The Silver layer contains:
- Cleaned data
- Standardized values
- Deduplicated records
- Validated business rules

---

## 🔧 Key Transformations Applied

### Customer Data
- Removed duplicates using `ROW_NUMBER()`
- Trimmed text fields
- Standardized gender & marital status
- Removed NULL customer IDs

---

### Product Data
- Split product key into category + product code
- Handled missing cost using `IFNULL`
- Standardized product line values
- Derived product end date using `LEAD()` function

---

### Sales Data
- Converted integer dates to MySQL `DATE`
- Fixed invalid (0 or wrong format)
- Recalculated incorrect sales values:

---

### ERP DATA
- Removed `NAS` prefix from customer ID
- Standardized gender values
- Converted country codes:
- US → United States
- DE → Germany

---

## 📦 SILVER TABLES

- `silver_crm_cust_info`
- `silver_crm_prd_info`
- `silver_crm_sales_details`
- `silver_erp_cust_az12`
- `silver_erp_loc_a101`
- `silver_erp_px_cust_cat_g1v2`

---

# 🥇 GOLD LAYER (BUSINESS MODEL)

## 📌 Purpose
Star schema for analytics and reporting.

---

## ⭐ DIMENSION TABLES

### 🧑 `gold_dim_customers`
- Combines CRM + ERP customer data
- One record per customer
- Surrogate key used
| Column          | Data Type   | Description                                                                    |
| --------------- | ----------- | ------------------------------------------------------------------------------ |
| customer_key    | INT         | Surrogate key uniquely identifying each customer record in the dimension table |
| customer_id     | INT         | Unique numerical identifier assigned to each customer                          |
| customer_number | VARCHAR(50) | Alphanumeric identifier used for tracking customers                            |
| first_name      | VARCHAR(50) | Customer first name                                                            |
| last_name       | VARCHAR(50) | Customer last name                                                             |
| country         | VARCHAR(50) | Customer country of residence                                                  |
| marital_status  | VARCHAR(50) | Marital status (Married, Single, etc.)                                         |
| gender          | VARCHAR(50) | Gender (Male, Female, n/a)                                                     |
| birthdate       | DATE        | Date of birth (YYYY-MM-DD)                                                     |
| create_date     | DATE        | Record creation date                                                           |


---

### 📦 `gold_dim_products`
- Only active products included
- Includes category + subcategory mapping
| Column         | Data Type    | Description                         |
| -------------- | ------------ | ----------------------------------- |
| product_key    | INT          | Surrogate key for product dimension |
| product_id     | INT          | Unique product identifier           |
| product_number | VARCHAR(50)  | Business product code               |
| product_name   | VARCHAR(100) | Name of the product                 |
| category_id    | VARCHAR(50)  | Category ID from product key        |
| category       | VARCHAR(100) | Product category name               |
| subcategory    | VARCHAR(100) | Product sub-category                |
| maintenance    | VARCHAR(50)  | Maintenance flag                    |
| cost           | INT          | Product cost                        |
| product_line   | VARCHAR(50)  | Product line (Road, Mountain, etc.) |
| start_date     | DATE         | Product start/activation date       |


---

## 💰 FACT TABLE

### `gold_fact_sales`
- Central transaction table
- Connects:
- Customers
- Products
- Contains sales metrics
| Column       | Data Type   | Description                  |
| ------------ | ----------- | ---------------------------- |
| order_number | VARCHAR(50) | Unique sales order number    |
| product_key  | INT         | Foreign key to dim_products  |
| customer_key | INT         | Foreign key to dim_customers |
| order_date   | DATE        | Order placement date         |
| ship_date    | DATE        | Shipping date                |
| due_date     | DATE        | Expected delivery date       |
| sales_amount | INT         | Total sales amount           |
| quantity     | INT         | Units sold                   |
| price        | INT         | Unit price                   |


---

               gold.dim_customers
                        |
                        |
        gold.fact_sales (central fact table)
                        |
                        |
               gold.dim_products

---

---

# ⚙️ KEY DESIGN PRINCIPLES

## ✔ Data Quality
- No duplicate customers
- No invalid dates
- No negative sales
- Standardized categorical values

---

## ✔ MySQL Optimizations
- Indexes on join keys:
  - customer_id
  - product_key
  - sales customer reference

---

## ✔ Modeling Approach
- Star Schema design
- Surrogate keys in Gold layer
- Slowly changing product handling (via dates)

---

# 🚀 BUSINESS VALUE

This data warehouse enables:
- Customer analysis
- Sales performance tracking
- Product profitability analysis
- Location-based insights
- Business reporting dashboards

---

# 🛠️ TECH STACK
- MySQL
- SQL (Window Functions, Joins, CTEs)
- Data Warehousing Concepts


