# 🏬 End-to-End Retail Sales Performance Analytics

## 📌 Project Objective
This advanced data analytics project evaluates transaction behavior across multi-regional brick-and-mortar stores to isolate revenue trends, optimize regional retail inventory, and identify customer payment preferences to increase store profit margins.

---

## 🛠️ Technical Stack & Tools Used
* **Data Manipulation & Processing:** Python 3, Pandas, NumPy
* **Visualization & Reporting:** Matplotlib, Seaborn
* **Database Querying Language:** SQL (PostgreSQL compatible)
* **Development Environment:** Google Colab

---

## 🧹 Step-by-Step Data Cleaning Pipeline
To ensure high data quality before analysis, the following cleaning pipeline was executed in Python:
1. **Imputed Missing Values:** Reconstructed missing unit prices automatically using mathematical aggregates (`TotalAmount / Quantity`).
2. **Outlier Filtering:** Verified transaction amounts against strict retail price bounds to eliminate data entry noise.
3. **Data Type Casting:** Normalized `TransactionID` and `CustomerID` values into standardized string formats for seamless relational database joins.

---

## 🗄️ Advanced SQL Analytics Showcase
To demonstrate database querying readiness, the following SQL scripts were developed to extract deep business insights from the cleaned transactional schema:

### 1. Cumulative Revenue & 3-Day Rolling Average
This query calculates the daily revenue running totals alongside a rolling average to smooth out weekly sales spikes.
```sql
SELECT 
    StoreLocation,
    TransactionDate,
    SUM(TotalAmount) AS Daily_Sales,
    SUM(SUM(TotalAmount)) OVER (PARTITION BY StoreLocation ORDER BY TransactionDate) AS Cumulative_Revenue,
    AVG(SUM(TotalAmount)) OVER (PARTITION BY StoreLocation ORDER BY TransactionDate ROWS BETWEEN 2 PRECEDING AND CURRENT ROW) AS Rolling_Avg_3Day
FROM retail_sales
GROUP BY StoreLocation, TransactionDate
ORDER BY StoreLocation, TransactionDate;
```

### 2. High-Value Customer Segmentation (RFM)
This query flags top-tier "VIP" customers based on how recently they shopped and how much money they spent.
```sql
SELECT 
    CustomerID,
    COUNT(TransactionID) AS Total_Visits,
    SUM(TotalAmount) AS Total_Spent,
    CASE 
        WHEN SUM(TotalAmount) >= 200 THEN 'VIP High-Spender'
        WHEN SUM(TotalAmount) BETWEEN 100 AND 199 THEN 'Core Customer'
        ELSE 'Low-Value Casual Buyer'
    END AS Customer_Segment
FROM retail_sales
GROUP BY CustomerID
ORDER BY Total_Spent DESC;
```

---

## 📊 Business Insights & Visualizations

### 1. Revenue Trends by Store Location
Chicago and New York represent our primary revenue anchors. The data reveals that the western retail center (Los Angeles) requires active localized marketing deployments and inventory adjustments to catch up.

![Store Location Performance](location_sales.png)

### 2. Transaction Type Distribution Analysis
Credit Cards and UPI combine to command a majority share of regional checkout pipelines. Action Item: Partner with local UPI providers to launch cashback offers, which can lower credit card processing fees for the retail store.

![Payment Method Split](payment_methods.png)

---

## 🎯 Final Business Recommendations for Stakeholders
* **Inventory Realignment:** Increase high-margin Electronics inventory in New York and Chicago branches during peak weekdays based on sales volume trends.
* **Targeted Loyalty Campaigns:** Use the SQL customer segmentation results to send automated discount notifications via email directly to "Core Customers" to bump them into the "VIP" bracket.
