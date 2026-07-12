# LLM Evaluation

## Introduction

Evaluation is the process of measuring the quality of a language model's outputs.

The goal is to objectively determine how well a model performs on different tasks.

Modern evaluation goes beyond simple accuracy and considers multiple dimensions simultaneously.

Typical evaluation criteria include:

- correctness
- factuality
- relevance
- helpfulness
- coherence
- safety
- robustness

Unlike traditional machine learning tasks, many LLM tasks do not have a single correct answer.

---

# Offline Evaluation

Offline evaluation measures model performance using predefined datasets before deployment.

It is commonly used for:

- model development
- benchmarking
- regression testing
- model comparison

Offline evaluation can be divided into:

- training metrics
- automatic evaluation metrics
- embedding-based metrics

---

# Cross-Entropy Loss

Cross-entropy is the primary **training objective**, not an evaluation metric.

During training:

- the model predicts probabilities for the next token
- the correct token (ground truth) is known
- the model is penalized when it assigns a low probability to the correct token

Properties:

- high probability of the correct token → low loss
- low probability of the correct token → high loss

The model learns by minimizing cross-entropy loss.

---

# Perplexity (PPL)

Perplexity is derived from cross-entropy.

It measures how uncertain a language model is when predicting text.

Properties:

- low perplexity → confident predictions
- high perplexity → uncertain predictions

$$
PPL = \exp\left(-\frac{1}{N}\sum_{i=1}^{N}\log P(w_i)\right)
$$

Example:

Sentence:

```
The cat chased the mouse
```

If the model assigns high probabilities to the correct tokens, perplexity will be low.

Interpretation:

> A perplexity of 3 means the model behaves as if it is choosing among roughly three equally likely next tokens.

Limitations:

- does not measure semantic quality
- does not evaluate factual correctness
- mainly useful for evaluating base language models

---

# BLEU

(Bilingual Evaluation Understudy)

BLEU compares generated text with a reference using n-gram precision.

It is widely known from machine translation.

Principle:

- split text into n-grams
- compare generated n-grams with reference n-grams
- compute precision
- apply a brevity penalty

Characteristics:

- precision-oriented
- typically uses 1–4 grams
- requires a reference answer

Typical applications:

- machine translation
- text generation

Advantages:

- simple
- reproducible
- automatic

Limitations:

- ignores meaning
- penalizes paraphrases
- sensitive to wording
- less suitable for open-ended generation

---

# ROUGE

(Recall-Oriented Understudy for Gisting Evaluation)

ROUGE measures how much information from the reference appears in the generated text.

Unlike BLEU, it focuses on **recall**.

Applications:

- text summarization

Common variants:

## ROUGE-N

Measures shared n-grams.

## ROUGE-L

Measures the longest common subsequence.

Comparison:

BLEU:

> "Don't generate unnecessary words."

ROUGE:

> "Don't miss important information."

Limitations:

- ignores semantics
- penalizes paraphrases
- rewards lexical overlap

---

# Exact Match (EM)

Exact Match checks whether the generated answer is identical to the reference.

Returns:

- 1 → identical
- 0 → different

Example:

Reference:

```
Elizabeth II
```

Prediction:

```
Queen Elizabeth II
```

Exact Match = 0

Applications:

- question answering
- factual datasets
- short answers

Limitations:

- extremely strict
- ignores semantic similarity

---

# F1 Score

F1 combines:

- precision
- recall

Unlike Exact Match, it measures partial overlap.

Typical applications:

- question answering
- entity extraction

Advantages:

- rewards partially correct answers
- less strict than Exact Match

---

# Embedding-Based Metrics

Traditional metrics compare words.

Embedding-based metrics compare **meaning**.

Both the reference and generated text are converted into embeddings.

Similarity is then measured using cosine similarity.

Advantages:

- captures semantics
- recognizes paraphrases
- handles synonyms

Limitations:

- does not verify factual correctness
- depends on the embedding model
- computationally more expensive

---

# BERTScore

BERTScore compares token embeddings rather than words.

Process:

- obtain embeddings for every token
- find the most similar token in the other sentence
- compute precision, recall and F1

Advantages:

- robust to paraphrases
- captures semantic similarity

Disadvantages:

- slower
- model-dependent

---

# Sentence Embeddings

Instead of comparing individual tokens, the entire sentence is represented by a single embedding.

Advantages:

- fast
- simple

Disadvantages:

- loses fine-grained information
- generally less accurate than token-level methods

---

# Human Evaluation

Human evaluation remains the gold standard for evaluating LLM outputs.

Human evaluators assess:

- correctness
- helpfulness
- clarity
- factuality
- relevance

