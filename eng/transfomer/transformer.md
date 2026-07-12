# Transformer Architecture – From Input to Text Generation

## 1. Tokenization

- Neural networks cannot process words directly – only numbers.
- The first step is to split text into **tokens**.

### Tokenization approaches

- Character-level
- Word-level
- **Subword tokens (most common today)**

Example:

```text
unbelievable

↓

un
believe
able
```

- Most modern LLMs use **Byte Pair Encoding (BPE)** or similar algorithms.
- Advantages:
  - Handles unknown words
  - Handles typos
  - Handles different word forms
- **For simplicity, we'll assume word-level tokenization in our examples.**

---

# 2. Vocabulary

Each token receives a unique ID.

```text
The      → 1
cat      → 2
chased   → 3
mouse    → 4
because  → 5
it       → 6
was      → 7
hungry   → 8
```

This converts words into numbers...

...but these numbers carry **no semantic meaning**.

---

# 3. Word Embeddings

Instead of representing a word with a single number, each token is represented by a vector.

Example:

- Vocabulary size: 50,000
- Embedding dimension: 1,000

Embedding matrix:

```text
50000 × 1000
```

Each word is now represented by a vector of 1,000 numbers.

Advantages:

- Similar words become close together.
- Semantic relationships naturally emerge.

Example:

```text
cat
dog
mouse
```

are much closer than

```text
cat
car
```

### Problem

Word meaning depends on context.

Example:

```text
mouse
```

can refer to

- an animal
- a computer mouse

The embedding alone cannot distinguish between them.

---

# 4. Input to the Transformer

Sentence:

```text
The cat chased the mouse because it was hungry.
```

↓

Embedding vectors

↓

Input matrix

```text
8 × 1000
```

(Number of tokens × embedding dimension)

---

# 5. Self-Attention

Goal:

> Every token determines which other tokens are most relevant for understanding its meaning.

Example:

When processing

```text
it
```

the model tries to determine:

> Does **it** refer to **cat** or **mouse**?

---

# 6. Query, Key and Value

Each embedding is transformed three times using learned weight matrices.

```
Q = XWQ
K = XWK
V = XWV
```

Example:

```
X

8 × 1000

WQ

1000 × 200

↓

Q

8 × 200
```

Similarly

```
K

8 × 200

V

8 × 300
```

---

## Query

> What information am I looking for?

Example:

```text
it

↓

Who do I refer to?
```

---

## Key

> What kind of information can I provide to other words?

Example:

```text
cat

↓

I am a possible noun that a pronoun could refer to.
```

Think of it as a **book title**.

---

## Value

The actual information carried by the word.

- Meaning
- Grammar
- Context

Analogy:

- **Query** → What book am I looking for?
- **Key** → Book title
- **Value** → Book content

---

# 7. Attention

Compute

```
Q × Kᵀ
```

↓

Attention scores

↓

Similarity matrix

Example (for the token **it**):

| Token | Attention |
|--------|----------:|
| The | 2% |
| cat | 42% |
| chased | 5% |
| the | 1% |
| mouse | 35% |
| because | 5% |
| it | 5% |
| was | 3% |
| hungry | 2% |

Each row represents how much attention a token pays to every other token.

---

# 8. Contextual Embeddings

Compute

```
Attention × V
```

↓

New embedding vectors

The vectors:

- Keep the same dimensionality
- Gain contextual information
- Better represent the meaning of each word

Example:

The embedding of

```text
it
```

is updated using information from

```text
cat
mouse
because
```

making it context-aware.

---

# 9. Multiple Transformer Layers

This process is repeated many times.

Each layer performs:

- Q
- K
- V
- Attention
- New contextual embeddings

Each layer produces a richer representation of the sentence.

---

# Text Generation

Input:

```text
The cat chased the mouse because it was
```

↓

Transformer

↓

Final contextual embeddings

↓

Take **only the last embedding**

(the embedding of **was**)

↓

Final linear layer

↓

Probability distribution over the vocabulary

Example:

```text
hungry     68%
tired      12%
sleeping    5%
...
```

↓

Select the next token

↓

Append it to the sentence

```text
The cat chased the mouse because it was hungry.
```

↓

Repeat until the sequence is complete.

---

# Training

Training sentence:

```text
The cat chased the mouse because it was hungry.
```

The model receives the **entire sentence** at once.

Targets are shifted by one token.

| Input | Target |
|--------|--------|
| The | cat |
| cat | chased |
| chased | the |
| the | mouse |
| mouse | because |
| because | it |
| it | was |
| was | hungry |
| hungry | EOS |

The model predicts the next token for **every position simultaneously**.

Loss is computed.

Backpropagation updates:

- Embedding matrix
- WQ
- WK
- WV
- All remaining Transformer weights

---

# Causal Mask

Purpose:

Prevent the model from seeing future tokens during training.

Without masking:

```text
The cat chased the mouse because it was hungry.
```

The token

```text
was
```

could already attend to

```text
hungry
```

making the prediction trivial.

The mask is applied **after**

```
QKᵀ
```

and **before**

```
Softmax
```

```
QKᵀ

↓

Mask

↓

Softmax

↓

Attention × V
```

As a result:

- **The** sees only **The**
- **cat** sees **The, cat**
- **chased** sees **The, cat, chased**
- ...
- **was** cannot see **hungry**
- **hungry** cannot see **EOS**

This ensures that the model learns under the same conditions in which it will later generate text.

# What Are Large Language Models (LLMs)?

Large Language Models (LLMs) are a class of deep neural networks designed to understand and generate natural language.

They are built using the **Transformer architecture** and trained on enormous amounts of text collected from sources such as:

- Books
- Articles
- Websites
- Source code
- Scientific papers
- Public online discussions

---

## How Do LLMs Work?

Although they appear to understand language, their fundamental training objective is surprisingly simple:

> **Predict the most probable next token (word or subword).**

For example:

```text
The capital of France is ...
```

The model predicts that the most likely continuation is:

```text
Paris
```

By repeating this prediction thousands of times while generating text, the model can produce coherent paragraphs, answer questions, write code, and perform many other language-related tasks.

---

## Why Are LLMs So Powerful?

Modern LLMs contain **billions of trainable parameters** distributed across many layers of neural networks.

During training, these parameters learn statistical relationships between words, sentences, concepts, and even programming languages.

As a result, the model can generalize to tasks it was never explicitly trained for.

Examples include:

- Question answering
- Translation
- Summarization
- Code generation
- Text classification
- Reasoning over natural language

