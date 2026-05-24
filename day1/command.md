# Day 1 Commands

## Docker Commands

### Check WSL version
```powershell
wsl --version
```

### Check processor type
```powershell
(Get-WmiObject -Class Win32_Processor).Architecture
```

### Pull and run ClickHouse container
```powershell
docker run -d `
  --name clickhouse-nyc `
  -p 8123:8123 `
  -p 9000:9000 `
  clickhouse/clickhouse-server
```

### Check running containers
```powershell
docker ps
```

### Connect to ClickHouse
```powershell
docker exec -it clickhouse-nyc clickhouse-client
```

### Start container after restart
```powershell
docker start clickhouse-nyc
```

## ClickHouse SQL Commands

### Show all databases
```sql
SHOW DATABASES;
```

### Create raw database
```sql
CREATE DATABASE raw;
```

### Create trips table
```sql
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

### Check table structure
```sql
DESCRIBE raw.trips;
```

### Load NYC Taxi data from S3
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

### Count loaded rows
```sql
SELECT count() FROM raw.trips;
```

### View sample data
```sql
SELECT * FROM raw.trips LIMIT 5;
```

### Analyze payment types
```sql
SELECT 
    payment_type,
    count() AS total_trips,
    avg(fare_amount) AS avg_fare
FROM raw.trips
GROUP BY payment_type;
```

## Key Learnings
- Always analyze data BEFORE creating table!
- Real world data has unexpected types!
- payment_type = text (CSH/CRE) not number!
- ClickHouse counted 1M rows in 0.002 seconds!
