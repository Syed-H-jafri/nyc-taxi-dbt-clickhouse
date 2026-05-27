Day 3 Insights ONLY! 📝

Create day3/insights.md
markdown# Day 3 Insights — Staging Model

## 🎯 What is a Staging Model?
Staging is the first transformation layer
in dbt that sits between raw data
and final tables.

Like a kitchen prep area:
→ Raw vegetables come in dirty
→ Chef washes and cuts them
→ Ready for cooking!

stg_trips = cleaned version of raw.trips!

## 💡 What stg_trips Does

### 1. Calculates New Column
dateDiff('minute', pickup, dropoff)
→ calculates trip duration in minutes!
→ raw data had pickup and dropoff times
→ but NOT duration!
→ we calculated it once here
→ every other table uses it!

### 2. Renames Columns
pickup_location  → pickup_borough
dropoff_location → dropoff_borough
Borough is clearer than location!
Manhattan, Brooklyn = boroughs!

### 3. Filters Bad Data
fare_amount > 0      → no free fares
passenger_count > 0  → no empty rides
trip_distance > 0    → no zero distance
Golden Rule:
Garbage in = Garbage out!
Staging prevents garbage
reaching the dashboard!

## 🔑 Key Concepts Learned

### dbt Model
What: just a SQL file!
Where: models/ folder
How: dbt reads and runs it automatically
Result: creates VIEW or TABLE in ClickHouse

### VIEW vs TABLE
VIEW:
→ No data stored
→ Just saved SQL query
→ Runs fresh every time
→ Lightweight!
→ Perfect for staging!
TABLE:
→ Data physically stored
→ Faster to query
→ Takes disk space
→ Perfect for fact tables!

### source() vs ref()
source() = points to RAW data
{{ source('raw', 'trips') }}
→ used in stg_trips.sql
→ points to raw.trips table
ref() = points to dbt MODEL
{{ ref('stg_trips') }}
→ used in dim/fct tables
→ points to stg_trips model
Rule:
Raw data   → use source()
dbt models → use ref()

### sources.yml
What: registers raw data in dbt
Why: dbt tracks where data comes from
Result: shows in DAG diagram!
enables source testing!
documents raw tables!

### dateDiff Function
dateDiff('minute', start, end)
→ calculates difference in minutes
Example:
pickup:  10:00 AM
dropoff: 10:25 AM
result:  25 minutes!

## 🚨 Real World Lesson
stg_trips is our quality gate!
Only clean valid data passes through!
Without staging:
→ wrong averages in dashboard
→ wrong business decisions
→ company loses money!
With staging:
→ only valid trips analyzed
→ correct business decisions!

## ⚡ Feature Engineering
Feature Engineering =
selecting, calculating and
preparing the right columns
for analysis!
We did feature engineering:
→ selected 12 useful columns
→ calculated trip_duration_min
→ renamed location to borough
→ filtered invalid trips

## 🎓 What Senior Engineers Know
dbt model = SQL file (nothing fancy!)
Staging = always first layer
source() = for raw tables
ref() = for dbt models
VIEW = lightweight, good for staging
Always filter bad data early!

## 🚀 Tomorrow — Day 4
- Create models/marts/ folder
- Build dim_payment table
- Build dim_location table
- Learn about star schema!
