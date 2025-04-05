---
title: RSA encryption explained
subtitle: The mathematics behind the classic public-key encryption algorithm.
tags: algorithm
---

* 
{:toc}

# Prime numbers

A **prime** number is a natural integer greater than 1 that is not the product of two smaller natural numbers. A prime number can only be divided by itself and 1. A number that is not a prime number is called a composite number.

A **semiprime** number is the product of exactly two prime numbers. For instance, 15 is a semiprime number since it is the product of 3 and 5. 49 is also a semiprime number since it is the product of 7 and 7. But 8 is not a semiprime number since 8 is the product of 2 and 4.

Two distinct numbers are **coprime** if they have no other common factor than 1. In other words, their GCD (Greatest Common Divisor) is 1. Two distinct prime numbers are always coprime. Two composite numbers can also be coprime (for instance 4 and 9 are both composite numbers but their GCD is 1). But a prime number cannot be coprime to itself.

# Modulus and congruence

Let us imagine a clock that shows the hours 1 to 12. If it is 10 o'clock now and one adds 5 hours, what time will it be? Well, 10 + 5 = 15 but the clock only goes to 12, after which it wraps around to 3. We see that 15 and 3 are kind of the same thing but not really. We say that **3 and 15 are congruent modulo 12**. It means that  15 and 3 have the same remainder when divided by 12 - which is 3. It also means that 12 is a divisor of their difference (15 - 3 = 12, which is a multiple of 12).

In other words, although 15 and 3 are not equal in standard arithmetic, they are "equivalent" in **modular arithmetic**. Formally, we write $$a$$ is congruent to $$b$$ modulo $$p$$ as follows:

\\[ a \equiv b \ (mod \ p) \\]

The set of all numbers congruent to $$b \ (mod \ p)$$ is $$\lbrace b + k \cdot p \ \vert \ k \ in \ \Z \rbrace$$.

# Modular exponentiation

**Modular exponentiation**, denoted as $$a^e \ mod \ n$$ involves raising a base $$a$$ to an exponent $$e$$ then taking the remainder when divided by a modulus $$n$$. Unlike traditional exponentiation which is invertible with logarithms, modular exponentiation introduces a cyclic and non-continuous structure which makes inversion impractical. In our example, solving for $$e$$ is computationally hard due to the discrete logarithm problem. From the original paper:

> Although this problem of "computing eth roots modulo n without factoring n" is not a well-known difficult problem like factoring, we feel reasonably confident that it is computationally intractable.

# Fermat's little theorem

Fermat's **little theorem** states that if $$p$$ is a prime number and $$a$$ is an integer coprime to $$p$$, then $$a^{p-1}$$ is congruent to $$1$$ modulo $$p$$. In modular arithmetic this is written as:

\\[ a^{p-1} \equiv 1 \ (mod \ p) \\]

This theorem is only guaranteed to work when $$p$$ is a prime number. If $$p$$ is a composite number, the theorem may still hold but is not guaranteed (cf Carmichael numbers). 

Let us try with $$p = 7$$ and $$a = 4$$: $$ 4^{7 - 1} = 4096$$ and $$4096 \ mod \ 7 = 1$$.

Therefore, for every integer $$a < p$$, there is a **modular multiplicative inverse** $$x$$ such that $$a \cdot x \equiv 1 \ (mod \ p)$$. In our example above, $$x = 2$$ since $$4 \cdot 2 \equiv 1 \ (mod \ 7)$$. This will be important later on.

# Euler's theorem 

Euler's totient function, named **phi**, counts the positive integers up to a given integer $$n$$ that are coprime to $$n$$. We denote this function $$\phi(n)$$.

In particular:
* if $$n$$ is prime then $$\phi(n) = n - 1$$ (i.e. all numbers up to $$n$$ are coprime to $$n$$ since $$n$$ is prime)
* if $$n$$ is semiprime and the product of prime numbers $$p$$ and $$q$$ then $$\phi(n) = (p-1) \times (q-1)$$

Let us look at some examples:
* since $$7$$ is prime: $$\phi(7) = 7 - 1 = 6$$
* since $$10$$ is semiprime and its prime factorization is $$2 \cdot 5$$: $$\phi(10) = (2 - 1) \times (5 - 1) = 4$$

**Euler's theorem** is a generalization of Fermat's little theorem to all numbers $$p$$ (prime or not). It states that if $$a$$ and $$n$$ are coprime then $$a ^ {\phi(n)}$$ is congruent to 1 modulo $$n$$:

