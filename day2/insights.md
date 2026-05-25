# Day 2 Insights — dbt Setup + Connection

## 🎯 What is dbt?
dbt (data build tool) is a Python library
that uses SQL for automation of data
transformation — making it easier,
faster and more professional!

## 💡 dbt 4 Superpowers

### 1. Transform Data 🔄
Takes raw messy data and cleans it
using SQL models automatically!

### 2. Test Data Quality ✅
Checks data is correct before
it reaches the dashboard!

### 3. Manage Build Order 📋
Figures out correct sequence
to build all tables automatically!

### 4. Document Everything 📚
Auto generates beautiful website
showing all models and columns!

## 🔑 Key Concepts Learned

### profiles.yml
What: connection settings file
Where: C:\Users\Owner.dbt\ (global!)
Why: tells dbt how to connect to ClickHouse

### dbt Project Structure
What: folder with all SQL models
Where: D:\nyc_taxi (local!)
Why: contains all transformation code

### Schema
What: folder inside a database
In ClickHouse: database = schema (same!)
Our case: database raw = schema raw

### Dependencies
What: correct build order
Example: stg_trips needs raw.trips first!
dbt figures this out automatically!

### Staging Concept
stg = staging = data cleaning layer
Like washing vegetables before cooking!
Raw data in → Clean data out!

## 🚨 Problems Faced + Solutions

### Problem 1: pip not recognized
Error: pip not recognized
Fix: use python -m pip instead!

### Problem 2: dbt not recognized
Error: dbt not recognized
Fix: add Scripts folder to Windows PATH

### Problem 3: Authentication failed
Error: ClickHouse password incorrect
Fix: recreate container with no password
