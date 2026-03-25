#  Section 3 — Decoding

# How Model Chooses the Next Word

---

#  Concept Explanation

Till now you know:

* model calculates probabilities
* example:

```text
managing → 40%  
deploying → 30%  
scaling → 20%
```

---

## Now the real question:

```text
How does model PICK one word?
```

---

 This process is called:

```text
DECODING
```

---

#  Simple Meaning

```text
Decoding = selecting next token from probabilities
```

---

#  Why Decoding Matters

Same input:

```text
"Kubernetes helps in"
```

---

Can give different outputs:

```text
1. managing containers  
2. scaling applications  
3. automating deployments  
```

---

 Why?

```text
Because of decoding strategy
```

---

#  Types of Decoding (Important)

We will cover 3 main ones:

1. Greedy
2. Sampling
3. Temperature

---

#  1. Greedy Decoding (Default Simple)

---

## How it works

```text
Pick highest probability always
```

---

## Example

```text
managing → 40%  
deploying → 30%  
scaling → 20%
```

 Output:

```text
managing
```

---

## Problem

```text
Too predictable  
Less creative
```

---

## DevOps Analogy

Always choosing:

```text
kubectl apply
```

even when better options exist

---

#  2. Sampling (More Realistic)

---

## How it works

 Instead of picking highest, choose randomly based on probability

---

## Example

```text
managing → 40%  
deploying → 30%  
scaling → 20%
```

---

 Possible outputs:

* managing (most likely)
* deploying (sometimes)
* scaling (rarely)

---

## Benefit

```text
More natural, more variety
```

---

#  3. Temperature (CONTROL KNOB )

---

## What is temperature?

```text
Controls randomness
```

---

## Values

---

### Low temperature (0.1)

```text
Very safe  
Almost greedy
```

---

### Medium (0.7)

```text
Balanced (best)
```

---

### High (1.5)

```text
Very creative  
Sometimes nonsense
```

---

## Example

Prompt:

```text
"Kubernetes is used for"
```

---

### Temp = 0.2

```text
managing containers
```

---

### Temp = 1.2

```text
orchestrating dynamic cloud ecosystems across distributed nodes
```

---

#  Key Insight

```text
Temperature = creativity control
```

---

#  How This Works Internally

---

## Step 1

Model gives probabilities

---

## Step 2

Temperature modifies probabilities

---

## Step 3

Sampling picks token

---

 That’s decoding

---

#  Hands-On Lab (VERY IMPORTANT)

---

## Objective

See decoding difference

---

## Code

```python
from transformers import pipeline

generator = pipeline("text-generation", model="gpt2")

prompt = "Kubernetes is used for"

print("\nLow Temperature:")
print(generator(prompt, max_new_tokens=15, temperature=0.2))

print("\nMedium Temperature:")
print(generator(prompt, max_new_tokens=15, temperature=0.7))

print("\nHigh Temperature:")
print(generator(prompt, max_new_tokens=15, temperature=1.2))
```

---

## What You Will Observe

* low → predictable
* medium → balanced
* high → creative

---

#  DevOps Insight

---

## Use case mapping

---

### Documentation / Learning

```text
temperature = low
```

---

### Blog writing / explanation

```text
temperature = medium
```

---

### Creative content

```text
temperature = high
```

---

#  Bonus (Important for Interviews)

---

## Other decoding methods

* Top-K
* Top-P (nucleus sampling)

---

 We will cover later if needed

---

#  One Minute Explanation

> LLM doesn’t just pick the next word blindly. It calculates probabilities and then uses a strategy like greedy or sampling to decide which word to choose.

```text
Decoding decides how the model selects the next word from probabilities
```


