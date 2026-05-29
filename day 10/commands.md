# Day 10 Commands — Text-to-SQL AI

## Install Required Libraries
```powershell
python -m pip install groq clickhouse-driver python-dotenv
```

## Create .env File
```powershell
New-Item -ItemType File -Path D:\nyc_taxi\.env
```

## .env File Content
GROQ_API_KEY=your_api_key_here

## Verify .gitignore has .env
Open .gitignore
Make sure .env is listed!
Never push API key to GitHub!

## Create text_to_sql.py
```powershell
New-Item -ItemType File -Path D:\nyc_taxi\text_to_sql.py
```

## Run Text-to-SQL Assistant
```powershell
python text_to_sql.py
```

## Test Questions
What is the average fare amount?
How many trips were taken in total?
What is total revenue?
What is average trip duration?
Show trips by payment type

## Fix Model Name (if needed)
```python
# Change in text_to_sql.py:
# OLD (decommissioned):
model="llama3-8b-8192"

# NEW (working):
model="llama-3.3-70b-versatile"
```

## Example Results
Question: What is the average fare amount?
Generated SQL:
SELECT AVG(fare_amount) AS average_fare_amount
FROM raw.fct_trips;
Result: 13.052289394939532 ✅

## Key Learnings
- Groq = free AI API (166+ questions/day!)
- llama-3.3-70b-versatile = current model
- SCHEMA = tells AI about our tables
- generate_sql() = converts English to SQL
- run_query() = executes SQL in ClickHouse
- .env = stores API keys safely
- Never put API keys in GitHub!
- Text-to-SQL = business users get data instantly!
