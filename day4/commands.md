# Day 4 Commands — Dimension Tables

## Create marts Folder
```powershell
New-Item -ItemType Directory -Path models\marts
```

## Create dim_payment.sql File
```powershell
New-Item -ItemType File -Path models\marts\dim_payment.sql
```

## dim_payment.sql Content
```sql
SELECT
    row_number() OVER ()        AS payment_key,
    payment_type,
    CASE payment_type
        WHEN 'CSH' THEN 'Cash payment'
        WHEN 'CRE' THEN 'Credit card payment'
        WHEN 'NOC' THEN 'No charge'
        WHEN 'DIS' THEN 'Dispute'
        ELSE 'Unknown payment type'
    END                         AS payment_description

FROM (
    SELECT DISTINCT payment_type
    FROM {{ ref('stg_trips') }}
    WHERE payment_type IS NOT NULL
)
```

## Create dim_location.sql File
```powershell
New-Item -ItemType File -Path models\marts\dim_location.sql
```

## dim_location.sql Content (Final Version)
```sql
SELECT
    row_number() OVER ()        AS location_key,
    borough,
    CASE borough
        WHEN 'Manhattan'     THEN 'Manhattan borough'
        WHEN 'Brooklyn'      THEN 'Brooklyn borough'
        WHEN 'Queens'        THEN 'Queens borough'
        WHEN 'Bronx'         THEN 'Bronx borough'
        WHEN 'Staten Island' THEN 'Staten Island borough'
        ELSE 'Unknown location'
    END                         AS location_description

FROM (
    SELECT 'Manhattan'     AS borough
    UNION DISTINCT
    SELECT 'Brooklyn'      AS borough
    UNION DISTINCT
    SELECT 'Queens'        AS borough
    UNION DISTINCT
    SELECT 'Bronx'         AS borough
    UNION DISTINCT
    SELECT 'Staten Island' AS borough
)
```

## Run dbt
```powershell
dbt run
```

## Expected Result
✅ dim_payment  → OK
✅ dim_location → OK
PASS=5 ERROR=0

## Verify dim_payment
```powershell
docker exec -it clickhouse-nyc clickhouse-client --query "SELECT * FROM raw.dim_payment"
```

## Expected dim_payment Result
1    CSH    Cash payment
2    CRE    Credit card payment
3    NOC    No charge
4    DIS    Dispute

## Verify dim_location
```powershell
docker exec -it clickhouse-nyc clickhouse-client --query "SELECT * FROM raw.dim_location"
```

## Expected dim_location Result
1    Manhattan      Manhattan borough
2    Queens         Queens borough
3    Brooklyn       Brooklyn borough
4    Staten Island  Staten Island borough
5    Bronx          Bronx borough

## Key Learnings
- row_number() OVER () = generates unique key
- CASE statement = IF ELSE in SQL
- DISTINCT = no duplicate values
- UNION DISTINCT = combine two lists, remove duplicates
- ref() = points to dbt model
- source() = points to raw data
- Star schema = fact table + dimension tables
- Dimension table = small reference table
- Fact table = large events table
