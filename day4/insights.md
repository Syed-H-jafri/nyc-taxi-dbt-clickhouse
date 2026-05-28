# Day 4 Insights — Dimension Tables

## 🎯 What is a Star Schema?
Star schema is a database design where:
→ Fact table sits in CENTER
→ Dimension tables surround it like STAR points
     dim_payment
          ⭐
          |
dim_location ⭐ ←→ fct_trips
|
⭐

## 💡 What are Dimension Tables?

### dim_payment (4 rows)
Answers: WHO paid and HOW?
Contains: payment types with descriptions
CSH = Cash payment
CRE = Credit card payment
NOC = No charge
DIS = Dispute

### dim_location (5 rows)
Answers: WHERE did trips go?
Contains: NYC boroughs with descriptions
Manhattan, Brooklyn, Queens,
Bronx, Staten Island

## 🔑 Key Concepts Learned

### CASE Statement
Like IF ELSE in programming!
CASE payment_type
WHEN 'CSH' THEN 'Cash payment'
WHEN 'CRE' THEN 'Credit card payment'
ELSE 'Unknown'
END
Converts codes to readable descriptions!
CSH → Cash payment ✅

### DISTINCT
Removes duplicate values!
Without DISTINCT:
CSH (trip 1)
CSH (trip 2)
CSH (trip 3)
= 3 rows!
With DISTINCT:
CSH = 1 row only! ✅

### UNION DISTINCT
Combines TWO SELECT results
into ONE list!
Removes duplicates automatically!
pickup boroughs + dropoff boroughs
= complete list of all boroughs!

### row_number() OVER ()
Generates unique number
for each row automatically!
1, 2, 3, 4, 5...
Used to create:
payment_key
location_key

### ref() vs source()
source() = points to RAW table
{{ source('raw', 'trips') }}
ref()    = points to dbt MODEL
{{ ref('stg_trips') }}
Rule:
Raw data   → source()
dbt models → ref()

## 🚨 Problems Faced + Solutions

### Problem 1: UNION Error
Error: Expected ALL or DISTINCT
Fix: Change UNION to UNION DISTINCT
Lesson: ClickHouse requires
UNION ALL or UNION DISTINCT!
Plain UNION not allowed!

### Problem 2: Empty Borough Data
Problem: pickup_borough was empty
in most trips!
Fix: hardcoded 5 NYC boroughs directly
Lesson: Real world data is always messy!
Sometimes source data is incomplete!
Document limitations and move forward!

## 💰 Why Dimension Tables Matter

### Without Dimensions:
fct_trips stores:
"Cash", "Cash", "Cash"
600,000 times!
= Waste of storage! 😰
= Inconsistent data!

### With Dimensions:
fct_trips stores:
1, 1, 1
600,000 times!
= Much less storage! ✅
= Always consistent!
= Update once in dim table
reflects everywhere!

## 🎓 Real World Lesson
Dimension tables are SMALL
but VERY important!
dim_payment = only 4 rows
dim_location = only 5 rows
But they give CONTEXT
to millions of fact rows!
Without context:
payment_type = CSH
→ what does CSH mean?
With dim_payment:
CSH = Cash payment ✅
→ everyone understands!
