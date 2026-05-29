# Day 9 — Power BI Dashboard Complete

## Commands Used

### Add Average Duration Card

Click Card visual
Check trip_duration_min
Change to Average
Result: 15.23 minutes!


### Add Pie Chart

Click Pie Chart visual
Legend: dim_payment → payment_description
Values: fct_trips → total_amount → Sum
Result: Revenue split by payment!


### Add Bar Chart (Avg Fare)

Click Clustered Bar Chart
Y-Axis: dim_payment → payment_description
X-Axis: fct_trips → fare_amount → Average
Turn on Data Labels
Result: Avg fare by payment type!


### Add Payment Summary Table

Click Table visual
Add columns:
→ dim_payment: payment_type
→ fct_trips: trip_id (Count)
→ fct_trips: fare_amount (Average)
→ fct_trips: total_amount (Sum)
→ fct_trips: tip_amount (Average)
Result: Complete payment summary!


### Add Hour Column (DAX)

Go to Table View
Click fct_trips
Click New Column
Type formula:
Hour = HOUR(fct_trips[pickup_datetime])
Press Enter!


### Add Line Chart (Trips by Hour)

Click Line Chart visual
X-Axis: fct_trips → Hour
Y-Axis: fct_trips → trip_id → Count
Result: Shows busiest hours!


### Apply Dark Theme

Click View → Themes
Select dark blue theme
Result: Professional looking! 😍


### Add Titles
Insert → Text box
Page 1: "NYC Taxi Analytics 2015"
Page 2: "Payment Analysis"
Page 3: "Trip Analysis"

### Rename Pages
Right click page tab
Click Rename
Page 1 → Executive Summary
Page 2 → Payment Analysis
Page 3 → Trip Analysis

### Save Dashboard
Ctrl + S
Name: NYC_Taxi_Dashboard.pbix

---

## 📊 Dashboard Structure

### Page 1: Executive Summary
✅ Title: NYC Taxi Analytics 2015
✅ Card: 993.708K Total Trips
✅ Card: 16.07M Total Revenue
✅ Card: 12.97M Total Fare
✅ Card: 15.23 Avg Duration
✅ Bar Chart: Trips by Payment
✅ Pie Chart: Revenue Split

### Page 2: Payment Analysis
✅ Title: Payment Analysis
✅ Bar Chart: Avg Fare by Payment
✅ Table: Complete Payment Summary

### Page 3: Trip Analysis
✅ Title: Trip Analysis
✅ Card: 3.80 Avg Distance (miles)
✅ Card: 13.05 Avg Fare
✅ Line Chart: Trips by Hour

---

## 💡 Key Insights Discovered

### Payment Insights:
💵 Cash = 615,250 trips (62%)
💳 Credit = 374,840 trips (38%)
💵 Cash revenue = $10,899,081 (68%)
💳 Credit revenue = $5,113,625 (32%)
💰 Total revenue = $16,072,116!

### Tip Insights:
💳 Credit card avg tip = $2.71
(card machine prompts automatically!)
💵 Cash tips = not recorded in system
(given directly to driver!)

### Trip Pattern Insights:
🌆 Busiest time = 9 PM (61K trips!)
😴 Quietest time = 5 AM (11K trips!)
📍 Average distance = 3.80 miles
⏱️ Average duration = 15.23 minutes
🚗 Average speed = ~15 mph NYC traffic

### Fare Insights:
💰 No charge = highest avg fare $15.03
(long trips that were comped!)
⚠️ Dispute = $14.03 avg fare
(higher fares = more disputes!)
💵 Cash = $13.51 avg fare
💳 Credit = $12.28 avg fare

---

## 🎓 Key Concepts Learned

### DAX Formula
DAX = Data Analysis Expressions
Power BI formula language!
Example:
Hour = HOUR(fct_trips[pickup_datetime])
→ extracts hour from datetime
→ 0-23 values
→ used for line chart!

### KPI Cards
KPI = Key Performance Indicator
Most important numbers!
Always shown at top!
First thing executives see!

### Data Labels
Shows values on chart bars!
Makes chart readable!
Turn on in Format Visual
→ Data labels → ON

### Multiple Pages
Professional dashboards
use multiple pages!
Page 1 = Executive overview
Page 2 = Detailed analysis
Page 3 = Specific topic
Each page tells ONE story! ✅

## 🚨 Problems Fixed

### Problem 1: Tips showing wrong
Cash showing 2.71 instead of Credit!
Fix: Refresh relationships in Power BI
Lesson: Always verify data with SQL first!
docker exec clickhouse-client query
to confirm actual values!

### Problem 2: No space for visuals
Canvas getting full!
Fix: Use multiple pages!
Lesson: Professional approach =
separate pages for topics!

## 🎓 Real World Lesson
Power BI is the EASIEST part!
All the hard work was:
→ Docker setup ✅
→ dbt pipeline ✅
→ Star schema ✅
→ Data cleaning ✅
Power BI just reads clean data
and makes it beautiful!
Hard work upfront = easy dashboard! ✅
