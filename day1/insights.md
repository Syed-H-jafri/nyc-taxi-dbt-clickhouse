# Day 1 Insights — NYC Taxi Data

## 🔍 What I Discovered

### Payment Type Analysis
| Payment Type | Total Trips | Avg Fare |
|-------------|-------------|----------|
| CSH (Cash)  | 617,650     | $13.59   |
| CRE (Credit)| 378,143     | $12.55   |
| NOC (No Charge) | 3,573   | $12.39   |
| DIS (Dispute)   | 1,294   | $10.93   |

## 💡 Business Insights

### Insight 1: Cash is King!
Cash trips = 617,650
Card trips = 378,143
Cash is 62% of all trips!
NYC people preferred cash in 2015!
### Insight 2: Cash Riders Pay More!

### Insight 3: Lost Revenue!
No Charge trips = 3,573
Avg fare = $12.39
Lost revenue = ~$44,000!

### Insight 4: Disputed Trips
Disputed trips = 1,294
Lowest avg fare = $10.93
Short trips = more disputes!

## ⚡ Performance Insight
1 million rows counted in 0.002 seconds!
1 million rows queried in 0.020 seconds!
This is the POWER of columnar database!

## 🎓 What I Learned Today

### About Docker:
- Docker runs locally on laptop
- Containers are isolated environments
- No mess on actual system
- One command to start/stop

### About ClickHouse:
- Columnar database = super fast analytics
- Stores data by column not by row
- Only reads columns needed for query
- Bad for updates/deletes
- Perfect for analytics!

### About Real World Data:
- Always analyze data BEFORE creating table!
- payment_type was text not number!
- Location columns were empty for many trips!
- Real data is always messy!

## 🚀 Tomorrow — Day 2
- Install dbt
- Connect dbt to ClickHouse
- Build first staging model!
