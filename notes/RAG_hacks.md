# 10 Advanced RAG Techniques

## 1. Chunking R&D

Experiment with different **chunking strategies** to find the approach that gives the best retrieval performance.

## 2. Encoder R&D

Test different **encoder/embedding models** and use a test set to select the model that performs best.

## 3. Improve Prompts

Improve prompts by providing useful information such as:

- General content
- Current date
- Relevant context
- Conversation history

## 4. Document Pre-processing

Use an LLM to improve or transform the **chunks and/or text before encoding**, so they can be better represented during retrieval.

## 5. Query Rewriting

Use an LLM to convert the user's original question into a better **RAG query** for retrieval.

## 6. Query Expansion

Use an LLM to turn one question into **multiple RAG queries**, allowing the system to retrieve information from different perspectives.

## 7. Re-ranking

Retrieve a set of results first, then use an LLM to **sub-select or reorder the most relevant results** before passing them to the LLM.

## 8. Hierarchical RAG

Use an LLM to create **summaries at multiple levels**, allowing retrieval at different levels of detail.

## 9. Graph RAG

Retrieve content that is **closely related to similar documents or entities** by using relationships represented in a graph.

## 10. Agentic RAG

Use **agents for retrieval**, combining RAG with **memory and external tools such as SQL** to handle more complex retrieval tasks.

---
