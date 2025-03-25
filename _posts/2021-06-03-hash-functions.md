---
title: Hash Functions
subtitle: Some definitions and a quick peek at SHA-256
tags: algorithm
---

A **hash function** transforms an arbitrary input of any size into a unique fixed-size output called "hash value" or simply "hash". The input can be anything: a single letter, the complete works of Victor Hugo or the high-definition photograph of a supernova. No matter the input, the hash function will always spit an output of fixed length.

A commonly used hash function is **SHA-256**, which converts the input into a series of 256 bits, traditionally represented as a string of 64 hexadecimal characters (4 bits per character). How many possible outputs is that?

\\[ 2^{256} \approx 1.158 \cdot 10^{77} \\]

With "only" 256 bits (32 bytes), the output appears deceptively small. In reality, $$10^{77}$$ is an unfathomably large number. For comparison, scientists generally estimate there are around $$10^{80}$$ atoms in the visible universe.

Let's look at SHA-256 in action using Python and the `hashlib` standard library:

```python
from hashlib import sha256

text = b"Hello World"
hash_value = sha256(text).hexdigest()

print(hash_value)
>>> "a591a6d40bf420404a011733cfb7b190d62c65bf0bcda32b57b277d9ad9f146e"
```

You can run the code on your side and you will find the exact same output. We say that the hash function is **deterministic** since the same input will always give the same output.

SHA-256 is an **asymmetric** hash function. Given the input "Hello World", it can quickly compute the corresponding hash. But given a hash, it cannot recompute the original input. There is no such function as `sha256_reverse()`. We say that the hash function is **non-reversible** or **one-way**. The only way to determine the input for a given output is to try all possible inputs one by one, which is infeasible.

If you try to slightly amend the input, for instance by changing only one letter, and run the code again, you will realize the output is radically different. Still the same length, obviously, but a completely different sequence of characters. This is the **avalanche effect**: similar but not identical inputs should produce radically different outputs. A good hash function generates outputs which are statistically indistinguishable from random.

If one cannot predict the output, and if the number of possible inputs is infinite, how can we be so sure that there are no two inputs somewhere that have nothing in common except that, against all odds, they generate the same output? The answer is simple: we don't. Such inputs would create a **collision**. We assume the hash functions we use are **collision-resistant** but there is no way to formally prove this property. That is why a good hash function should have a uniform distribution of hash values across the output space to avoid clustering.

Finally, a good hash function should be computationally efficient. For instance, the above Python code runs in well under a second on a standard computer despite Python's slower performance compared to some other languages.

To summarize, a hash function is **deterministic**, **non-reversible**, **collision-resistant**, **unpredictable** and **fast**.


