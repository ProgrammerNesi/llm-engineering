# RAG Foundations: Tokens, Embeddings & Cosine Similarity

## What is a Token?

A **token** is the smallest unit of text that an LLM processes. It is **not always a complete word**. Depending on the tokenizer, a token can be a whole word, part of a word, punctuation, or even whitespace.

Example:

```
Artificial Intelligence is amazing!

↓

["Artificial", "Intelligence", "is", "amazing", "!"]
```

Each token is assigned a unique **Token ID** by the tokenizer. The Transformer never processes raw text—it only works with these token IDs.

---

## What is a Token ID?

A **Token ID** is the numerical representation of a token in the tokenizer's vocabulary.

Example:

```
"Artificial"

↓

Token ID = 4312
```

Every token has exactly one ID within a tokenizer's vocabulary.

---

## What is an Embedding?

An **embedding** is a dense numerical vector that represents the semantic meaning of text.

Example:

```
"Artificial"

↓

[0.24, -0.81, 0.45, ..., 0.19]
```

Unlike Token IDs, embeddings capture relationships and meanings between words.

---

## Token Embeddings

Inside an LLM, every Token ID is first converted into a **Token Embedding** using an embedding layer.

Pipeline:

```
Text

↓

Tokenizer

↓

Token IDs

↓

Embedding Layer

↓

Token Embeddings
```

These token embeddings are then processed by the Transformer.

---

## Chunk Embeddings

In RAG, we do **not** store token embeddings.

Instead, an **Embedding Model** converts an entire sentence, paragraph, or chunk into **one single embedding vector**.

Example:

```
Chunk

↓

"Artificial Intelligence is transforming healthcare."

↓

Embedding Model

↓

[0.18, -0.42, ..., 0.71]
```

One chunk always corresponds to one embedding vector.

---

## What is an Embedding Model?

An embedding model is a neural network trained specifically to convert text into vectors that preserve semantic meaning.

Unlike chat models, embedding models do **not generate text**.

Their only purpose is to map text into a vector space where semantically similar texts are located close to one another.

Examples:

- BAAI/bge-small-en-v1.5
- BAAI/bge-base
- e5-large
- text-embedding-3-small
- text-embedding-3-large

---

## Why Use Embeddings?

Computers cannot understand the meaning of words directly.

Embeddings convert language into mathematical vectors, allowing similarity to be measured using vector operations.

Similar meanings produce similar vectors.

Example:

```
Dog

↓

Vector A

Puppy

↓

Vector B
```

Vector A and Vector B will be very close.

---

## What is Cosine Similarity?

Cosine similarity is a metric used to measure how similar two vectors are by comparing the angle between them.

It ignores vector length and focuses only on direction.

Similarity Score:

- 1 → Exactly same meaning
- 0 → Unrelated
- -1 → Opposite direction (rare in embeddings)

Higher cosine similarity means higher semantic similarity.

---

## Why Cosine Similarity Instead of Euclidean Distance?

Embedding vectors may have different magnitudes but still represent the same meaning.

Cosine similarity ignores magnitude and compares only semantic direction, making it ideal for text retrieval.

Therefore, most embedding models and vector databases use cosine similarity.

---

# How RAG Actually Works

## Step 1: Documents

Suppose we have a document:

```
Artificial Intelligence is transforming healthcare by enabling faster diagnosis.
```

---

## Step 2: Chunking

The document is split into smaller pieces.

Example:

```
Chunk 1:
Artificial Intelligence is transforming healthcare.

Chunk 2:
It enables faster diagnosis.

Chunk 3:
It improves patient care.
```

Each chunk becomes an independent retrieval unit.

---

## Step 3: Tokenization

Each chunk is tokenized.

Example:

```
Artificial Intelligence is transforming healthcare

↓

["Artificial", "Intelligence", "is", "transforming", "healthcare"]
```

↓

```
[4312, 9811, 16, 8732, 15209]
```

---

## Step 4: Chunk Embedding

The embedding model processes all tokens in the chunk and produces **one embedding vector**.

```
Chunk

↓

Embedding Model

↓

Chunk Embedding
```

Example:

```
[0.13, -0.42, ..., 0.91]
```

---

## Step 5: Store in Vector Database

Each chunk embedding is stored in a vector database along with its original text.

```
Chunk

↓

Embedding

↓

Vector Database
```

The original chunk text is also stored as metadata.

---

## Step 6: User Query

Suppose the user asks:

```
How is AI used in hospitals?
```

The same embedding model converts the query into a vector.

```
Query

↓

Embedding Model

↓

Query Embedding
```

---

## Step 7: Similarity Search

The vector database compares the **Query Embedding** with every stored **Chunk Embedding** using cosine similarity.

```
Query Embedding

↓

Compare with Chunk 1

↓

0.92

Compare with Chunk 2

↓

0.81

Compare with Chunk 3

↓

0.45
```

The chunks with the highest similarity are retrieved.

---

## Step 8: Context Injection

The retrieved chunks are inserted into the LLM prompt.

Example:

```
Question:

How is AI used in hospitals?

Context:

Artificial Intelligence is transforming healthcare...

Answer:
```

The LLM now answers using the retrieved context instead of relying only on its internal knowledge.

---

# Important Concept

Many beginners think RAG compares individual words.

This is incorrect.

RAG compares:

```
Query Embedding

VS

Chunk Embedding
```

It does **not** compare:

```
Query Tokens

VS

Document Tokens
```

Tokens are only an intermediate step used to create embeddings.

---

# Complete RAG Pipeline

```
Documents

↓

Chunking

↓

Chunks

↓

Tokenizer

↓

Token IDs

↓

Embedding Model

↓

Chunk Embeddings

↓

Vector Database

────────────────────────────

User Query

↓

Tokenizer

↓

Embedding Model

↓

Query Embedding

↓

Cosine Similarity

↓

Top-K Similar Chunks

↓

LLM

↓

Final Response
```

---

# Key Takeaways

- A token is the smallest unit of text processed by an LLM.
- A Token ID is the numerical identifier assigned by the tokenizer.
- An embedding is a dense vector representing semantic meaning.
- Token embeddings are used internally by Transformers.
- Chunk embeddings are used in RAG for retrieval.
- Embedding models convert entire chunks into a single vector.
- Vector databases store chunk embeddings.
- Cosine similarity finds the most semantically similar chunks.
- RAG retrieves relevant chunks and provides them as context to the LLM before generation.