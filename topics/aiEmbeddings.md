One direction that I find very promising is to use LLMs to generate embeddings and then build your ML applications on top of these embeddings, e.g. for search and recsys. As of April 2023, the cost for embeddings using the smaller model text-embedding-ada-002 is $0.0004/1k tokens. If each item averages 250 tokens (187 words), this pricing means $1 for every 10k items or $100 for 1 million items.

# Embeddings

Embeddings are a numerical representation of text that can be used to measure the relatedness between two pieces of text.

Turn data into a vector with hundreds of dimensions.

Data, like words, converted into an array of numbers, known as a vector, which contains pattern of relationship between the data.

Open AI ada = 8,191 tokens i.e. 32,764 characters ~ 10 pages of text.

# Vector databases

> What pieces of text in the database have similar vectors to the prompt.

A vector database indexes and stores vector embeddings for fast retrieval and similarity search. It compares how close embeddings are.

It turns the user's question into a vector, which is compared to the vectors in the database.

Ex. Pinecone, Qdrant, Weaviate, Chroma, Faiss, Redis, Milvus, ScaNN.
