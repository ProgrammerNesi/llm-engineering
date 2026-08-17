# RAG Evaluations

Evaluations are essential for improving a RAG pipeline because they let us **measure retrieval quality and answer quality instead of relying only on trial and error**.

## 1. Curate a Test Set

Create a set of **example questions** where the correct/relevant context is already identified and **reference answers** are provided. This test set becomes the basis for consistently evaluating changes to the RAG pipeline.

## 2. Measure Retrieval

Retrieval evaluates whether the RAG system is actually finding the right chunks.

### MRR — Mean Reciprocal Rank

MRR measures the **average inverse rank of the first relevant result**.

If the first relevant chunk is ranked:

- 1st → score = `1`
- 2nd → score = `1/2`
- 3rd → score = `1/3`

A higher MRR means relevant information tends to appear higher in the results.

### nDCG — Normalized Discounted Cumulative Gain

nDCG measures whether **relevant chunks are ranked higher** in the retrieved results. Highly relevant chunks appearing near the top contribute more than the same chunks appearing lower down.

### Recall@K

Recall@K measures the **proportion of test questions where the relevant context appears within the top K retrieved chunks**.

For example, with `K = 5`, it asks:

> "Did the relevant chunk appear somewhere in the top 5 results?"

If you have multiple keywords/concepts that need to be found, **keyword coverage** can serve as a similar recall-oriented metric.

### Precision@K

Precision@K measures the **proportion of the top K retrieved chunks that are actually relevant**.

For example, if 3 out of the top 5 chunks are relevant:

`Precision@5 = 3/5 = 0.6`

## 3. Measure Answers

After evaluating retrieval, evaluate the **quality of the final generated answer**. An **LLM-as-a-judge** can score answers against criteria such as:

- **Accuracy** — Is the answer correct?
- **Completeness** — Does it contain the required information?
- **Relevance** — Does it actually answer the question?

## Overall RAG Evaluation

```text
Test Set
   ↓
Measure Retrieval
   ├── MRR
   ├── nDCG
   ├── Recall@K
   └── Precision@K
   ↓
Measure Generated Answers
   ├── Accuracy
   ├── Completeness
   └── Relevance


                    tests.jsonl
                         │
                         ↓
                  load_tests()
                         │
                         ↓
                    TestQuestion
                         │
             ┌───────────┴───────────┐
             ↓                       ↓
     Retrieval Evaluation      Answer Evaluation
             │                       │
             ↓                       ↓
      fetch_context()         answer_question()
             │                       │
             ↓                       ↓
          Chroma                  Chroma
             │                       │
             ↓                       ↓
       Retrieved Docs            Gemini
             │                       │
       ┌─────┼─────┐               ↓
       ↓     ↓     ↓          Generated Answer
      MRR   nDCG  Coverage           │
                                      ↓
                                  LLM Judge
                                      │
                             ┌────────┼────────┐
                             ↓        ↓        ↓
                          Accuracy Complete Relevance

Conclusion:
MRR = Average inverse rank of first hit; 1 if the first chunk always has relevant context

nDCG = Did relevant chunks get ranked higher up

Recall@K = Proportion of tests where relevant context was in the top K chunks

Or if you have multiple keywords to look for, keyword coverage is similar recall metric

Precision@K = Proportion of the top K chunks that are relevant