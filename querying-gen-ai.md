# Overview of gen ai

- Gen AI powered by foundational models (we know this one)
- Foundational Model is a type of machine learning
- Pre trained on vast amounts of data
- Selling us Bedrock
- Talking about RAG
- Vector embedding - turning raw data into a vector value, with no attached meaning.
- Challenges with vectors
  - Can take time to really generate the embeddings
  - Embedding size can range from 6152B => 6KiB. That's a lot. So 1 million rows might take up 6GB, so it could start spiralling, especially if you're generating a whole lot of them.
  - Query time, need to calculate distance to check them
- Way to handle long query time - Approximate Nearest Neighbour (ANN). Find similar vector without searching all, and faster than exact nearest neighbour.
- "Recall" (% of expected results) can vary so might not get everything you would expect.
- When choosing vector storage
  - Where does it fit into my workflow
  - How much data am I storing
  - What matters most here: storage, perf, relevancy, cost?
  - What are my tradeoffs - indexing, query time, schema design?

# Postgres as a vector store

- Will need to get RAG going to ensure you can provide data to the LLM so it becomes a lot more useful.
- Supports vector store
- Very old (Over 35 years), enterprise level performance.
- Biggest killer feature (that's what I'm calling it) is extensibility via extensions. Add new functionality.
- Vector search came in from that.
- Postgres can act as a persistent transactional store while working with other vector search systems.
- Great to be able to store everything side by side.
- Everything works without modification if you're already using it.
- What is pgvector?

  - open source extension which adds support for storage of vector data types, indexing, searching and metadata for vectors
    Supports IVFFlat/HNSW indexing
  - Exact nearest neighbour (K-NN) and Approximate Nearest Neightbour (ANN) for searcg
  - Colocate them with embeddings
  - Distance operators - define what distance actually is.

- Exact nearest neightbours - no index
- Fast indexing - IVFFlat
- Easy to manage, high performance - HNSW

# pgVector strategies and best practices

# Amazon Aurora features for vector queries
