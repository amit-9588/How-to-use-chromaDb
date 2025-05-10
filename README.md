# How-to-use-chromaDb

### What is chromaDB ?
Chroma is an AI-native open-source vector database. It comes with everything you need to get started built in, and runs on your machine. 

### Why Use a VectorDB Instead of MySQL for Vectors?

| Feature                                     | MySQL                              | VectorDB (e.g., FAISS, Chroma, Milvus)      |
| ------------------------------------------- | ---------------------------------- | ------------------------------------------- |
| **Storage of vectors**                      | ✅ Yes (e.g., as JSON, BLOB, ARRAY) | ✅ Yes                                       |
| **Vector indexing (ANN)**                   | ❌ No                               | ✅ Yes (Approximate Nearest Neighbor Search) |
| **Similarity search (cosine, L2, dot)**     | ❌ No (manual, slow)                | ✅ Built-in, fast                            |
| **Scalability for high-dimensional search** | ❌ Poor                             | ✅ Optimized                                 |
| **Performance (Latency & Throughput)**      | ❌ Slow                             | ✅ Fast, millisecond responses               |
| **Specialized search algorithms**           | ❌ Absent                           | ✅ HNSW, IVF, PQ, etc.                       |
| **Metadata + vector filtering**             | ❌ Hard (joins needed)              | ✅ Native support                            |
| **Use case fit**                            | ❌ Not intended                     | ✅ Purpose-built for vectors                 |


### ✅ What Kind of Data Can You Store in ChromaDB?
ChromaDB is a vector database, but it does support storing non-vector data alongside vectors, in the form of:

🔹 1. Documents (documents):
Strings or text data.

Example: "This is a paragraph about neural networks."

🔹 2. Metadata (metadatas):
Structured data: numbers, strings, booleans (like JSON/dictionary).

`Example: {"topic": "AI", "year": 2023, "is_verified": True}`

🔹 3. Embeddings (embeddings):
Numeric vectors (usually list of floats).

Required only when adding pre-computed vectors.

Optional if you're using Chroma with embedding functions (like in LangChain).

✅ So yes, ChromaDB supports:

| Data Type                                   | Stored in ChromaDB?                | Notes                                      |
| ------------------------------------------- | ---------------------------------- | ------------------------------------------- |
| Text (string)                    | ✅ as documents |    Used for search or retrieval                               |
| Numbers                  | 	✅ in metadatas                          | e.g., {"price": 10} |
|Boolean/Date    | 	✅ in metadatas              | e.g., {"active": true}                  |
| Vectors (floats) | 	✅  in embeddings                        | Needed for similarity search                       |
| Images/Audio/etc.      | ❌ (raw)                           | You must convert to embeddings first         |


### 🚫 What You Can't Do in ChromaDB:
- Perform SQL-style joins.
- Store complex relational data.
- Use it as a full replacement for MySQL/Postgres.



### prerequisites
```
brew install python
```
This installs both `python3` and `pip3`.
-  python3  ```python3 --version```
-  pip3 ```pip3 --version```

### 1. Install
```
pip3 install chromadb             
```

### 2. Create a Chroma Client
```
import chromadb
chroma_client = chromadb.Client()   
```

### 3.  Create a collection
Collections are where you'll store your embeddings, documents, and any additional metadata. Collections index your
embeddings and documents, and enable efficient retrieval and filtering. You can create a collection with a name:
```
collection = chroma_client.create_collection(name="my_collection")          
```

### 4. Add some text documents to the collection
Chroma will store your text and handle embedding and indexing automatically. You can also customize the embedding model. 
You must provide unique string IDs for your documents.
```
collection.add(
    documents=[
        "This is a document about pineapple",
        "This is a document about oranges"
    ],
    ids=["id1", "id2"]
)
```

### 5. Query the collection
You can query the collection with a list of query texts, and Chroma will return the n most similar results. It's that easy!
```
results = collection.query(
    query_texts=["This is a query document"], 
    n_results=2 # how many results to return
)
print(results)
```
If `n_results` is not provided, Chroma will return 10 results by default. Here we only added 2 documents, so we set `n_results=2`.