\\[ a ^ {\phi(n)} \equiv 1 \ (mod \ n) \\]

In the case where $$n$$ is a prime number, we know that $$\phi(n) = n-1$$ and we are back to Fermat's little theorem.

# RSA encryption

Let's now come back to our encryption problem. RSA encryption involves a **public key** used to cipher a message **M** into **C** and a **private key** used to decipher **C** back into **M**.

In order to come up with the keys, Bob chooses two distinct prime numbers $$p$$ and $$q$$ and computes $$n = p \cdot q$$. Since both $$p$$ and $$q$$ are prime, $$n$$ is semiprime so $$\phi = (p-1) * (q-1)$$ which means there are $$(p-1) * (q-1)$$ totients $$e < n$$ that are coprime to $$n$$. Bob chooses one totient $$e$$. The totient $$e$$ has to be coprime to $$n$$ so that there is a modular multiplicative invrse. Bob computes the modular multiplicative inverse $$d$$ so that $$e \cdot d \equiv 1 \ (mod \ n)$$. Bob then broadcasts his public key $$(e, n)$$ and keeps his private key $$(d, n)$$ to himself.

Alice wants to encrypt a message $$M$$ made of one number. To do so, Alice computes the encrypted message $$C = M ^ e \ (mod \ n)$$ and sends it to Bob. Bob receives $$C$$ and computes $$M^\prime = C ^ d \ (mod \ n)$$ which is equal to $$M$$. Both encryption and decryption involve raising a number by a specific exponent so that the two operations cancel each other.

Let's see why that works:

\\[ M^\prime = C ^ d \ (mod \ n) \\]

Replacing $$C$$ by $$M ^ e$$ we get:

\\[ M^\prime = M ^ {e \cdot d} \ (mod \ n) \\]

We know that:

\\[e \cdot d \equiv 1 \ (mod \ \phi(n)) \\]

Which means:

\\[e \cdot d = k \cdot \phi(n) + 1 \\]

So we can rewrite:

\\[M ^ {e \cdot d} = M ^ {k \cdot \phi(n) + 1} = M ^ {k \cdot \phi(n)} \cdot M\\]

And therefore:

\\[ M^\prime = M ^ {k \cdot \phi(n)} \cdot M \ (mod \ n) \\]

By Euler's Theorem:

\\[M ^ {k \cdot \phi(n)} \equiv 1 \ (mod \ n)\\]

Therefore:

\\[M ^\prime = M ^ {k \cdot \phi(n)} \cdot M \ (mod \ n)\\]

We finally get:

\\[M^\prime = 1 \cdot M \equiv M \ (mod \ n) \\]

# A simple example

Let us now look at a basic example using small prime numbers.

```python
# Choose two distinct prime numbers
p = 13
q = 19

# Compute n
n = p * q  # n = 247

# Compute the Euler totient
phi = (p-1) * (q-1) = 12 * 18  # phi = 216

# Choose the public exponent
# e must satisfy 1 < e < phi
# e must be coprime with phi
e = 5

# Calculate d (the private exponent)
# d is the modular multiplicative inverse of e modulo φ(n).
# all solutions are in the form (173 + 216k) with k an integer (negative, positive or null)
d = 173

# Keys
public_key = (247, 5)
private_key = (247, 173)

# The message to encrypt is "Hello World"
msg = "hello world"
ascii_msg = [ord(c) for c in msg]
# [104, 101, 108, 108, 111, 32, 119, 111, 114, 108, 100]

encrypted_msg = [i ** e % n for i in ascii_msg]
# [130, 43, 166, 166, 232, 223, 123, 232, 95, 166, 237]

decrypted_msg = [i ** d % n for i in encrypted_msg]
decrypted_msg = ''.join(chr(i) for i in decrypted_msg)
assert msg == decrypted_msg
```

The key ideas are that encryption is a one-way function (it is infeasible to compute the $$e-th$$ modular root of $$C$$ when the numbers are large) and that encryption and decryption are inverse operations. In order to find $$d$$, an attacker would need to know $$\phi(n)$$, which means knowing $$p$$ and $$q$$ from $$n$$, which is a prime factorization problem. For very large prime numbers (hundreds of digits), this is not feasible.

RSA encryption is widely used in securing digital communications, such as in SSL/TLS protocols for web browsing and in digital signatures for authentication.