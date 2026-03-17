# Chapter 2 — Tokens and Embeddings

---

# Table of Contents for This Chapter

1. Introduction to Tokens and Embeddings
2. Tokenization — How LLMs Understand Text
3. Hands-On: Tokenizing Input and Generating Text
4. How Tokenizers Work Internally
5. Types of Tokenization
6. Comparing Real LLM Tokenizers
7. Tokenizer Design Choices
8. Token Embeddings Inside Language Models
9. Contextualized Word Embeddings
10. Sentence / Text Embeddings
11. Word Embeddings Before LLMs
12. Word2Vec Algorithm
13. Embeddings for Recommendation Systems
14. Hands-On Lab — Song Recommendation Engine
15. Key Takeaways
16. How I Would Explain This to Students

---

# 1. Introduction to Tokens and Embeddings

## Concept Explanation

If you want to understand how **Large Language Models work internally**, you must understand **two fundamental ideas**:

```
Tokens
Embeddings
```

These two concepts are the **foundation of how language models think**.

A language model **cannot read text like humans do**.

Instead, it processes language in two steps:

```
Text → Tokens → Embeddings → Neural Network
```

So the pipeline looks like this:

```
Input Text
     ↓
Tokenizer
     ↓
Token IDs
     ↓
Embedding Layer
     ↓
Vectors
     ↓
Language Model
```

Think of it like this:

Humans see:

```
Hello world
```

The model sees something like:

```
[15496, 995]
```

Then converts it into **vectors of numbers**.

---

## Why This Concept Matters

If you understand tokens and embeddings, you understand:

* how LLMs read text
* why token limits exist
* why prompts cost tokens
* why embeddings power search systems
* how semantic search works

Almost **every LLM system depends on this concept**.

---

## Real-World Example

When you type into **ChatGPT**:

```
Explain Kubernetes in simple terms
```

The system performs:

```
Prompt
 ↓
Tokenization
 ↓
Token IDs
 ↓
Embeddings
 ↓
Transformer model
 ↓
Generated response
```

So **text never directly enters the model**.

---

# 2. Tokenization — How LLMs Understand Text

## Concept Explanation

A **token** is simply a **small piece of text**.

Tokens can be:

* words
* parts of words
* punctuation
* characters

Example sentence:

```
Write an email apologizing
```

Possible tokenization:

```
Write
an
email
apolog
izing
```

Notice something important:

```
apologizing → apolog + izing
```

This is called **subword tokenization**.

---

## Why This Concept Matters

Language models have **token limits**.

Example:

```
GPT-4 context window ≈ thousands of tokens
```

So longer text → more tokens → more compute.

---

# 3. Hands-On: Tokenizing Input and Generating Text

Let's run a real example.

---

## Step 1 — Load Model and Tokenizer

```python
from transformers import AutoModelForCausalLM, AutoTokenizer
```

Explanation:

| Component            | Purpose                         |
| -------------------- | ------------------------------- |
| AutoModelForCausalLM | Loads generative language model |
| AutoTokenizer        | Converts text → tokens          |

---

```python
model = AutoModelForCausalLM.from_pretrained(
    "microsoft/Phi-3-mini-4k-instruct",
    device_map="cuda",
    torch_dtype="auto",
    trust_remote_code=True,
)
```

Explanation:

* downloads the LLM
* loads it into GPU memory

---

```python
tokenizer = AutoTokenizer.from_pretrained("microsoft/Phi-3-mini-4k-instruct")
```

This loads the tokenizer associated with the model.

Important rule:

```
Every LLM must use its own tokenizer.
```

---

## Step 2 — Define Prompt

```python
prompt = "Write an email apologizing to Sarah..."
```

---

## Step 3 — Convert Prompt to Tokens

```python
input_ids = tokenizer(prompt, return_tensors="pt").input_ids.to("cuda")
```

This converts the prompt into **token IDs**.

Example:

```
tensor([1,14350,385,4876,27746,...])
```

Each number corresponds to a token.

---

## Step 4 — Generate Output

```python
generation_output = model.generate(
  input_ids=input_ids,
  max_new_tokens=20
)
```

The model generates **20 new tokens**.

---

## Step 5 — Decode Tokens

```python
print(tokenizer.decode(generation_output[0]))
```

The tokenizer converts tokens back to text.

---

# 4. How Tokenizers Work Internally

Tokenizers break text into tokens based on:

1️ Tokenization method
2️ Vocabulary design
3️ Training dataset

---

## Step-by-Step Tokenization Pipeline

```
Text
 ↓
Tokenizer
 ↓
Token IDs
 ↓
Embedding vectors
 ↓
Neural network
```

---

# 5. Types of Tokenization

There are **four main tokenization methods**.

---

## Word Tokenization

Example:

```
"I love AI"
```

Tokens:

```
I
love
AI
```

Problem:

New words cannot be represented.

---

## Subword Tokenization (Most Common)

Example:

```
apologizing → apolog + izing
```

Advantages:

* handles unknown words
* reduces vocabulary size

Used by most modern models.

---

## Character Tokenization

Example:

```
play → p l a y
```

Advantages:

* can represent any word

Problem:

* too many tokens
* slower learning

---

## Byte Tokenization

Text is split into **byte-level representation**.

Useful for:

* multilingual models
* complex characters

---

# 6. Comparing Real LLM Tokenizers

The book compares real tokenizers used in different models.

---

## BERT Tokenizer

Uses:

