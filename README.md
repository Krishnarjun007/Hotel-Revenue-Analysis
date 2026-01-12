# Hotel-Revenue-Analysis
# Hotel Revenue Analysis (2018-2020) - Milestone 1

## 📌 Project Overview
This project focuses on the transformation and modeling of historical hotel booking data. The objective of **Milestone 1** was to move from raw data extraction to a structured **Star Schema** to enable efficient and scalable business intelligence reporting.

---

## 🏗️ Data Architecture: The Star Schema
A Star Schema was implemented to optimize query performance and ensure data integrity. The model separates quantitative metrics (Facts) from descriptive attributes (Dimensions).

### 1. Fact Table
* **`Raw_fact`**: Contains all booking transactions, ADR, lead times, and foreign keys.

### 2. Dimension Tables
* **`Dim_Hotel`**: Contains hotel type details (Resort vs. City Hotel).
* **`Dim_Date`**: Standardized calendar for time-intelligence analysis.
* **`Dim_Room`**: Mapping of room types and properties.
* **`Dim_Customer`**: Unique guest profiles based on demographics.



---

## 🛠️ ETL & Data Engineering Steps
Using the **Power Query Editor**, the following transformations were performed:

### Data Consolidation & Cleaning
* **Append Queries**: Combined 2018, 2019, and 2020 datasets into the master `Raw_fact` table.
* **Error Handling**: Replaced errors in the `children` column with `0`.
* **Index Generation**: Added a unique `booking_id` for every record.

### Feature Engineering & Mapping
* **Cost Mapping**: Assigned numeric costs based on meal types (BB, HB, FB, SC).
* **Discount Logic**: Applied market segment discounts ranging from 10% to 100%.
* **ID Normalization**: Standardized `hotel_id` (H01/H02) and `room_id` (1-10).
* **Date Key**: Created a numeric key ($Year * 10000 + Month * 100 + Day$) for high-performance relationship linking.

### Customer ID Integration
A unique `customer_id` was created in the `Dim_Customer` table and merged back into the `Raw_fact` table using a multi-column join (Adults, Children, Babies, and Country).



---
