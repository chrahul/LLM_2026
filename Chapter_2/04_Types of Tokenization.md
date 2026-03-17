

# Types of Tokenization

*(Word vs Subword vs Character vs Byte)*

---

# Big Picture First

When we say:

```text
"Language model breaks text into tokens"
```

The question is:

 **How exactly does it break?**

There are **4 main ways** to do it:

```id="types"
1. Word Tokenization
2. Subword Tokenization   (Most Important)
3. Character Tokenization
4. Byte Tokenization
```

---

# 1️ Word Tokenization

## Concept Explanation

This is the simplest approach.

 Split text into **words**

Example:

```text id="ex1"
"I love Kubernetes"
```

Tokens:

```text id="tok1"
["I", "love", "Kubernetes"]
```

---

## Why This Matters

This was used in early NLP systems like:

* word2vec

---

## Problem

### Problem 1 — Vocabulary Explosion

```text id="ex2"
run
running
runner
runs
```

All become separate tokens.

Vocabulary becomes huge.

---

### Problem 2 — Unknown Words

```text id="ex3"
ChatGPTization
```

Tokenizer:

 "I don't know this word"

---

## Key Takeaway

Word tokenization is simple but:

```id="kt1"
 Not scalable
 Cannot handle new words
```

---

# 2️ Subword Tokenization (MOST IMPORTANT)

## Concept Explanation

Instead of whole words, we break into **smaller meaningful parts**.

Example:

```text id="ex4"
apologizing
```

Tokens:

```text id="tok2"
["apolog", "izing"]
```

---

## Why This Is Powerful

### Problem 1 Solved — Vocabulary

Instead of:

```text id="ex5"
apology
apologize
apologizing
apologist
```

We use:

```text id="tok3"
apolog + suffix
```

---

### Problem 2 Solved — New Words

```text id="ex6"
DevOpsGPTization
```

Tokenizer:

```text id="tok4"
Dev + Ops + GPT + ization
```

 Works even if word is new.

---

## Real Models Using This

* GPT-2
* GPT-3
* GPT-4
* LLaMA

---

## Trade-Off

```id="trade1"
More tokens than word-level
But better generalization
```

---

## Key Takeaway

```id="kt2"
Subword tokenization = Best balance
```

---

# 3️ Character Tokenization

## Concept Explanation

Break everything into **characters**

Example:

```text id="ex7"
"play"
```

Tokens:

```text id="tok5"
["p", "l", "a", "y"]
```

---

## Advantage

```id="adv1"
Handles ANY word
```

Even:

```text id="ex8"
KubernetesXYZ
```

---

## Problem

### Too Many Tokens

```text id="ex9"
"machine"
```

Word tokens:

```text id="tok6"
["machine"] → 1 token
```

Character tokens:

```text id="tok7"
["m","a","c","h","i","n","e"] → 7 tokens
```

---

## Impact

```id="impact1"
More tokens → more computation → slower model
```

---

## Key Takeaway

```id="kt3"
Flexible but inefficient
```

---

# 4️ Byte Tokenization

## Concept Explanation

Everything is broken into **bytes (binary level)**.

Example:

```text id="ex10"
"A"
```

Byte representation:

```text id="tok8"
[65]
```

Unicode characters:

```text id="ex11"

```

Becomes multiple bytes.

---

## Why This Matters

* handles any language
* handles emojis
* no unknown tokens

---

## Used In Research Models

Some models experiment with this approach.

---

## Trade-Off

```id="trade2"
Very flexible but complex to train
```

---

# Comparison Summary

| Type      | Advantage     | Problem              |
| --------- | ------------- | -------------------- |
| Word      | simple        | fails on new words   |
| Subword   | best balance  | slightly more tokens |
| Character | very flexible | too many tokens      |
| Byte      | universal     | complex              |

---

# Why Subword Won (IMPORTANT)

This is the **core concept**.

Subword tokenization gives:

```id="win"
Handles unknown words
Keeps vocabulary small
Efficient token usage
Works across languages
```

That’s why **modern LLMs use subword tokenization**.

---

# Real Insight (Very Important for You)

Now your earlier question becomes clear:

> Why "Kubernetes" is sometimes split?

Answer:

```id="answer"
Because tokenizer vocabulary does not always contain full word
```

So it breaks into:

```text
Kuber + net + es
```

---

# Hands-On Mini Lab

## Objective

See how tokenization differs

---

## Code

```python id="lab1"
from transformers import AutoTokenizer

tokenizer = AutoTokenizer.from_pretrained("gpt2")

text = "Kubernetes is amazing"
tokens = tokenizer.tokenize(text)

print(tokens)
```

---

## Expected Output (Example)

```text id="out1"
['K', 'uber', 'net', 'es', ' is', ' amazing']
```

---

## Learning

* tokens are not always words
* subword splitting happens
* space handling is special

---

# Real System Insight (Important for DevOps Thinking)

Tokenization affects:

### 1. Cost

```id="cost"
More tokens → more API cost
```

---

### 2. Context Window

```id="context"
More tokens → less space for input
```

---

### 3. Performance

Better tokenization → better model accuracy

---

# One minute Explaination

> Language models don’t always split text into full words. Instead, they break words into smaller meaningful parts called subwords. This allows them to handle new words, reduce vocabulary size, and generalize better. That’s why modern models like GPT use subword tokenization instead of word-level tokenization.

---

# Where We Are Now

| Topic                 | Status |
| --------------------- | ------ |
| Tokenization basics   | Done      |
| Hands-on tokenization | Done      |
| Tokenizer internals   | Done      |
| Embeddings            | Done      |
| Types of tokenization | Done      |

---

# Next Step

Now comes a **very powerful and practical section**:

# Comparing Real Tokenizers (BERT vs GPT vs GPT-4 vs Code Models)

This will show:

```id="next2"
why different models behave differently
why code models understand indentation
why GPT is better than older models
```

