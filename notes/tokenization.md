Tokenization is the process of converting text into tokens and mapping each token to a unique token ID using a tokenizer and its vocabulary. These token IDs are converted into embeddings, which are processed by the Transformer. The Transformer produces a probability distribution over the entire vocabulary for the next token, selects one token, and its token ID is finally decoded back into human-readable text by the tokenizer.



# Complete Mental Pipeline

```
Messages
(system + user)

↓

Chat Template

↓

Formatted Prompt

↓

Tokenizer

↓

Token IDs

↓

PyTorch Tensor

↓

Move to GPU

↓

Transformer Model

↓

Predict Next Token

↓

Generate Next Token

↓

Repeat

↓

Token IDs

↓

Tokenizer Decode

↓

Human-readable Text
```

## The single most important thing to remember

Only **two components understand text**:

1. **The tokenizer** (Text ↔ Token IDs)
2. **You**, the human.

The **Transformer never sees text**. It only receives **embeddings derived from token IDs**, performs computations on those numerical representations, predicts the next token IDs, and relies on the tokenizer again to convert those IDs back into text. Keeping this separation clear is one of the most important mental models in LLM engineering.