# Day 6 Insights — dbt Tests

## 🎯 What are dbt Tests?
dbt tests are automatic data quality checks
that verify your data is correct
before it reaches the dashboard!

Like a car factory quality check:
→ Check engine works ✅
→ Check brakes work ✅
→ Check lights work ✅
→ If anything fails → fix before delivery!

dbt tests = same thing for data! ✅

## 🧪 4 Types of Tests

### 1. unique
Checks no duplicate values exist!
trip_id must be unique!
No two trips same ID!
If duplicate found → TEST FAILS! ❌

### 2. not_null
Checks no empty values exist!
fare_amount must have a value!
Cannot be empty/missing!
If null found → TEST FAILS! ❌

### 3. accepted_values
Checks only valid values exist!
payment_type can ONLY be:
CSH, CRE, NOC, DIS
If "XYZ" appears → TEST FAILS! ❌

### 4. relationships
Checks keys match between tables!
Every payment_key in fct_trips
must exist in dim_payment!
If orphan key found → TEST FAILS! ❌

## 📊 Our Test Results
stg_trips tests:
✅ trip_id unique
✅ trip_id not_null
✅ fare_amount not_null
✅ payment_type not_null
✅ payment_type accepted_values
✅ passenger_count not_null
✅ trip_distance not_null
dim_payment tests:
✅ payment_key unique
✅ payment_key not_null
✅ payment_type not_null
dim_location tests:
✅ location_key unique
✅ location_key not_null
✅ borough not_null
fct_trips tests:
✅ trip_id unique
✅ trip_id not_null
✅ fare_amount not_null
✅ payment_key not_null
✅ payment_key relationships
Total: 18/18 PASSING! 🏆

## 🚨 Problems Fixed

### Problem 1: Example models failing
Error: not_null_my_first_dbt_model failing
Fix: Delete models\example folder
Lesson: Clean up example files always!

### Problem 2: Relationships test error
Error: join_use_nulls setting missing
Fix: Add SETTINGS join_use_nulls = 1
to fct_trips.sql
Lesson: ClickHouse needs this setting
for JOIN with NULLs!

### Problem 3: Deprecation warnings
Warning: accepted_values needs arguments
Fix: Add arguments: property
before values: list
Lesson: Always fix warnings!
Clean code = professional code!

## 💡 Why Tests Matter

### Without tests:
Bad data reaches dashboard!
Wrong numbers shown!
Wrong business decisions!
Company loses money! 😰

### With tests:
Bad data caught immediately!
Fixed before dashboard!
Correct business decisions!
Business grows! ✅

## 🎓 Real World Lesson
Golden Rule:
─────────────
Test early!
Test everything!
Fix immediately!
Rule
→ Write tests FIRST
→ Then write code
→ Called TDD
(Test Driven Development)
Our score: 18/18 = 100%! 🏆
Data is clean and reliable! ✅
