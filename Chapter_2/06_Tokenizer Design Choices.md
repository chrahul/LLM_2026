

# Tokenizer Design Choices

*(Why different models behave differently)*

---

# Big Idea First

Let’s start with a powerful statement:

```id="bigidea2"
A tokenizer is not random.
It is carefully designed based on 3 key decisions.
```

---

# The 3 Core Design Decisions

```id="design3"
1. Tokenization Algorithm
2. Tokenizer Parameters
3. Training Data (Domain)
```

These three decide:

```id="impact_all"
✔ How text is split
✔ What tokens exist
✔ What the model understands easily
```

---

# 1️ Tokenization Algorithm

## Concept Explanation

This is the **method used to split text into tokens**.

---

## Common Algorithms

### Byte Pair Encoding (BPE)

Used by:

* GPT-2
* GPT-4
* LLaMA

 Builds tokens based on **frequent patterns**

Example:

```text id="bpe1"
apologizing → apolog + izing
```

---

### WordPiece

Used by:

* BERT

Example:

```text id="wp1"
capitalization → capital + ##ization
```

---

### SentencePiece

Used by:

* Flan-T5

 Works without relying on spaces
 Good for multilingual systems

---

## Why Algorithm Matters

```id="algo_impact"
Different algorithm → different token splits → different learning patterns
```

---

# 2️ Tokenizer Parameters

Now comes the **most practical part**.

---

## 2.1 Vocabulary Size

## Concept

```id="vocab_concept"
How many tokens exist in tokenizer?
```

Typical sizes:

```id="vocab_sizes"
30K (BERT)
50K (GPT-2)
100K+ (GPT-4)
```

---

## Trade-Off

### Small Vocabulary

```id="small_vocab"
✔ smaller model
❌ more token splitting
```

Example:

```text id="small_vocab_ex"
Kubernetes → Kuber + net + es
```

---

### Large Vocabulary

```id="large_vocab"
✔ fewer tokens per sentence
✔ faster processing
❌ larger memory
```

Example:

```text id="large_vocab_ex"
Kubernetes → single token
```

---

## Real Insight

```id="real1"
Better tokenizer → fewer tokens → lower cost + better performance
```

---

## 2.2 Special Tokens

These are **non-text tokens with special meaning**.

---

### Common Special Tokens

```text id="sp1"
<s>           → start of text
</s>          → end of text
<pad>         → padding
<unk>         → unknown word
```

---

### Task-Specific Tokens

#### BERT

```text id="sp2"
[CLS] → classification
[SEP] → separator
[MASK] → training
```

---

#### Chat Models

Example:

* Phi-3

```text id="sp3"
<|user|>
<|assistant|>
<|system|>
```

---

## Why Special Tokens Matter

```id="sp_impact"
They tell the model HOW to behave
```

Example:

```text id="sp4"
<|user|> Explain Kubernetes
<|assistant|>
```

 Model knows:

* input is from user
* response should come next

---

## 2.3 Capitalization Handling

## Design Choice

Should tokenizer:

```id="cap1"
Lowercase everything?
OR
Preserve case?
```

---

### Example

```text id="cap2"
Apple vs apple
```

* Apple → company
* apple → fruit

---

## Trade-Off

```id="cap_trade"
Lowercase:
✔ simpler model
❌ loses meaning

Preserve case:
✔ more information
❌ larger vocabulary
```

---

# 3️ Training Data (Most Important)

## Concept Explanation

Even if everything is same:

```id="same_config"
Same algorithm
Same vocab size
Same parameters
```

 Tokenizer behavior changes based on:

```id="data"
TRAINING DATA
```

---

## Example 1 — English Model

```id="data1"
Focus: natural language
```

Tokenizer learns:

* words
* grammar
* common phrases

---

## Example 2 — Code Model

```id="data2"
Focus: programming
```

Tokenizer learns:

* indentation
* keywords (if, else, elif)
* symbols

---

## Example 3 — Multilingual Model

```id="data3"
Focus: multiple languages
```

Tokenizer learns:

* unicode
* cross-language patterns

---

## Real Example (Very Important)

### Python Code

Bad tokenization:

```text id="bad_code"
"    " → 4 separate tokens
```

Good tokenization:

```text id="good_code"
"    " → 1 token
```

 Why?

Because indentation is **critical in Python**

---

## Key Insight

```id="data_insight"
Tokenizer is optimized for the dataset it was trained on
```

---

# Putting It All Together

## Full Tokenizer Design Pipeline

```id="full_design"
Choose Algorithm
        ↓
Set Vocabulary Size
        ↓
Define Special Tokens
        ↓
Train on Dataset
        ↓
Final Tokenizer
```

---

# Real-World Impact

This directly affects:

---

## 1️ Model Performance

```id="perf"
Better tokenizer → better understanding → better output
```

---

## 2️ Cost

```id="cost2"
Fewer tokens → cheaper API usage
```

---

## 3️ Task Specialization

```id="task"
Code tokenizer → better coding
Science tokenizer → better research answers
```

---

# Hands-On Mini Lab

## Objective

See tokenizer behavior differences

---

## Code

```python id="lab3"
from transformers import AutoTokenizer

text = "def add(a, b): return a + b"

tokenizer = AutoTokenizer.from_pretrained("gpt2")

print(tokenizer.tokenize(text))
```

---

## Experiment

Try:

* code text
* normal English
* emojis

 Observe how tokens change

---

# How I Would Explain This to Students

> A tokenizer is designed using three main decisions: the algorithm, the vocabulary, and the training data. These decisions determine how text is broken into tokens and how efficiently a model can understand it. A well-designed tokenizer improves model performance, reduces cost, and enables specialization for tasks like coding, chat, or scientific research.

---

# Where We Are Now

| Topic                 | Status |
| --------------------- | ------ |
| Tokenization basics   | ✅      |
| Types of tokenization | ✅      |
| Tokenizer comparison  | ✅      |
| Tokenizer design      | ✅      |

---

# Next Step

Now we move to:

#  Contextualized Word Embeddings (VERY IMPORTANT)

This will answer:

```id="next4"
Why same word has different meaning in different sentences
How LLMs understand context
Why GPT is better than Word2Vec
```


