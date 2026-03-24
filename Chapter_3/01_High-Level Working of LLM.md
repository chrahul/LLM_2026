#  Let’s Start — Section 1

# High-Level Working of LLM

---

#  Concept Explanation

Now till Chapter 2 you learned:

```text
How language becomes numbers
```

Now Chapter 3 answers:

```text
How model thinks using those numbers
```

---

## Big Idea

LLM is NOT writing full paragraph at once

 It works like this:

```text
It predicts NEXT word again and again
```

---

## Flow

```text
Input → predict next token  
      → add to input  
      → predict next token  
      → repeat
```

---

## Example

You give:

```text
"The capital of France is"
```

---

Model does:

Step 1:

```text
Predict → "Paris"
```

Step 2:

```text
"The capital of France is Paris"
```

Now again:

```text
Predict → "."
```

---

#  Key Insight

```text
LLM is just a next-word prediction machine
```

---

#  Why This Matters

If you understand this, everything becomes clear:

* Why hallucination happens
* Why temperature matters
* Why prompt matters

---

Because:

```text
Model is NOT thinking  
It is predicting probabilities
```

---

#  Real World Example

Think like this:

---

## Student Writing Sentence

Teacher gives:

```text
"The sun rises in the"
```

You may thinks:

* east → 90%
* west → 5%
* sky → 5%

 You may picks "east"

---

LLM does exactly same thing

---

#  Technical Deep Dive

---

## Term: Forward Pass

Every time model predicts a token:

```text
Input → goes through neural network → output
```

This is called:

```text
Forward Pass
```

---

## Important Point

```text
One token = one forward pass
```

---

## Why slow?

Because:

```text
100 words = 100 forward passes
```

---

#  Autoregressive Model

Important term :

```text
Autoregressive
```

---

## Meaning

```text
Model uses its own output as next input
```

---

## Flow

```text
Input → Output1  
Input + Output1 → Output2  
Input + Output1 + Output2 → Output3
```

---

#  Key Takeaways

* LLM generates one token at a time
* It predicts next token, not full text
* Each token = one forward pass
* It uses previous output as next input
* This is called autoregressive model

---

# Hands-On Lab 

## Objective

See how LLM generates text step by step

---

## Code

```python
from transformers import pipeline

generator = pipeline("text-generation", model="gpt2")

output = generator("The future of AI is", max_new_tokens=10)

print(output)
```

---

## What to Observe

* Output comes gradually
* It is not pre-written
* It grows word by word

---

## Learning Outcome

```text
You will see LLM is predicting step by step
```

---

# One Minute explanation 

> LLM is not writing like a human. It is just guessing the next word again and again based on probability.


