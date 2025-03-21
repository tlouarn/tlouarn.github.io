---
title: Hash Functions
subtitle: Some definitions and a quick peek at SHA-256
tags: algorithm
---

A hash function transforms an arbitrary input of any size into a fixed-size output called "hash value" or simply "hash". 

Inputs can be anything: a single word, the complete works of Victor Hugo or the high-definition photograph of a supernova. No matter the input, the hash function will always spit an output of fixed length.

Several hash functions have been designed by cryptographers. A commonly used one is SHA-256 which converts the input into a series of 256 bytes, usually represented as an hexadecimal string (4 bytes per characters gives a 64-long string). How many possible outputs is that? Well, 

\\[ 2^{256} \approx 1.158 \cdot 10^{77} \\]

With "only" 256 bytes, the output appears deceiptevely small. In reality, $$10^{77}$$ is an unfathomably high number. For comparison, scientists generally estimate there are around $$10^{80}$$ atoms in the visible universe.

Let's look at an example using Python and the SHA-256 hashing function:

```python
from hashlib import sha256

text = b"Hello World"
hash_value = sha256(text).hexdigest()

print(hash_value)
>>> "a591a6d40bf420404a011733cfb7b190d62c65bf0bcda32b57b277d9ad9f146e"
```

* You can run the code on your side and you will find the exact same output. The hash function is **deterministic**: the same input will always give the same output.

* SHA-256 is an asymetric hash function, which means: given the input "Hello World", it can quickly compute the corresponding hash. But given a hash, it cannot recompute the original input. There is no such function as `sha256_reverse()`. We say that the hash function is **non-reversible** or **one-way**. The only way to try to find the input for a given output is to try all the possible inputs one by one, which is infinite.

* Now let's slightly amend the input, for instance by changing only one letter. If you run the code again, you will realize the output is radically different. Still the same length, obviously, but a completely different sequence of characters. This is the **avalanche effect**: similar but not identital inputs should produce radically different outputs. A good hash function generates an output which is statistically indistinguishable from random.

* If we can't predict the outputs, and if the number of possible inputs is infinite, how can we be so sure that there are no two inputs somewhere that have nothing in common except that, against all odds, they generate the same output? Simple answer: we don't. Such inputs would create a **collision**. We assume the hash functions we use are **collision-resistant** but there is no way to formally prove it. Furthermore, a good hash function should have a uniform distribution of hash values across the output space to avoid clustering.

* A good hash function should be computationally-efficient: we want to be able to compute the output fast.


To summarize, a hash function is:
* deterministic
* non-reversible
* collision-resistant
* avalanche
* fast

# Usage

Hash functions can be used.

To check the validity of a file with a **checksum**. Acts like the verification digits at the end of a credit card number. If you have a large file to download, how to ensure that all the packets have correctly been received? The server computes a checksum of the file, then sends the file and the checkum. The client receives the file bit by bit (a big file with risk of data loss), then computes the checksum (a 256 bytes piece of data ie low risk of data loss) and ensures both checksums match.

In cryptography, in the context of proof of work.

