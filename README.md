# Hotel-Revenue-Analysis
# Hotel Revenue Analysis (2018-2020) - Milestone 1

📌 Project Overview
This project focuses on the transformation and relational modeling of hotel booking and marketing data. The objective of Milestone 1 was to move from raw data extraction to a structured Star Schema, ensuring regional date compatibility and engineering new behavioral features for high-performance reporting.

🏗️ Data Architecture: The Star Schema
A Star Schema was implemented to optimize query performance and ensure data integrity. The model separates quantitative metrics (Facts) from descriptive attributes (Dimensions).

Fact Table

Raw Fact: The central processing hub containing ADR, Total Revenue, Lead Times, and the engineered Calculated Profit.

Dimension Tables

hotel: Contains channel-level data (Direct, OTA, Corporate) representing business branches.

date: A standardized calendar for time-intelligence analysis, utilizing a numeric date_key.

room: Mapping of market segments and dynamically assigned room categories.

customer: Unique guest profiles based on demographics and country of origin.

🛠️ ETL & Data Engineering Steps
Using the Power Query Editor, the following transformations were performed to prepare the data for analysis:

Data Consolidation & Cleaning
Regional Date Parsing: Resolved DD/MM/YYYY format conflicts in the 2024 dataset using Locale-based transformation (English-UK).

Consolidation: Appended historical (2018-2020) and current operational records into a unified master Fact table.

Profit Engineering: Calculated a Calculated_Profit metric (Total_Revenue - Total_Costs) to fill reporting gaps.

Feature Engineering & Advanced Mapping
Room Categorization: Engineered a Room Category (Standard, Deluxe, Premium) based on ADR (Average Daily Rate) thresholds to track yield across pricing tiers.

Behavioral Segmentation: Developed Booking Duration and Stay Type (Short/Medium/Long) logic to categorize guest stay patterns.

Discount Logic: Applied market segment discounts and assigned numeric costs based on meal types (BB, HB, FB, SC).

Relational Mapping
Attribute-Based Merging: Dimension tables were integrated into the Raw Fact table using descriptive attributes (Booking_Channel, Market_Segment) to ensure seamless filtering.

Customer Integration: A unique customer_id was created in the customer dimension and merged into the Fact table using multi-column joins (Adults, Children, Babies, and Country).
