# Hotel Business Revenue & Guest Insights Analysis (2018-2020)

## 📌 Project Overview
This project focuses on the transformation, relational modeling, and advanced visualization of hospitality data. The lifecycle progressed from raw data extraction and **Star Schema** modeling in Milestone 1 to the development of high-performance industry KPIs and behavioral guest segmentation using DAX in Milestone 2.

---

## 🏗️ Milestone 1: Data Architecture & ETL
The objective was to move from raw data to a structured Star Schema to optimize query performance and ensure data integrity.

### Data Architecture
* **Fact Table (Raw_fact)**: The central hub containing ADR, Total Revenue, Lead Times, and Calculated Profit.
* **Dimension Tables**:
    * **Hotel**: Branch-level data (Direct, OTA, Corporate).
    * **Date**: Standardized calendar for time-intelligence analysis.
    * **Room**: Market segments and dynamically assigned room categories.
    * **Customer**: Unique guest profiles based on demographics and country.

### ETL & Engineering Steps
* **Regional Date Parsing**: Resolved format conflicts using Locale-based transformations (English-UK).
* **Profit Engineering**: Created a `Calculated_Profit` metric (`Total_Revenue - Total_Costs`).
* **Room Categorization**: Engineered categories (Standard, Deluxe, Premium) based on ADR thresholds.

---

## 📊 Milestone 2: Revenue & Occupancy Analytics
Milestone 2 involved extending the model with complex DAX measures and interactive dashboards to track hospitality KPIs and guest behavior.

### Key Performance Indicators (KPIs)
* **Occupancy %**: Measures the percentage of available rooms sold.
* **Average Daily Rate (ADR)**: Measures the average rental income per occupied room.
* **RevPAR**: Revenue per Available Room, assessing the hotel's ability to fill rooms at a specific rate.

### Guest Segmentation Logic
* **Guest Type**: Classified as Solo, Family, or Business based on adult/child counts.
* **Stay Type**: Categorized into Short, Medium, or Long stays based on total nights.
* **Spend Type**: Segmented into High, Medium, or Low spenders based on ADR thresholds.
* **Loyalty**: Identified by analyzing the frequency of bookings per unique Customer ID.

---