```
WordPiece tokenization
```

Example tokens:

```
capital ##ization
```

Special tokens:

| Token  | Meaning              |
| ------ | -------------------- |
| [CLS]  | classification token |
| [SEP]  | separator            |
| [PAD]  | padding              |
| [MASK] | masked word          |

Model:

**BERT**

---

## GPT-2 Tokenizer

Uses:

```
Byte Pair Encoding (BPE)
```

Advantages:

* preserves whitespace
* supports emojis

---

## GPT-4 Tokenizer

Improved tokenizer.

Features:

* larger vocabulary
* better whitespace handling
* optimized for code

Example:

```
elif
```

becomes a **single token**.

---

## Code Models — StarCoder

Code-focused models need different tokenizers.

Example tokens:

```
<filename>
<reponame>
```

These help models understand code repositories.

---

# 7. Tokenizer Design Choices

Three factors determine tokenizer behavior.

---

## Tokenization Algorithm

Examples:

* BPE
* WordPiece
* SentencePiece

---

## Vocabulary Size

Typical values:

```
30K
50K
100K
```

Larger vocabularies reduce token splitting.

---

## Special Tokens

Examples:

```
<s>           start of text
</s>          end of text
<pad>         padding
<unk>         unknown token
```

Chat models also use:

```
<|user|>
<|assistant|>
<|system|>
```

---

# 8. Token Embeddings Inside Language Models

After tokenization, tokens become **vectors**.

Example:

```
Token ID → embedding vector
```

Each token has a vector like:

```
[0.14, -0.02, 0.93, ...]
```

This embedding matrix is stored inside the model.

Before training:

```
embeddings = random
```

During training:

```
embeddings learn semantic meaning
```

---

# 9. Contextualized Word Embeddings

Traditional embeddings were **static**.

Example:

```
bank → same vector everywhere
```

Language models improved this.

Now embeddings are **contextualized**.

Example:

```
bank (finance) ≠ bank (river)
```

Each context produces a different vector.

---

## Code Example

```python
from transformers import AutoModel, AutoTokenizer
```

Load tokenizer.

```python
tokenizer = AutoTokenizer.from_pretrained("microsoft/deberta-base")
```

Load model.

```python
model = AutoModel.from_pretrained("microsoft/deberta-v3-xsmall")
```

Tokenize input.

```python
tokens = tokenizer('Hello world', return_tensors='pt')
```

Run model.

```python
output = model(**tokens)[0]
```

---

## Output Shape

```
torch.Size([1,4,384])
```

Meaning:

```
batch size = 1
tokens = 4
embedding dimension = 384
```

Tokens are:

```
[CLS]
Hello
world
[SEP]
```

---

# 10. Sentence / Text Embeddings

Sometimes we need **one vector for entire text**.

Example uses:

* semantic search
* clustering
* recommendations

---

## Code Example

```python
from sentence_transformers import SentenceTransformer
```

Load embedding model.

```python
model = SentenceTransformer("sentence-transformers/all-mpnet-base-v2")
```

Generate embedding.

```python
vector = model.encode("Best movie ever!")
```

Output dimension:

```
768
```

So the entire sentence becomes:

```
768-dimensional vector
```

---

# 11. Word Embeddings Before LLMs

Before transformers, we used models like:

* Word2Vec
* GloVe
* FastText

These created embeddings for words.

---

# 12. Word2Vec Algorithm

Word2Vec uses two key ideas:

### Skip-gram

Predict neighboring words.

Example:

```
machine learning is powerful
```

Training pairs:

```
(machine, learning)
(machine, is)
(machine, powerful)
```

---

### Negative Sampling

Random word pairs added as **false examples**.

Example:

```
(machine, banana)
```

Model learns:

```
related vs unrelated words
```

---

# 13. Embeddings for Recommendation Systems

Embeddings work beyond language.

Example:

Music recommendation.

Idea:

```
Song = word
Playlist = sentence
```

Then Word2Vec learns:

```
songs appearing together → similar embeddings
```

---

# Hands-On Lab — Music Recommendation

## Objective

Build a recommendation system using **Word2Vec embeddings**.

---

## Step 1 — Load Playlist Dataset

```python
import pandas as pd
```

Load playlist data.

---

## Step 2 — Train Word2Vec Model

```python
from gensim.models import Word2Vec
```

Train model:

```python
model = Word2Vec(
    playlists,
    vector_size=32,
    window=20,
    negative=50
)
```

---

## Step 3 — Recommend Songs

```python
model.wv.most_similar(positive=str(song_id))
```

Output example:

```
Metallica → Van Halen
Metallica → Judas Priest
Metallica → Guns N’ Roses
```

The model learned **genre similarity**.

---

# Key Takeaways

1️ Language models process **tokens**, not text.

2️ Tokenizers convert:

```
text → token IDs
```

3️ Tokens become **embeddings (vectors)**.

4️ Language models generate **contextual embeddings**.

5️ Sentence embeddings enable:

* semantic search
* recommendation systems
* clustering
* RAG systems

---

# If I had to explain Chapter 2 in one minute:

> A language model cannot understand text directly. First, the text is broken into small pieces called tokens. Each token is then converted into a numeric vector called an embedding. These embeddings are what the neural network actually processes. Modern language models produce contextual embeddings, meaning the same word can have different representations depending on context. These embeddings are the foundation of many AI systems like search engines, recommendation systems, and retrieval-augmented generation.

---


