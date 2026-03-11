## **E-Commerce Sales & Customer Analytics Portfolio Project**
### **Project Overview**
This project is an end-to-end Data Analytics solution designed to extract actionable business    
insights from a transnational retail dataset. It covers the entire data lifecycle: from raw data  
ingestion and ETL processing in Power Query, to relational data modeling (Star Schema), and  
finally, dynamic visualization in Power BI.  


### 1. Data Import & Infrastructure  
The dataset used for this project is the Tata Online Retail dataset, containing transnational    
transactions occurring between 2010 and 2011.  
* **Source:** Kaggle (Tata Online Retail Dataset) - https://www.kaggle.com/datasets/ishanshrivastava28/tata-online-retail-dataset
* **Size:** ~500,000+ rows, 8 columns
* **Infrastructure:** Raw data (online_retail.csv) is version-controlled using Git LFS due  
  to its large size (>50MB). The data was imported directly into Power BI via Power Query for  
  the ETL process.

### 2. Data Profiling & Cleansing
Data quality assessment and transformation were performed using Power Query.

**Data Profiling:** Evaluated using Power Query's `Column Quality`, `Column Distribution`, and  
`Column Profile` features based on the entire dataset.

**Data Dictionary & Transformation Logic:**  
The dataset was rigorously cleaned to ensure accuracy for revenue and customer-level analytics:

| Column Name | Data Type | Description & Cleaning Logic |
|---|---|---|
| `InvoiceNo` | Text | Unique transaction identifier, contains no null values. Since the focus is on revenue analysis, transactions containing "C" (cancellations/returns) were explicitly excluded. |
| `StockCode` | Text | Product identifier. Verified to contain no errors or null values. |
| `Description` | Text | Product name. Less than 1% of records were empty. Since these also lacked a CustomerID and had a UnitPrice = 0 (providing no value for revenue analysis), they were removed. |
| `Quantity` | Whole number | Item quantity. Filtered to include only values > 0. |
| `InvoiceDate` | Date | Date and time of the transaction. Contains no errors or null values. Extracted temporal dimensions (Year, MonthNumber, MonthName, Quarter) to support time-series revenue analysis (for the `dim_date` table). |
| `UnitPrice` | Currency | Price per unit. Filtered to include only values > 0. |
| `CustomerID` | Whole number | Customer identifier. Approximately 25% of records contained null values; these were removed to enable accurate customer-level analytics. |




### 3. Data Modeling & Transformation  

The normalized tables were structured into a **Star Schema** to optimize filtering and DAX  
measure calculations.  

* **Fact Table:** `fact_transactions` (contains quantitative data: sales, quantities, dates, and  
  foreign keys).  
* **Dimension Tables:** `dim_customers`, `dim_products`, `dim_date`.  

**Technical Challenge Solved:**  
During the creation of the 1-to-Many relationship between `dim_products` and  
`fact_transactions` via `StockCode`, Power Pivot detected duplicates in the dimension table  
despite Power Query showing unique values. This occurred because **Power Query is case-sensitive,  
whereas Power Pivot is not.**  

* *Solution:* Applied `TRIM` and `UPPERCASE` functions in Power Query to key columns  
  (`StockCode`, `CustomerID`, `InvoiceDate`) to standardize formatting, remove hidden  
  spaces, and successfully build the relationships.

[See Data Model Architecture below](images/star_schema.png)


### 4. Dashboard Construction
The analytical dashboard was built in Power BI, utilizing advanced DAX measures (Total Revenue,  
Average Order Value, MoM Growth, etc.). It consists of three core pages to answer specific business questions:

* **Page 1: Executive Overview**

  *Answers: "How is the business performing overall?"*

  Features high-level KPIs, revenue trends over time, top 10 products, and top-performing  
  geographic regions.

* **Page 2: Customer Analytics**

  *Answers: "Who are our customers and how should we engage them?"*

  Highlights Customer Segmentation, Pareto Analysis (identifying the top 20% of  
  customers), and Revenue per Segment.

* **Page 3: Growth & Operations**

  *Answers: "When do people buy and what drives profitability?"*
  
  Focuses on Month-over-Month (MoM) Growth %, seasonal peaks, and revenue  
  concentration risk.


### 5. Key Business Insights & Recommendations
Transforming data into actionable business strategies:
1. **Revenue Concentration Risk:**
    * *Insight:* The analysis revealed that a significant portion of total revenue is generated  
      by a very small segment of top customers.
    * *Recommendation:* While nurturing these 'Champions' is critical, the business must  
      mitigate risk by running targeted campaigns to convert 'Potential Loyalists' and recent  
      buyers into frequent shoppers to diversify the revenue stream.

2. **Seasonality & Inventory Operations:**
    * *Insight:* Sales exhibit strong seasonality, with massive spikes in transaction volume  
      during specific months.
    * *Recommendation:* Optimize inventory levels for top-performing  `dim_products` ahead of these  
       historical peaks and allocate marketing budgets to capitalize on high-traffic periods.



### ⏳ Next Steps & Work in Progress

This project is actively being developed. To provide a deeper level of analytical rigor and a  
better user experience, the following deliverables are currently in progress and will be    
published to this repository soon:

* **Interactive Demonstration (** `/images` **):** A short GIF animation and a 2-minute video  
    presentation  walking through the dynamic cross-filtering, drill-down capabilities, and  
    actionable insights of the Power BI dashboard.
* **Advanced Analytical Reports (** `/reports` **directory):**
  * `Data Quality Report.pdf`: A comprehensive audit of the dataset's hygiene,  
     structural integrity, and applied ETL rules.
  * `EDA.pdf` (Exploratory Data Analysis): Statistical summaries, univariate/bivariate  
     analysis, and data distributions.
  * `RFM.pdf`: Detailed methodology and mathematical scoring behind the Recency,  
     Frequency, and Monetary customer segmentation.
  * `Customers.pdf`: A deep dive into customer portfolio analysis, geographic distribution,  
     and lifetime value concepts.

      
