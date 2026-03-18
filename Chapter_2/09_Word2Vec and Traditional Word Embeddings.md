
# Section 11 — Word2Vec and Traditional Word Embeddings

This is where you understand:

```text
How machines first started understanding meaning
```

---

# Concept Explanation

Let’s start with the problem.

---

## Problem Before Word2Vec

Earlier systems like bag of words:

* only counted words
* no meaning
* no relationship

Example:

```text
king and queen → no connection
```

---

## What Word2Vec Changed

Word2Vec introduced a powerful idea:

```text
Words that appear in similar context have similar meaning
```

---

## Simple Example

```text
The king rules the kingdom  
The queen rules the kingdom
```

king and queen appear in similar context
model learns they are related

---

## Definition

```text
Word2Vec = converts words into vectors based on context
```

---

# Why This Concept Matters

This is the foundation of:

* embeddings
* semantic search
* recommendation systems
* modern LLM understanding

---

Without Word2Vec:

```text
No embeddings → No LLM
```

---

# Real World Example

---

## Google Search

When you search:

```text
best laptop for coding
```

Google understands:

* laptop
* programming
* developer needs

Not just keywords

---

## Recommendation Systems

Netflix or Spotify:

```text
User likes action movies
```

System recommends:

* similar movies

based on embedding similarity

---

# Technical Deep Dive

Now let’s understand how Word2Vec actually works.

---

# Core Idea

Word2Vec is trained like a prediction problem.

---

## Step 1 — Take a Sentence

```text
I love trading in stock market
```

---

## Step 2 — Create Training Data (Sliding Window)

Window size = 2

Focus word = love

Context words:

```text
I and trading
```

---

## Training pairs

```text
love → I  
love → trading
```

---

## Step 3 — Model Task

Model tries to learn:

```text
Are these words related or not
```

---

# Two Key Techniques

---

## 1 Skip Gram

```text
Given a word → predict its neighbors
```

Example:

```text
Input: love  
Output: I, trading
```

---

## 2 Negative Sampling

Important concept.

---

### Problem

If model only sees correct pairs:

```text
love → trading  
love → I
```

Model can cheat:

```text
Always say YES
```

---

### Solution

Add wrong examples:

```text
love → elephant  
love → car
```

Now model learns:

```text
Correct vs Incorrect relationships
```

---

## Final Learning

Model adjusts vectors so that:

```text
related words → closer  
unrelated words → far
```

---

# What Happens Internally

---

## Step 1

Each word gets random vector

---

## Step 2

Model trains on millions of examples

---

## Step 3

Vectors get updated

---

## Final Result

```text
king → [vector]  
queen → [vector]
```

very close in space

---

# Key Insight

```text
Meaning is learned from context, not definition
```

---

# Hands On Lab

## Objective

See word similarity using pretrained embeddings

---

## Code

```python
import gensim.downloader as api

model = api.load("glove-wiki-gigaword-50")

print(model.most_similar("king"))
```

---

## Expected Output

```text
queen  
prince  
emperor  
kingdom
```

---

## What You Learn

* embeddings capture meaning
* similarity can be measured
* model understands relationships

---

# Important Limitation

Word2Vec has a big problem.

---

## Static Embedding

```text
bank → same meaning everywhere
```

---

Example:

```text
river bank  
financial bank
```

Not the same vector 

---

# This Leads To

```text
Need for contextual embeddings → LLM
```

---

# Key Takeaways

* Word2Vec learns meaning from context
* Uses skip gram and negative sampling
* Produces word embeddings
* Foundation of modern NLP
* But embeddings are static

---

# One minute explanation

Word2Vec is the first system that taught computers how to understand meaning. It learns relationships between words based on how they appear together in sentences. If two words appear in similar contexts, the model places them close in vector space. This idea is the foundation of modern language models, even though Word2Vec itself cannot understand context dynamically.

