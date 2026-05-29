## 🎯 What is a Fact Table?
Fact table is the CENTER of star schema!
It stores WHAT HAPPENED:
→ Every taxi trip
→ All numeric measures
→ Keys connecting to dimensions

Like a receipt for every trip!
trip happened + how much + how long + how far!

## ⭐ Star Schema Complete!
     dim_payment
          ⭐
          |
dim_location ⭐ ←→ fct_trips ←→ ⭐
|    (CENTER!)
⭐

## 🔑 Key Concepts Learned

### LEFT JOIN
Keeps ALL rows from left table!
Even if no match in right table!
No data lost! ✅
vs INNER JOIN:
Only keeps MATCHING rows!
Data can be lost! ❌
We use LEFT JOIN because:
→ Some trips have empty borough
→ Don't want to lose those trips!

### Table Aliases
stg_trips → t
dim_payment → p
dim_location (pickup) → pl
dim_location (dropoff) → dl
Why?
→ Shorter to write!
→ t.trip_id instead of stg_trips.trip_id
→ Cleaner code! ✅

### materialized='table'
{{ config(materialized='table') }}
Tells dbt to store as REAL TABLE
not just a VIEW!
Why important?
→ VIEW runs SQL every query (slow!)
→ TABLE stores data physically (fast!)
→ Power BI needs fast queries!
→ Essential for dashboard! ✅

### join_use_nulls = 1
SETTINGS join_use_nulls = 1
ClickHouse specific setting!
Allows NULL values in JOINs!
Without it → ERROR! ❌
With it → Works perfectly! ✅

### Two Joins Same Table
Same dim_location used TWICE!
Once for pickup borough
Once for dropoff borough
Different aliases:
pl = pickup location
dl = dropoff location
Smart reuse of same table! ✅

## 📊 Real Business Results

### Star Schema Query Results:
Cash payment    → 615,250 trips → $10,899,081 revenue
Credit card     → 374,840 trips →  $5,113,625 revenue
No charge       →   2,618 trips →     $43,962 revenue
Dispute         →   1,000 trips →     $15,447 revenue
Total: 993,708 trips → $16,072,116 revenue! 💰

## 💡 Business Insights Discovered
💰 Total revenue = $16 MILLION!
🚕 Cash = 62% of all trips!
💳 Credit = 38% of all trips!
🆓 No charge = rare but exists!
⚠️ Disputes = very small percentage!

## 🎓 Real World Lesson
fct_trips is the most important table!
Everything connects through it!
Dashboard queries use it!
AI queries use it!
Business decisions come from it!
Without clean fact table:
→ wrong dashboard numbers
→ wrong business decisions
→ company loses money!
With clean fact table:
→ accurate insights
→ correct decisions
→ business grows! ✅
