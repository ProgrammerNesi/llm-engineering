# RAG — Pros, Cons & Evals

**Advantages:** RAG provides a **rapid time to market**, gives an LLM access to a large amount of **domain-specific/external expertise**, can **reduce context length** by retrieving only relevant information, and helps avoid distracting the model with irrelevant context.

**Limitations:** RAG can sometimes feel like a **hack** because its performance is highly **empirical (trial and error)**. It may unexpectedly return **"I don't know"**, and even a good RAG design can produce poor results if retrieval or configuration is wrong.

**Important Point:** RAG performance depends heavily on design choices. For example, the **chunking strategy can significantly affect performance**. Therefore, instead of relying only on intuition and trial-and-error, we need **evaluations (evals)** to systematically measure how well the RAG system performs.

**Key Takeaway:** RAG is powerful, but building a good RAG system is not just about implementing retrieval — it requires **testing, measuring, and iterating using evals**.