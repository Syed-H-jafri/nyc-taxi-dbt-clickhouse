# Day 3 Commands — Staging Model

## Open dbt Project in VS Code
File → Open Folder → D:\nyc_taxi

## Create Staging Folder
```powershell
New-Item -ItemType Directory -Path models\staging
```

## Create stg_trips.sql File
```powershell
New-Item -ItemType File -Path models\staging\stg_trips.sql
```

## stg_trips.sql Content
```sql
SELECT
    -- identifiers
    trip_id,

    -- timestamps
    pickup_datetime,
    dropoff_datetime,

    -- calculated column
    dateDiff('minute',
             pickup_datetime,
             dropoff_datetime)
             AS trip_duration_min,

    -- trip details
    passenger_count,
    trip_distance,

    -- financials
    fare_amount,
    tip_amount,
    total_amount,

    -- payment
    payment_type,

    -- locations renamed
    pickup_location  AS pickup_borough,
    dropoff_location AS dropoff_borough

FROM {{ source('raw', 'trips') }}

WHERE
    fare_amount > 0
    AND passenger_count > 0
    AND trip_distance > 0
```

## Create sources.yml File
```powershell
New-Item -ItemType File -Path models\staging\sources.yml
```

## sources.yml Content
```yaml
version: 2

sources:
  - name: raw
    database: raw
    schema: raw
    tables:
      - name: trips
        description: "Raw NYC taxi trips loaded from S3"
        columns:
          - name: trip_id
            description: "Unique trip identifier"
          - name: pickup_datetime
            description: "When trip started"
          - name: dropoff_datetime
            description: "When trip ended"
          - name: passenger_count
            description: "Number of passengers"
          - name: trip_distance
            description: "Trip distance in miles"
          - name: fare_amount
            description: "Base fare amount"
          - name: tip_amount
            description: "Tip amount"
          - name: total_amount
            description: "Total amount charged"
          - name: payment_type
            description: "Payment method CSH CRE NOC DIS"
          - name: pickup_location
            description: "Pickup borough name"
          - name: dropoff_location
            description: "Dropoff borough name"
```

## Run dbt
```powershell
dbt run
```

## Expected Result
2 of 3 OK created sql view model raw.stg_trips ✅
Completed successfully!
PASS=3 ERROR=0

## Verify stg_trips Data
```powershell
docker exec -it clickhouse-nyc clickhouse-client --query "SELECT * FROM raw.stg_trips LIMIT 5"
```

## Key Learnings
- dbt model = just a SQL file!
- VIEW = window to data not real table
- source() = points to raw data
- ref() = points to dbt models
- dateDiff = calculates time difference
- WHERE = filters bad data
- staging = data cleaning layer
- Always analyze data before building!
