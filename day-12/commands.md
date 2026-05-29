# Day 12 Commands — RAG Assistant

## Create rag.py
```powershell
New-Item -ItemType File -Path D:\nyc_taxi\rag.py
```

## Run RAG Assistant
```powershell
python rag.py
```

## Test Questions for RAG
Why do cash riders pay more than credit card riders?
What time are taxis busiest?
Why are tips higher for credit cards?
How are NYC taxi fares calculated?
What are the five NYC boroughs?

## Update documents.py (if needed)
Open documents.py
Add more detail to relevant section
Save file
Then recreate embeddings!

## Recreate Embeddings After Update
```powershell
python create_embeddings.py
```

## Verify Updated Chunks
```powershell
docker exec -it clickhouse-nyc clickhouse-client --query "SELECT chunk_text FROM raw.doc_chunks WHERE title = 'NYC Taxi Payment Analysis'"
```

## Example RAG Results
Question: Why do cash riders pay more?
Found chunks:
→ NYC Taxi Payment Analysis (0.468)
→ NYC Taxi Fare Structure (0.278)
→ NYC Taxi Data Overview (0.162)
Answer:
Cash riders pay more because they
tend to take slightly longer trips.
Cash avg fare = $13.51
Credit avg fare = $12.28 ✅
Question: Why tips higher for credit cards?
Answer:
Credit card tips are higher because
passengers are prompted on card machine
to add tip automatically!
Suggests 20, 25 or 30 percent! ✅

## Key Learnings
- RAG = Retrieval Augmented Generation
- similarity_search() = finds relevant chunks
- cosine_similarity() = measures text similarity
- generate_answer() = AI reads chunks + answers
- top_k=3 = retrieve top 3 most relevant chunks
- Better documents = better answers!
- AI honest when info not in documents!
- Update docs + recreate embeddings = better RAG!
