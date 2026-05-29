## Create schema.yml File
```powershell
New-Item -ItemType File -Path models\marts\schema.yml
```

## schema.yml Content
```yaml
version: 2

models:
  - name: stg_trips
    description: "Cleaned and filtered NYC taxi trips"
    columns:
      - name: trip_id
        description: "Unique trip identifier"
        tests:
          - unique
          - not_null

      - name: fare_amount
        description: "Base fare amount"
        tests:
          - not_null

      - name: payment_type
        description: "Payment method"
        tests:
          - not_null
          - accepted_values:
              arguments:
                values:
                  - CSH
                  - CRE
                  - NOC
                  - DIS

      - name: passenger_count
        description: "Number of passengers"
        tests:
          - not_null

      - name: trip_distance
        description: "Trip distance in miles"
        tests:
          - not_null

  - name: dim_payment
    description: "Payment type dimension table"
    columns:
      - name: payment_key
        description: "Unique payment key"
        tests:
          - unique
          - not_null

      - name: payment_type
        description: "Payment type code"
        tests:
          - not_null

  - name: dim_location
    description: "Location dimension table"
    columns:
      - name: location_key
        description: "Unique location key"
        tests:
          - unique
          - not_null

      - name: borough
        description: "NYC borough name"
        tests:
          - not_null

  - name: fct_trips
    description: "Main fact table for NYC taxi trips"
    columns:
      - name: trip_id
        description: "Unique trip identifier"
        tests:
          - unique
          - not_null

      - name: fare_amount
        description: "Base fare amount"
        tests:
          - not_null

      - name: payment_key
        description: "Foreign key to dim_payment"
        tests:
          - not_null
          - relationships:
              arguments:
                to: ref('dim_payment')
                field: payment_key
```

## Delete Example Models
```powershell
Remove-Item -Recurse -Force models\example
```

## Run dbt Tests
```powershell
dbt test
```

## Expected Result
PASS=18 WARN=0 ERROR=0 ✅
All 18 tests passing!

## Run dbt Run (verify no warnings)
```powershell
dbt run
```

## Expected Clean Result
PASS=4 WARN=0 ERROR=0 ✅
No warnings!
No errors!

## Fix dbt_project.yml
Remove example section:
example:
+materialized: view

## Key Learnings
- unique test = no duplicate values
- not_null test = no empty values
- accepted_values = only valid values allowed
- relationships = keys match between tables
- schema.yml = where tests are defined
- dbt test = runs all tests automatically
- Always fix warnings to keep code clean!
