# 🚖 NammaYatri Trips Dashboard | SQL + Power BI 

## 🚀 Project Description

The **NammaYatri Trips Dashboard** is a data analytics and visualization project based on trip data from the NammaYatri app, which is primarily used in **Bengaluru, India**. Using **SQL** for querying and **Power BI** for dashboarding, this project explores key metrics like completed trips, earnings, driver performance, and conversion rates. The dashboard helps uncover trends in user behavior, cancellations, and area-wise activity across the city. It demonstrates how real-world urban mobility data can be transformed into meaningful insights to support smarter transportation decisions.

---

## 📌 Project Highlights

- 🛠️ Used **MySQL** for writing analytical queries
- 📊 Built an interactive dashboard in **Power BI**
- 🎯 Tracked key performance indicators like **trips**, **earnings**, **conversion rate**, **cancellations**, and **location-based analysis**


## Dashboard Preview
<img width="1326" height="741" alt="Screenshot 2025-07-31 215448" src="https://github.com/user-attachments/assets/871bdc91-40a7-42c4-9291-deb69df1452d" />

---

### ✅ **KPIs Displayed**

- Total Trips  
- Completed Rides  
- Total Earnings  
- Total Searches  
- Driver & Customer Cancellations  
- OTP Confirmations  
- Conversion Rate  
- Average Fare & Distance  
---
### 📊 **Visual Analytics**

- **Line Charts**: Trips vs Duration, Fare vs Duration, Distance Trends  
- **Geo Map**: Location-wise Trip Density across Bengaluru  
- **Tables**: Fare Distribution, Search Funnel Metrics  
- **Gauge**: Conversion Rate  
- **Filters**: Duration, Assembly (region)

---

## 🛠️ Tech Stack
🔷 SQL (MySQL/PostgreSQL):

= Cleaned and prepared raw ride data using CTEs, JOINS, aggregation, and date-time functions.

- Generated KPIs like ride counts, conversion rates, and driver earnings.

```sql
-- Total Trips
SELECT COUNT(DISTINCT tripid) FROM trips_details;

-- Total Drivers
SELECT COUNT(DISTINCT driverid) FROM trips;

-- Total Earnings
SELECT SUM(fare) FROM trips;

-- Completed Trips
SELECT SUM(end_ride) FROM trips_details;

-- Total Searches
SELECT SUM(searches) FROM trips_details;

-- Total Searches that got Estimates
SELECT SUM(searches_got_estimate) FROM trips_details;

-- Total Searches for Quotes
SELECT SUM(searches_for_quotes) FROM trips_details;

-- Total Searches that got Quotes
SELECT SUM(searches_got_quotes) FROM trips_details;

-- Total Driver Cancellations
SELECT COUNT(*) - SUM(driver_not_cancelled) FROM trips_details;

-- Total OTP Entered
SELECT SUM(otp_entered) FROM trips_details;

-- Total End Ride
SELECT SUM(end_ride) FROM trips_details;

-- Average Distance per Trip
SELECT AVG(distance) FROM trips;

-- Average Fare per Trip
SELECT AVG(fare) FROM trips;
-- Or
SELECT SUM(fare)/COUNT(*) FROM trips;

-- Most Used Payment Method
SELECT a.method 
FROM payment a 
JOIN (
  SELECT faremethod, COUNT(DISTINCT tripid) cnt 
  FROM trips 
  GROUP BY faremethod 
  ORDER BY cnt DESC 
  LIMIT 1
) b 
ON a.id = b.faremethod;

-- Highest Payment Method
SELECT a.method 
FROM payment a 
JOIN (
  SELECT * 
  FROM trips 
  ORDER BY fare DESC 
  LIMIT 1
) b 
ON a.id = b.faremethod;

-- Top 2 Locations with Most Trips
SELECT loc_from, loc_to, COUNT(DISTINCT tripid) trip 
FROM trips 
GROUP BY loc_from, loc_to 
ORDER BY trip DESC 
LIMIT 1;

-- Area with Highest Earnings
SELECT loc_from, SUM(fare) AS fare 
FROM trips 
GROUP BY loc_from 
ORDER BY fare DESC 
LIMIT 1;

-- Area with Most Driver Cancellations
SELECT loc_from, COUNT(*) - SUM(driver_not_cancelled) AS can 
FROM trips_details 
GROUP BY loc_from 
ORDER BY can DESC 
LIMIT 1;

-- Area with Most Customer Cancellations
SELECT loc_from, COUNT(*) - SUM(customer_not_cancelled) AS can 
FROM trips_details 
GROUP BY loc_from 
ORDER BY can DESC 
LIMIT 1;

-- Area with Most Trips by Duration
SELECT * 
FROM (
  SELECT duration, loc_from, COUNT(DISTINCT tripid) AS cnt, 
         RANK() OVER (PARTITION BY duration ORDER BY COUNT(DISTINCT tripid) DESC) rnk 
  FROM trips 
  GROUP BY duration, loc_from
) a 
WHERE rnk = 1;

-- Top 5 Earning Drivers
SELECT driverid, SUM(fare) AS fare 
FROM trips 
GROUP BY driverid 
ORDER BY fare DESC 
LIMIT 5;

-- Top 5 with Ties (Using RANK)
SELECT * 
FROM (
  SELECT driverid, SUM(fare) AS fare, 
         DENSE_RANK() OVER (ORDER BY SUM(fare) DESC) AS rnk 
  FROM trips 
  GROUP BY driverid
) a 
WHERE rnk <= 5;

-- Duration with Most Trips
SELECT duration, COUNT(DISTINCT tripid) AS cnt 
FROM trips 
GROUP BY duration 
ORDER BY cnt DESC 
LIMIT 1;

-- Duration with Highest Fare
SELECT duration, SUM(fare) AS fare 
FROM trips 
GROUP BY duration 
ORDER BY fare DESC 
LIMIT 1;

-- Driver-Customer Pair with Most Orders
SELECT driverid, custid, COUNT(DISTINCT tripid) AS cnt 
FROM trips 
GROUP BY driverid, custid 
ORDER BY cnt DESC 
LIMIT 1;

-- Search to Estimate Rate
SELECT SUM(searches_got_estimate) * 100.0 / SUM(searches) FROM trips_details;

-- Estimate to Quote Rate
SELECT SUM(searches_for_quotes) * 100.0 / SUM(searches) FROM trips_details;

```

🔷 Power BI:
- Built on Power BI Desktop

- DAX used for calculating derived KPIs

- Custom slicers and drill-down features

- Visual consistency for professional insights

- Color-coded design reflecting the app’s real-world tone

---

## ✅ Use Cases
- Urban mobility analysis for smart transportation planning

- Business insights for ride-hailing operators

- Monitoring user behavior and cancellation trends

- Location-based driver allocation and pricing models

- Internal KPIs for company decision-making

---

## 🔧 Future Enhancements
- Add real-time refresh with cloud database integration

- Deploy dashboard via Power BI Service with scheduled updates

- Include forecasting for trip volumes and fare trends

- Incorporate ML models to predict cancellations or fare anomalies

