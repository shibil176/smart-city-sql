# 🌆 Smart City Resource Management System (SQL Project)

## 📌 Overview  
This project is a **unique SQL portfolio project** designed to showcase advanced database design and query skills. Unlike common SQL projects (like e-commerce or library systems), this project models a **Smart City** and manages its:  

- 🏠 Households & Residents  
- ⚡ Energy Consumption  
- 💧 Water Usage  
- 🚗 Traffic Monitoring  
- 🗑 Waste Collection  

The database supports **analytics, optimization, and alerting** to help city administrators make **data-driven decisions**.  

---

## 🎯 Features
✔ **Normalized Database Design** with multiple entities (residents, households, sensors, etc.)  
✔ **Advanced SQL Queries** (joins, aggregates, CTEs, window functions)  
✔ **Automated Alerts with Triggers** (detect high energy usage)  
✔ **Stored Procedures** for reusable reports  
✔ **Views** for real-time dashboards  
✔ **Sample Data** included for quick demo  

---

## 🏗 Database Schema  

### Tables
- `Residents` – basic citizen info  
- `Households` – housing units with zones  
- `Energy_Consumption` – electricity usage  
- `Water_Usage` – water consumption  
- `Traffic_Sensors` – vehicle monitoring  
- `Waste_Collection` – garbage pickup  

---

## 📂 Schema Diagram (ERD)

![Smart City ERD](smart_city_erd.png)

---

## 📊 Example Queries  

### 1️⃣ Rank Top Energy Consumers
```sql
SELECT h.address, SUM(e.kwh_used) AS total_energy,
       RANK() OVER (ORDER BY SUM(e.kwh_used) DESC) AS energy_rank
FROM Energy_Consumption e
JOIN Households h ON e.household_id = h.household_id
GROUP BY h.address;
```

### 2️⃣ Detect Water Wastage (>500L per day)
```sql
WITH WaterCTE AS (
    SELECT h.address, AVG(w.liters_used) AS avg_daily
    FROM Water_Usage w
    JOIN Households h ON w.household_id = h.household_id
    GROUP BY h.address
)
SELECT * FROM WaterCTE
WHERE avg_daily > 500;
```

### 3️⃣ Real-Time Dashboard (View)
```sql
CREATE VIEW SmartCityDashboard AS
SELECT h.zone,
       SUM(e.kwh_used) AS total_energy,
       SUM(w.liters_used) AS total_water,
       SUM(t.vehicle_count) AS total_traffic,
       SUM(wc.waste_collected_kg) AS total_waste
FROM Households h
LEFT JOIN Energy_Consumption e ON h.household_id = e.household_id
LEFT JOIN Water_Usage w ON h.household_id = w.household_id
LEFT JOIN Traffic_Sensors t ON h.zone = t.zone
LEFT JOIN Waste_Collection wc ON h.zone = wc.zone
GROUP BY h.zone;
```

---

## ⚡ Automation  

### 🚨 Trigger – High Energy Alert  
```sql
CREATE TRIGGER energy_alert_trigger
AFTER INSERT ON Energy_Consumption
FOR EACH ROW
BEGIN
   IF NEW.kwh_used > 50 THEN
      INSERT INTO EnergyAlerts (household_id, date, usage, message)
      VALUES (NEW.household_id, NEW.date, NEW.kwh_used, 'High energy usage detected');
   END IF;
END;
```

### 📑 Stored Procedure – Weekly Waste Report  
```sql
CREATE PROCEDURE WeeklyWasteReport()
BEGIN
   SELECT zone, SUM(waste_collected_kg) AS total_waste
   FROM Waste_Collection
   WHERE date >= CURDATE() - INTERVAL 7 DAY
   GROUP BY zone
   ORDER BY total_waste DESC;
END;
```

---

## 🚀 How to Run  

1. Clone this repository:  
   ```bash
   git clone https://github.com/your-username/smart-city-sql.git
   cd smart-city-sql
   ```

2. Create the database in MySQL (or PostgreSQL with slight changes):  
   ```sql
   CREATE DATABASE SmartCityDB;
   USE SmartCityDB;
   ```

3. Run the schema & data files:  
   ```sql
   SOURCE schema.sql;
   SOURCE data.sql;
   ```

4. Try the example queries, views, triggers, and stored procedures.

---

## 🏆 Why This Project Stands Out  
- 🔹 Not a generic CRUD database — it models **modern smart city challenges**  
- 🔹 Shows off **advanced SQL concepts** (not just SELECT/INSERT)  
- 🔹 Can be extended into **AI/analytics** (forecasting energy demand, traffic predictions, etc.)  
- 🔹 Looks professional on GitHub & LinkedIn  
