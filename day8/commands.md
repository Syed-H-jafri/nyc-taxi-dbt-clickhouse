# Day 8 Commands — Power BI Connection

## Download Power BI Desktop
Go to: https://powerbi.microsoft.com/desktop
Click: Download free
Install: PBIDesktopSetup_x64.exe

## Download ClickHouse ODBC Driver
Go to: https://github.com/ClickHouse/clickhouse-odbc/releases/latest
Download: clickhouse-odbc-*-win64.msi
Install: double click and follow steps

## Connect Power BI to ClickHouse

Open Power BI Desktop
Click "Get data from other sources"
Search "click"
Select "ClickHouse"
Click Connect
Click Continue (third party warning)
Fill in:
Host: localhost
Port: 8123
Database: raw
Mode: Import
Click OK
Username: default
Password: (empty)
Click Connect


## Select Tables to Import
In Navigator window:
✅ fct_trips
✅ dim_payment
✅ dim_location
Click Load!

## Set Up Relationships in Model View

Click 3rd icon on left sidebar
Drag payment_key from fct_trips
to payment_key in dim_payment
Drag pickup_location_key from fct_trips
to location_key in dim_location


## Build First Card Visual

Click Card visual (123 icon)
Expand fct_trips
Check trip_id
Change to Count
Result: 993,708K total trips!


## Build Total Revenue Card

Click Card visual
Check total_amount
Change to Sum
Result: 16.07M total revenue!


## Build Average Fare Card

Click Card visual
Check fare_amount
Change to Average
Result: 12.97 avg fare!


## Build Bar Chart

Click Clustered Bar Chart
Y-Axis: dim_payment → payment_description
X-Axis: fct_trips → trip_id → Count
Result: Trips by payment type!


## Save Dashboard
Press Ctrl + S
Name: NYC_Taxi_Dashboard
Location: D:\

## Key Learnings
- ODBC = Open Database Connectivity
- ODBC = universal translator between apps and databases
- Import mode = copies data into Power BI
- Relationship = connection between tables via keys
- Card visual = shows single number (KPI)
- Bar chart = compares categories
- Always set up relationships in Model View!
