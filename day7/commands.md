# Day 7 Commands — dbt Docs + DAG

## Generate dbt Documentation
```powershell
dbt docs generate
```

## Serve dbt Documentation Website
```powershell
dbt docs serve
```

## Open in Browser
http://localhost:8080

## View DAG Diagram

Open http://localhost:8080
Click blue button
bottom right corner
DAG diagram appears!
Click any model
to see details!


## Stop docs server
Press Ctrl + C
in terminal to stop!

## Key Commands Summary
```powershell
# Full workflow
cd D:\nyc_taxi
dbt docs generate
dbt docs serve
```

## What DAG Shows
🟢 raw.trips (source)
↓
🔵 stg_trips
↓
┌──┴──────────┐
↓             ↓
🔵 dim_payment  🔵 dim_location
↓             ↓
└──────┬──────┘
↓
🔵 fct_trips

## Key Learnings
- dbt docs generate = creates documentation
- dbt docs serve = opens website locally
- DAG = Directed Acyclic Graph
- Green node = source (raw data)
- Blue node = dbt model
- Arrows = dependencies
- Click node = see details!
