---
title: Blockchain Fundamentals (1)
tags: bitcoin, blockchain
---

This post is part of a series where I try to explain the basic principles of the blockchain technology in English.

In this first post, I'm introducing two key concepts that will be reused throughout the series.
I try to remain high-level and do not expose all the underlying complexity of the concepts used.

# Hash function

A hash function transforms an arbitrary input of any size into a fixed-size value called "hash value" or simply "hash". Inputs can be anything: a single word, the complete works of Victor Hugo or the high-definition photograph of a supernova. No matter the input, the hash function will spit an output of fixed length.

Several hash functions have been designed by cryptographers. A commonly used one is SHA-256 which converts the input into a series of 256 bytes, usually represented as an hexadecimal string (4 bytes per characters gives a 64-long string).

Let's look at an example using Python and the SHA-256 hashing function:

```python
from hashlib import sha256

text = b"Hello World"
hash_value = sha256(text).hexdigest()

print(hash_value)
```

The hash value of the string "Hello World" is `a591a6d40bf420404a011733cfb7b190d62c65bf0bcda32b57b277d9ad9f146e`.

SHA-256 is an asymetric hash function, which means: given the input "Hello World", it can quickly compute the corresponding hash. But given a hash, it cannot recompute the original input. 

Let's look at the hash value of a slightly different string where I've added an underscore between the two words:

```python
from hashlib import sha256

text = b"Hello_World"
hash_value = sha256(text).hexdigest()

print(hash_value)
```

The hash value is now `345837a29e408cf69771db1188157664da5d9e6c1182b467f0100864761019b7`.

As you can see, it is completely different.




issues:
* collisons
* quantum


SHA-256 is widely used.

# Public/Private keys

Let's now look at another classic problem. 

Alice wants to send a confidential document to Bob and wants to make sure that only Bob can open it.
* If Alice sends the document in a box and does not lock it, anyone intercepting the box on the way to Bob would be able to open it and read the document. So obviously that doesn't work.
* Alice could place the document in a box, lock it with a padlock, and send it to Bob. This way, anyone intercepting the box wouldn't be able to open it. But neither would Bob, since he doesn't have the key. So that does not work.
* Alice could send the locked box as a first parcel and the key in a separate parcel. The thief would need to intercept both parcels in order to open the box, which is less likely but not foolproof (yes I'm talking to you, corporate workers still sending spreadsheet passwords in a separate email).

Now let's equip the box with two padlocks and let's give a padlock to each of Alice and Bob. Each of Alice and Bob keep their keys. Now the process becomes:
* Alice places the document in the box, locks it with her padlock and sends to Bob.
* Bob receives the box, places his own padlock on top of Alice's, and sends back to Alice.
* Alice receives the box, removes her padlock, and sends back to Bob.
* Bob receives the box again, this time with only his padlock, and can open it.

Anyone intercepting the box would have had to open either one or two padlocks, which is impossible.