Advantages:

- understands meaning
- evaluates context
- detects hallucinations

Disadvantages:

- expensive
- slow
- subjective

---

# Common Human Evaluation Methods

## Rating

Assign a numerical score.

Example:

```
1–5
```

Simple but subjective.

---

## Pairwise Comparison

Compare two model outputs.

Choose the better one.

Generally produces more consistent results than rating.

---

## Rubric-Based Evaluation

Evaluate using predefined criteria.

Examples:

- correctness
- completeness
- clarity
- usefulness

Provides more structured evaluation.

---

## Task-Specific Evaluation

Uses criteria designed for a specific application.

Examples:

- customer support
- legal assistants
- medical AI
- coding assistants

---

# LLM-as-a-Judge

Modern evaluation increasingly uses another LLM as the evaluator.

Instead of human annotators, an LLM judges the quality of responses.

Advantages:

- scalable
- inexpensive
- correlates well with human evaluation

Common approaches:

- pairwise comparison
- rubric prompting
- reference-free evaluation

Potential biases:

- position bias
- verbosity bias
- self-preference bias
- inconsistency

Mitigation strategies:

- multiple evaluation runs
- randomized answer order
- carefully designed prompts
- calibration against human judgments

---

# Online Evaluation

After deployment, models are evaluated using real user interactions.

Typical methods include:

- A/B testing
- user feedback
- click-through rates
- task completion rates
- production monitoring

Online evaluation measures actual user experience rather than benchmark performance.

---

# Agent Evaluation

Modern AI systems often use autonomous agents rather than single model calls.

Evaluation therefore includes:

- task completion
- planning quality
- tool selection
- tool usage
- execution cost
- latency
- number of reasoning steps

Agent evaluation is becoming increasingly important as AI systems become more autonomous.

---

# Benchmarks

Benchmarks provide standardized datasets for comparing language models.

A benchmark typically contains:

- tasks
- reference answers
- evaluation metrics

Every model solves the same tasks, making objective comparison possible.

---

# Common Benchmarks

## GLUE

Historic NLP benchmark.

Includes tasks such as:

- sentiment analysis
- textual entailment
- sentence similarity

---

## SuperGLUE

More difficult successor to GLUE.

Focuses on advanced language understanding and reasoning.

---

## MMLU

Massive Multitask Language Understanding.

Measures:

- knowledge
- reasoning
- multiple academic disciplines

---

## GPQA

Graduate-level scientific reasoning benchmark.

Designed to evaluate difficult expert-level questions.

---

## BIG-bench

Large collection of diverse reasoning tasks.

Includes:

- logic
- creativity
- mathematics
- language understanding

---

## HELM

(Holistic Evaluation of Language Models)

Evaluates:

- performance
- robustness
- fairness
- bias
- calibration

---

## MMMU

Massive Multi-discipline Multimodal Understanding.

Evaluates multimodal reasoning using:

- text
- images
- diagrams
- charts

---

## SWE-bench

Evaluates AI software engineering systems.

The benchmark measures whether models can solve real GitHub issues.

Widely used for coding agents.

---

## LiveBench

Continuously updated benchmark.

Designed to reduce benchmark contamination and better reflect current model capabilities.

---

## Humanity's Last Exam

One of the most challenging reasoning benchmarks.

Focuses on problems that remain difficult even for the strongest frontier models.

---

# Benchmark Limitations

Benchmarks are useful, but they have important limitations.

Common problems:

- data contamination
- benchmark overfitting
- limited real-world coverage
- optimization for benchmark scores instead of actual usefulness

High benchmark scores do not necessarily imply better performance in production.

---

# Leaderboards

Popular model leaderboards include:

- Artificial Analysis
- LMArena

These aggregate results across multiple benchmarks and provide comparisons between leading models.

---

# Modern LLM Evaluation Pipeline

Modern AI systems are typically evaluated using multiple complementary approaches.

```
Offline Evaluation

↓

Human Evaluation

↓

LLM-as-a-Judge

↓

Online A/B Testing

↓

Production Monitoring
```

No single metric is sufficient on its own.

---

# Summary

Evaluating LLMs requires multiple perspectives.

Training:

- Cross-Entropy

Automatic metrics:

- Perplexity
- BLEU
- ROUGE
- Exact Match
- F1

Semantic metrics:

- BERTScore
- Sentence Embeddings

Human-centered evaluation:

- Human Evaluation
- LLM-as-a-Judge

Real-world evaluation:

- Online Evaluation
- Agent Evaluation
- Production Monitoring

The most reliable assessment combines several evaluation methods rather than relying on a single metric.