## 📂 Repository Structure
Based on the project hierarchy:
* **Milestone 1/**
    * `Data/`: Raw source files (e.g., Hotel book.csv).
    * `Documentation/`: Milestone 1 Project Report.
    * `Images/`: Star Schema and initial trend charts.
* **Milestone 2/**
    * `Screenshots/`: Final dashboard views (Customer Report, O&R Metrics).
    * `Updated Revenue analysis.pbix`: The complete Power BI report.
* **README.md**: Full project documentation.

---

## 💻 Technical Implementation (DAX Queries)
### **I.  Calculated Columns & Logic**
```
// 1. Seasonal Classification
Season = SWITCH( TRUE(), 
    MONTH(Raw_fact[arrival_date]) IN {12,1,2}, "Winter", 
    MONTH(Raw_fact[arrival_date]) IN {3,4,5}, "Summer", 
    MONTH(Raw_fact[arrival_date]) IN {6,7,8}, "Monsoon", 
    "Autumn" 
)

// 2. Booking Channel Grouping
Booking_channel = IF(Raw_fact[distribution_channel] = "Direct", "Direct", "OTA")

// 3. Total Length of Stay
Total Nights = Raw_fact[stays_in_week_nights] + Raw_fact[stays_in_weekend_nights]

// 4. Spend Type Segmentation
Spend_Type = IF( Raw_fact[ADR] >= 150, "High Spender", 
             IF( Raw_fact[ADR] >= 80, "Medium Spender", "Low Spender" ) )

// 5. Guest Type (Custom Column Logic)
Guest_type = 
    if [adults] = 1 and [children] = 0 and [babies] = 0 then "Solo" 
    else if [adults] >= 2 and ([children] > 0 or [babies] > 0) then "Family" 
    else "Business"
```

### **II. Measures (KPIs & Calculations)**
```dax
// 1. Total Rooms in Inventory
Rooms Available = COUNTROWS(Room)

// 2. Total Bookings Sold
Rooms Sold = COUNT(Raw_fact[booking_id])

// 3. Occupancy Rate across time
Avg Daily Occupancy % = AVERAGEX( 
    VALUES( 'Date'[arrival_date] ), 
    DIVIDE([Rooms Sold], [Rooms Available]) 
)

// 4. Total Room Revenue
Room Revenue = SUM( Raw_fact[revenue] )

// 5. Revenue Per Available Room
RevPAR = DIVIDE( [Room Revenue], [Rooms Available] )

// 6. Loyalty: Total Bookings per Guest
Total Bookings = CALCULATE( 
    COUNTROWS(Raw_fact), 
    FILTER( ALL(Raw_fact), Raw_fact[Customer.1.Customer_id] = MAX(Raw_fact[Customer.1.Customer_id]) ) 
)

// 7. Cancellation Metrics
Cancelled Bookings = CALCULATE(COUNT(Raw_fact[booking_id]), Raw_fact[is_canceled]=1)
Cancellation % = DIVIDE([Cancelled Bookings], [Total Bookings])

// 8. Individual Guest Average ADR
Avg ADR = CALCULATE( 
    AVERAGE(Raw_fact[ADR]), 
    FILTER( ALL(Raw_fact), Raw_fact[Customer.1.Customer_id] = MAX(Raw_fact[Customer.1.Customer_id]) ) 
)
```

### 🏨 Infosys Springboard Virtual Internship 6.0 | Milestone 3

This repository contains the **Hotel Revenue Analysis and Forecasting** dashboard developed during Milestone 3 of the Infosys Springboard Internship. The project focuses on transforming historical booking data into actionable insights and future demand predictions using **Power BI** and the **Prophet** forecasting model.

## 📊 Dashboard Overview
The dashboard provides a comprehensive view of hotel performance, tracking key metrics across historical and forecasted timelines.

* **Total Forecasted Bookings**: A global KPI showing predicted future demand beyond the historical data range.
* **Forecasting Trends**: Area charts showing the evolution of `yhat` (forecast), including upper and lower confidence intervals.
* **Cancellation Analysis**: Visualizes monthly cancellation trends to assist in revenue protection strategies.
* **Interactive Slicers**: Filters for Hotel Type, Country, and Booking Channel for granular data exploration.

## 🛠️ Technical Implementation & DAX
During development, specific DAX challenges were addressed to ensure accurate data representation and context handling.

### 1. Global Forecast Logic
To prevent the forecast card from showing blank values (`--`), a variable-based approach was used to identify the "End of History" across the entire dataset, ignoring local row filters:

### 📂 Repository Structure

Based on the project hierarchy:

* **Milestone 1/**
    * `Data/` : Raw source files (e.g., Hotel book.csv).
    * `Documentation/` : Milestone 1 Project Report.
    * `Images/` : Star Schema and initial trend charts.
* **Milestone 2/**
    * `Screenshots/` : Final dashboard views (Customer Report, O&R Metrics).
    * `Updated Revenue analysis.pbix` : The complete Power BI report.
* **Milestone 3/**
    * `Screenshot/` : Latest report views including forecasting trends.
        * `F & C Report (1).png` : Full Forecasting & Cancellation dashboard.
        * `F & C Report 2021 Forecast.png` : Filtered view showing 2021 demand predictions.
    * `Updated Revenue analysis.pbix` : Finalized Power BI file with integrated Prophet forecast data and DAX measures.
* **LICENSE** : Project licensing information.
* **README.md** : Full project documentation and technical implementation details.
* 
```dax
Total Forecasted Bookings = 
VAR OverallLastDate = CALCULATE( MAX('Date'[Date]), ALL('Date') )
RETURN
CALCULATE(
    SUM( Forecast[yhat] ),
    Forecast[ds] > OverallLastDate
)

Avg Cancellation Rate = 
AVERAGEX( 
    VALUES( 'Date'[Month] ), 
    CALCULATE( SUM( Customer[Cancelled Bookings] ) )
)
