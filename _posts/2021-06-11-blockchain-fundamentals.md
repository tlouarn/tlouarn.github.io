---
title: Blockchain Fundamentals (1)
tags: bitcoin, blockchain, hash
---

This post is part of a series where I try to explain the basic principles of the blockchain technology in English.

In this first post, I'm introducing two key concepts that will be reused throughout the series.
I try to remain high-level and do not expose all the underlying complexity of the concepts used.

# Hash functions

A hash function transforms an arbitrary input of any size into a fixed-size output called "hash value" or simply "hash". Inputs can be anything: a single word, the complete works of Victor Hugo or the high-definition photograph of a supernova. No matter the input, the hash function will spit an output of fixed length.

Several hash functions have been designed by cryptographers. A commonly used one is SHA-256 which converts the input into a series of 256 bytes, usually represented as an hexadecimal string (4 bytes per characters gives a 64-long string). How many possible outputs is that? Well, 

\\[ 2^256 ~ 1.158 * 10^77 \\]

With "only" 256 bytes, the output appears deceiptevely small. In reality, 10^77 is an unfathomably high number. For comparison, scientists generally estimate there are around 10^80 atoms in the visible universe. See my other post Big Numbers.

Let's look at an example using Python and the SHA-256 hashing function:

```python
from hashlib import sha256

text = b"Hello World"
hash_value = sha256(text).hexdigest()

print(hash_value)
>>> "a591a6d40bf420404a011733cfb7b190d62c65bf0bcda32b57b277d9ad9f146e"
```

You can run the code on your side and you will find the exact same output. The hash function is **deterministic**: the same input will always give the same output.

SHA-256 is an asymetric hash function, which means: given the input "Hello World", it can quickly compute the corresponding hash. But given a hash, it cannot recompute the original input. There is no such function as `sha256_reverse()`. We say that the hash function is **non-reversible** or **one-way**. The only way to try to find the input for a given output is to try all the possible inputs one by one, which is infinite.

Now let's slightly amend the input, for instance by adding an underscore between the two words. If you run the code again, you will realize the output is radically different. Still the same length, obviously, but a completely different sequence of characters. This is the **avalanche effect**: similar but not identital inputs should produce radically different outputs. A good hash function generates an output which is statistically indistinguishable from random.

Now if we can't predict the outputs, and if the number of possible inputs is infinite, how can we be so sure that there are no two inputs somewhere that have nothing in common except that, against all odds, they generate the same output? Simple answer: we don't. Such inputs would create a **collision**. We assume the hash functions we use are **collision-resistant** but there is no way to formally prove it.

To summarize, a hash function is:
* deterministic
* non-reversible
* collision-resistant
* avalanche
* fast

uniform distribution of hash values across the output space to avoid clustering


# Public/Private keys

Let's now look at another classic problem. 

Alice wants to send a confidential document to Bob and wants to make sure that only Bob can open it.
* If Alice sends the document in a box and does not lock it, anyone intercepting the box on the way to Bob would be able to open it and read the document. So obviously that doesn't work.
* Alice could place the document in a box, lock it with a padlock, and send it to Bob. This way, anyone intercepting the box wouldn't be able to open it. But neither would Bob, since he doesn't have the key. So that does not work.
* Alice could send the locked box as a first parcel and the key in a separate parcel. The thief would need to intercept both parcels in order to open the box, which is less likely but not foolproof (yes I'm talking to you, corporate workers still sending spreadsheet passwords in a separate email).

Now let's equip the box with two padlocks and let's give a padlock to each of Alice and Bob. Each of Alice and Bob keep their keys. Now the process becomes:
* Alice places the document in the box, locks it with her padlock and sends to Bob
* Bob receives the box, places his own padlock next to Alice's and sends back to Alice
* Alice receives the box, removes her padlock, and sends back to Bob
* Bob receives the box again, this time with only his padlock, and can open it

This way, anyone intercepting the box at any time would be unable to open it.

