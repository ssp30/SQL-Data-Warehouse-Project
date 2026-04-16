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

#### `bronze_erp_loc_a101`
Customer location data.

#### `bronze_erp_px_cust_cat_g1v2`
Product category mapping table.

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
- Derived end date using `LEAD()` function

---

### Sales Data
- Converted integer dates to MySQL `DATE`
- Fixed invalid or zero dates
- Recalculated incorrect sales values:
