# Day 2 Commands — dbt Setup + Connection

## Check Python Version
```powershell
python --version
```

## Install dbt
```powershell
python -m pip install dbt-core dbt-clickhouse
```

## Verify dbt Version
```powershell
dbt --version
```

## Create dbt Project
```powershell
cd D:\
dbt init nyc_taxi
```

## Open profiles.yml
```powershell
notepad C:\Users\Owner\.dbt\profiles.yml
```

## profiles.yml Content
```yaml
nyc_taxi:
  target: dev
  outputs:
    dev:
      type: clickhouse
      schema: raw
      host: localhost
      port: 8123
      user: default
      password: ""
      secure: False
```

## Fix ClickHouse Password Issue
```powershell
docker stop clickhouse-nyc
docker rm clickhouse-nyc
docker run -d `
  --name clickhouse-nyc `
  -p 8123:8123 `
  -p 9000:9000 `
  -e CLICKHOUSE_DEFAULT_ACCESS_MANAGEMENT=1 `
  -e CLICKHOUSE_PASSWORD="" `
  clickhouse/clickhouse-server
```

## Reload Data After Container Reset
```powershell
docker exec -it clickhouse-nyc clickhouse-client
```
```sql
CREATE DATABASE raw;
CREATE TABLE raw.trips (
    trip_id          UInt64,
    pickup_datetime  DateTime,
    dropoff_datetime DateTime,
    passenger_count  UInt8,
    trip_distance    Float32,
    fare_amount      Float32,
    tip_amount       Float32,
    total_amount     Float32,
    payment_type     String,
    pickup_location  String,
    dropoff_location String
)
ENGINE = MergeTree()
ORDER BY (pickup_datetime, trip_id);
```

## Load NYC Taxi Data Again
```powershell
docker exec -it clickhouse-nyc clickhouse-client --query "
INSERT INTO raw.trips
SELECT
    rowNumberInAllBlocks() AS trip_id,
    pickup_datetime,
    dropOff_datetime AS dropoff_datetime,
    passenger_count,
    trip_distance,
    fare_amount,
    tip_amount,
    total_amount,
    payment_type,
    pickup_boroname AS pickup_location,
    dropoff_boroname AS dropoff_location
FROM url(
    'https://datasets-documentation.s3.eu-west-3.amazonaws.com/nyc-taxi/trips_0.gz',
    'TabSeparatedWithNames'
)
"
```

## Test dbt Connection
```powershell
cd D:\nyc_taxi
dbt debug
```

## Expected Result
All checks passed! ✅

## Key Learnings
- dbt is a Python library like pandas
- dbt-core = brain of dbt
- dbt-clickhouse = ClickHouse connector
- profiles.yml = connection settings
- profiles.yml lives in C: drive (global!)
- dbt project lives in D: drive (local!)
- Schema = folder inside database
- Dependencies = correct build order
- Always run dbt debug before building!
