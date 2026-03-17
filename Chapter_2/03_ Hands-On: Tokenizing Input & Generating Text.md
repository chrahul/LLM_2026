
# Hands-On: Tokenizing Input & Generating Text

## Concept Explanation

Let’s think like a system.

When you give input:

```text
Write an email apologizing to Sarah
```

You think the model reads this text.

 **Wrong**

Actual flow:

```id="flow1"
Text
↓
Tokenizer
↓
Token IDs
↓
Model
↓
Generated Token IDs
↓
Tokenizer (decode)
↓
Final Output Text
```

---

## Step-by-Step Code Explanation

### Step 1 — Load Model & Tokenizer

```python
from transformers import AutoModelForCausalLM, AutoTokenizer
```

 This loads:

* Model (brain)
* Tokenizer (language converter)

---

```python
model = AutoModelForCausalLM.from_pretrained(
    "microsoft/Phi-3-mini-4k-instruct",
    device_map="cuda",
    torch_dtype="auto",
    trust_remote_code=True,
)
```

### What is happening here?

* downloading pretrained LLM
* loading weights into GPU
* ready for inference

---

```python
tokenizer = AutoTokenizer.from_pretrained("microsoft/Phi-3-mini-4k-instruct")
```

 This loads the tokenizer **specific to this model**

 Important rule:

```id="rule1"
Model and tokenizer must always match
```

---

### Step 2 — Define Prompt

```python
prompt = "Write an email apologizing to Sarah..."
```

---

### Step 3 — Tokenization

```python
input_ids = tokenizer(prompt, return_tensors="pt").input_ids.to("cuda")
```

## What is happening?

```id="flow2"
Text → Tokens → Token IDs
```

Example:

```id="example1"
"apologizing" → ["apolog", "izing"]
```

Then:

```id="example2"
["apolog", "izing"] → [5281, 304]
```

---

### Step 4 — Generate Output

```python
generation_output = model.generate(
  input_ids=input_ids,
  max_new_tokens=20
)
```

 Model generates **next tokens**

---

### Step 5 — Decode Output

```python
print(tokenizer.decode(generation_output[0]))
```

 Converts token IDs → human-readable text

---

## Key Insight

```id="insight1"
Model never sees text.
It only sees numbers.
```

---

# SECTION 4 — How Tokenizers Work Internally

## Concept Explanation

Tokenizer is NOT magic.

It follows **3 design decisions**:

---

## 1️ Tokenization Algorithm

Examples:

* BPE (used by GPT-2)
* WordPiece (used by BERT)
* SentencePiece

Each decides:

```id="algo1"
how to split text
```

---

## 2️ Vocabulary

Tokenizer has a fixed dictionary.

Example:

```id="vocab1"
"king" → 3215
"queen" → 4981
"apolog" → 5281
"izing" → 304
```

---

## 3️ Training Data

Tokenizer learns from dataset.

Example:

* English dataset → English tokens
* Code dataset → code-friendly tokens

---

## Internal Flow

```id="flow3"
Text
↓
Split into tokens
↓
Match tokens with vocabulary
↓
Return token IDs
```

---

## Important Observation

From your example earlier:

```id="obs1"
apologizing → apolog + izing
```

This happens because:

```id="obs2"
"apologizing" not in vocab
"apolog" and "izing" are in vocab
```

---

# SECTION 8 — Token Embeddings Inside Language Models

This is the **most important part**.

---

## Concept Explanation

After tokenization:

```id="flow4"
Token IDs → Embeddings → Transformer
```

---

## What is Embedding Layer?

It is a **huge lookup table**.

Example:

```id="matrix1"
Vocabulary Size = 50,000
Embedding Dimension = 768
```

Matrix:

```id="matrix2"
50,000 × 768
```

---

## How It Works

Example:

```id="example3"
Token ID = 5281
```

Step:

```id="step1"
Go to row 5281 in matrix
```

Output:

```id="step2"
[0.23, -0.11, 0.87, ...]
```

That is the embedding.

---

## Important Point

Before training:

```id="before"
embeddings = random numbers
```

After training:

```id="after"
embeddings = meaningful vectors
```

---

## Why This Matters

Because embeddings capture **relationships**.

Example:

```id="example4"
king ≈ queen
car ≈ truck
doctor ≈ nurse
```

---

## Architecture View

```id="flow5"
Token IDs
   ↓
Embedding Layer
   ↓
Vectors
   ↓
Transformer
```

---

## One Very Important Insight

 The embedding layer is actually part of the model weights.

So when you download a model:

```id="download"
You also download embeddings
```

---

# Mini Mental Model

```id="mental1"
Token ID = word index
Embedding = word meaning vector
```

---

# Hands-On Lab (Mini)

## Objective

Understand token → embedding flow

---

## Step 1

```python
tokens = tokenizer("Hello world")
print(tokens.input_ids)
```

---

## Step 2

```python
model = AutoModel.from_pretrained("microsoft/deberta-v3-xsmall")
output = model(**tokens)
```

---

## Step 3

```python
print(output[0].shape)
```

Expected:

```id="output1"
(1, number_of_tokens, embedding_dimension)
```

---

## What You Learn

* token IDs → embeddings
* embedding dimension
* model input structure

---

# How I Would Explain This to Students

> When you give text to an LLM, the system first breaks it into tokens using a tokenizer. Each token is mapped to a numeric ID. These IDs are then converted into vectors using the embedding layer. These vectors represent the meaning of the tokens and are passed into the transformer, which processes them and generates new tokens as output.

---

# Where We Stand Now

After this:

| Topic               | Status     |
| ------------------- | ---------- |
| Tokenization        |  complete |
| Hands-on token flow |  complete |
| Tokenizer internals |  complete |
| Embeddings          |  complete |

---

# Next Step (Very Important)

Now we move to:

#  Types of Tokenization (Word vs Subword vs Character vs Byte)

This will answer:

```id="next"
WHY tokens split words
WHY token count changes
WHY GPT behaves differently from BERT
```

 and we’ll go deep into that.
