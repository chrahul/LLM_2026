# Contextualized Word Embeddings

This is where:

```text
LLM stops being “smart calculator”
and starts becoming “intelligent system”
```

---

#  Concept Explanation

Let’s start with a problem.

---

##  Problem with Previous Embeddings

Earlier (Word2Vec / basic embeddings):

```text
"bank" → same vector everywhere
```

---

### Example

```text
Sentence 1: I deposited money in the bank  
Sentence 2: I sat near the river bank
```

 Old models:

```text
bank = same meaning 
```

---

##  What LLM Does Differently

LLM says:

```text
Meaning depends on CONTEXT
```

---

##  Definition

```text
Contextual Embedding = Word meaning changes based on surrounding words
```

---

# Simple Analogy 

Think like this:

---

### Static Embedding (Old)

```text
Rahul = same personality everywhere 
```

---

### Contextual Embedding (LLM)

```text
Rahul (Cloud Architect) → techno mode  
Rahul (trading) → trader mode  
Rahul (family trip) → relaxed mode
```

Same person, different context = different behavior

---

# How It Works (Step-by-Step Flow)

Let’s take:

```text
"I love Python"
```

---

## Step 1 — Tokenization

```text
["I", "love", "Python"]
```

---

## Step 2 — Token IDs

```text
[101, 2023, 5098]
```

---

## Step 3 — Static Embeddings

```text
I → vector A  
love → vector B  
Python → vector C  
```

---

## Step 4 — Transformer Processing 

Now model does:

```text
Each word looks at ALL other words
```

---

### Attention in Action

```text
Python looks at:
→ "I"
→ "love"
```

---

## Step 5 — Contextual Embedding

```text
Python → new vector (based on context)
```

---

## Important

```text
Embedding BEFORE transformer = static  
Embedding AFTER transformer = contextual
```

---

# Why This Is Powerful

---

## 1️ Same Word, Different Meaning

```text
"Apple is launching new iPhone" → company  
"I ate an apple" → fruit
```

 Different embeddings

---

## 2️ Sentence Understanding

```text
"The animal didn’t cross the road because it was tired"
```

 "it" → refers to animal (model understands)

---

## 3️ Relationships Between Words

```text
king → male ruler  
queen → female ruler  
```

 Model understands relation

---

#  Technical Deep Dive

---

## Output of Transformer

From previous lab:

```python
output.shape → [1, 4, 384]
```

---

### Meaning

```text
Each token → 384-dim vector  
Each vector is CONTEXTUAL
```

---

## Key Point

```text
Each word has DIFFERENT embedding depending on sentence
```

---

#  Hands-On Lab

## Objective

See contextual embeddings in action

---

## Code

```python
from transformers import AutoTokenizer, AutoModel
import torch

tokenizer = AutoTokenizer.from_pretrained("microsoft/deberta-v3-xsmall")
model = AutoModel.from_pretrained("microsoft/deberta-v3-xsmall")

sentence1 = "I went to the bank to deposit money"
sentence2 = "I sat by the river bank"

inputs1 = tokenizer(sentence1, return_tensors="pt")
inputs2 = tokenizer(sentence2, return_tensors="pt")

output1 = model(**inputs1).last_hidden_state
output2 = model(**inputs2).last_hidden_state

print(output1.shape)
print(output2.shape)
```

---

## What to Observe

* Find token index of "bank"
* Compare vectors

 They will be **different**

---

#  Key Insight

```text
Same word → different vectors → different meaning
```

---

#  Where This Is Used

---

## 1️ Named Entity Recognition (NER)

```text
Apple → company OR fruit
```

---

## 2️ Summarization

Model understands:

```text
important vs less important words
```

---

## 3️ ChatGPT / LLMs

```text
Understands intent, tone, context
```

---

## 4️ Image Generation (Important)

Systems like:

* DALL·E
* Midjourney

Use contextual embeddings to understand prompts

---

# Key Takeaways

* Static embeddings = same meaning everywhere 
* Contextual embeddings = meaning depends on sentence 
* Transformer updates embeddings using attention
* This is the core intelligence of LLMs

---

# One Minute Explanation

> Earlier models gave the same meaning to a word everywhere. But LLMs are smarter. They understand that meaning depends on context. So they dynamically update the word’s meaning based on the full sentence. That’s why ChatGPT feels intelligent.

---

# Next Step

Now we move to:

# Section 10 — Sentence / Text Embeddings

This will answer:

```text
How entire paragraph → single vector
How semantic search works
How RAG works
```

