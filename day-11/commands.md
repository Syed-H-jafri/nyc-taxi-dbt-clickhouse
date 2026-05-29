# Day 11 Commands — Embeddings

## Install sentence-transformers
```powershell
python -m pip install sentence-transformers
```

## Create documents.py
```powershell
New-Item -ItemType File -Path D:\nyc_taxi\documents.py
```

## Create create_embeddings.py
```powershell
New-Item -ItemType File -Path D:\nyc_taxi\create_embeddings.py
```

## Run Embeddings Creator
```powershell
python create_embeddings.py
```

## Expected Result
⏳ Loading embedding model...
✅ Model loaded!
⏳ Creating doc_chunks table...
✅ Table created!
📄 Processing: NYC Taxi Fare Structure
📄 Processing: NYC Borough Guide
📄 Processing: NYC Taxi Payment Analysis
📄 Processing: NYC Taxi Trip Patterns
📄 Processing: NYC Taxi Data Overview
✅ Created 5 chunks!
✅ Embeddings stored in ClickHouse!
✅ Total chunks in database: 5
🎉 Done! Ready for RAG!

## Verify Chunks in ClickHouse
```powershell
docker exec -it clickhouse-nyc clickhouse-client --query "SELECT chunk_id, title, substring(chunk_text, 1, 100) FROM raw.doc_chunks"
```

## Check Specific Chunk
```powershell
docker exec -it clickhouse-nyc clickhouse-client --query "SELECT chunk_text FROM raw.doc_chunks WHERE title = 'NYC Taxi Payment Analysis'"
```

## doc_chunks Table Structure
```sql
CREATE TABLE raw.doc_chunks (
    chunk_id    UInt64,
    title       String,
    chunk_text  String,
    embedding   Array(Float32)
)
ENGINE = MergeTree()
ORDER BY chunk_id
```

## Key Learnings
- Embeddings = text converted to numbers
- Similar text = similar numbers
- all-MiniLM-L6-v2 = embedding model used
- Runs locally = no API needed!
- chunk_size = 200 words per chunk
- Array(Float32) = stores embedding vectors
- Better documents = better RAG answers!
- Always recreate embeddings after updating docs!
