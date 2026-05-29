# Day 5 Commands — fct_trips Fact Table

## Create fct_trips.sql File
```powershell
New-Item -ItemType File -Path models\marts\fct_trips.sql
```

## fct_trips.sql Content
```sql
{{ config(materialized='table') }}

SELECT
    -- identifiers
    t.trip_id,

    -- timestamps
    t.pickup_datetime,
    t.dropoff_datetime,
    t.trip_duration_min,

    -- trip details
    t.passenger_count,
    t.trip_distance,

    -- financials
    t.fare_amount,
    t.tip_amount,
    t.total_amount,

    -- payment key (links to dim_payment!)
    p.payment_key,

    -- location keys (links to dim_location!)
    pl.location_key AS pickup_location_key,
    dl.location_key AS dropoff_location_key

FROM {{ ref('stg_trips') }} t

LEFT JOIN {{ ref('dim_payment') }} p
    ON t.payment_type = p.payment_type

LEFT JOIN {{ ref('dim_location') }} pl
    ON t.pickup_borough = pl.borough

LEFT JOIN {{ ref('dim_location') }} dl
    ON t.dropoff_borough = dl.borough

SETTINGS join_use_nulls = 1
```

## Run dbt
```powershell
dbt run
```

## Expected Result
6 of 6 OK created sql table model raw.fct_trips
PASS=6 ERROR=0 ✅

## Verify fct_trips Data
```powershell
docker exec -it clickhouse-nyc clickhouse-client --query "SELECT * FROM raw.fct_trips LIMIT 5"
```

## Count fct_trips Rows
```powershell
docker exec -it clickhouse-nyc clickhouse-client --query "SELECT count() FROM raw.fct_trips"
```

## Star Schema Query
```powershell
docker exec -it clickhouse-nyc clickhouse-client --query "
SELECT 
    p.payment_type,
    p.payment_description,
    count() AS total_trips,
    round(avg(f.fare_amount), 2) AS avg_fare,
    round(sum(f.total_amount), 2) AS total_revenue
FROM raw.fct_trips f
JOIN raw.dim_payment p
    ON f.payment_key = p.payment_key
GROUP BY 
    p.payment_type,
    p.payment_description
ORDER BY total_trips DESC
"
```

## Key Learnings
- fct_trips = center of star schema!
- LEFT JOIN = keep all rows even if no match
- INNER JOIN = only keep matching rows
- Table alias = short nickname (t, p, pl, dl)
- materialized='table' = store as real table not view!
- SETTINGS join_use_nulls = 1 for ClickHouse JOINs
- Star schema complete with fact + dimension tables!
