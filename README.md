# Hotel-Revenue-Analysis

📌 Project Overview
This project focuses on the transformation, relational modeling, and advanced visualization of hospitality data. The lifecycle progressed from raw data extraction and Star Schema modeling in Milestone 1 to the development of high-performance industry KPIs and behavioral guest segmentation using DAX in Milestone 2.

🏗️ Milestone 1: Data Architecture & ETL
The objective was to move from raw data to a structured Star Schema to optimize query performance and ensure data integrity.

Data Architecture
Fact Table (Raw_fact): The central hub containing ADR, Total Revenue, Lead Times, and Calculated Profit.

Dimension Tables:

Hotel: Branch-level data (Direct, OTA, Corporate).

Date: Standardized calendar for time-intelligence analysis.

Room: Market segments and dynamically assigned room categories.

Customer: Unique guest profiles based on demographics and country.

ETL & Engineering Steps
Regional Date Parsing: Resolved format conflicts using Locale-based transformations (English-UK).

Profit Engineering: Created a Calculated_Profit metric (Total_Revenue - Total_Costs).

Room Categorization: Engineered categories (Standard, Deluxe, Premium) based on ADR thresholds.

📊 Milestone 2: Revenue & Occupancy Analytics
Milestone 2 involved extending the model with complex DAX measures and interactive dashboards to track hospitality KPIs and guest behavior.

Key Performance Indicators (KPIs)
Occupancy %: Measures the percentage of available rooms sold.

Average Daily Rate (ADR): Measures the average rental income per occupied room.

RevPAR: Revenue per Available Room, assessing the hotel's ability to fill rooms at a specific rate.

Guest Segmentation Logic
Guest Type: Classified as Solo, Family, or Business based on adult/child counts.

Stay Type: Categorized into Short, Medium, or Long stays based on total nights.

Spend Type: Segmented into High, Medium, or Low spenders based on ADR thresholds.

Loyalty: Identified by analyzing the frequency of bookings per unique Customer ID.

📂 Repository Structure
Milestone 1/: Data Architecture, ETL documentation, and initial star schema.

Milestone 2/:

Screenshots/: Final dashboard views (Customer Report, O&R Metrics).

Updated Revenue analysis.pbix: The complete Power BI report.

README.md: Full project documentation.

