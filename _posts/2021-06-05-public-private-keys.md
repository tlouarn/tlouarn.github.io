---
title: Public and private keys
subtitle: Meet Alice and Bob in their prime.
tags: algorithm
---

# The problem

Alice wants to send a confidential document to Bob and wants to make sure that only Bob can open it. Alice does not have access to a private courier and needs to use a public communication channel.

Alice could place the document in a box, lock it with a padlock, and send it to Bob. This way, anyone intercepting the box wouldn't be able to open it. But neither would Bob, since he doesn't have the key. So that does not work.

Alice could send the locked box as a first parcel and the key in a separate parcel. The thief would need to intercept both parcels in order to open the box. Although slightly more secure, this approach is not foolproof.

Now let us equip the box with two locks and let us give a distinct padlock to each of Alice and Bob. Alice and Bob each have their own key for their own padlock. The process becomes:
* Alice places the document in the box, locks it with her padlock and sends to Bob
* Bob receives the box, places his own padlock next to Alice's and sends back to Alice
* Alice receives the box, removes her padlock, and sends back to Bob
* Bob receives the box again, this time with only his padlock, and can open it

This way, anyone intercepting the box at any time would be unable to open it.

# Brush up on prime numbers

Let's make a little detour through the number theory and remind ourselves basic definition around prime numbers.
* A **prime** number is a natural integer greater than 1 that is not the product of two smaller natural numbers. A prime number can only be divided by itself and 1. A number that is not a prime number is called a composite number.
* A **semiprime** number is the product of exactly two prime numbers. For instance, 15 is a semiprime number since it is the product of 3 and 5. 49 is also a semiprime number (product of 7 and 7). But 8 is not a semiprime number (product of 2 and 4 and 4 is not prime).
* Two distinct numbers are **coprime** if they have no other common factor than 1. In other words, their GCD (Greatest Common Divisor) is 1. A prime number is not coprime to itself. Two distinct prime numbers are always coprime. Two composite numbers can also be coprime: for instance 4 and 9 are both composite numbers but their GCD is 1.

# Modulus and congruence

Imagine a clock that shows the hours 1 to 12. If it's 10 o'clock now and you add 5 hours, what time will it be? Well, 10 + 5 = 15 but the clock only goes to 12, after which it wraps around to 3. We say that **3 and 15 are congruent modulo 12**. 

It means that:
* 15 and 3 have the same remainders when divided by 12 (that is 3)
* in other words, both 15 mod 12 and 3 mod 12 yield the same result (i.e. 3)
* 12 is a divisor of their difference (15 - 3 = 12, which is a multiple of 12)

In other words, although 15 and 3 are not equal in standard arithmetic, they kind of fall in the same group in **modular arithmetic**: they are "equivalent" within a modular system.

Formally, we write $$a$$ is congruent to $$b$$ mod $$p$$ as follows:

\\[ a \equiv b \ (mod \ p) \\]

The set of all numbers congruent to $$b \ (mod \ p)$$ can be expressed as:
\\[ \{ a = b + k \cdot p \ | \ k \in \Z \} \\]

# Fermat's little theorem

Fermat's **little theorem** states that if $$p$$ is a prime number and $$a$$ is coprime to $$p$$, then $$a^{p-1}$$ is congruent to $$1$$ modulo $$p$$. In modular arithmetic this is written as:

\\[ a^{p-1} \equiv 1 \ (mod \ p) \\]

This theorem is only guaranteed to work when $$p$$ is a prime number. If $$p$$ is a composite number, the theorem may still hold but is not guaranteed (cf Carmichael numbers). 

Let's try with $$p = 7$$ and $$a = 4$$: 

\\[ a^{p-1} = 4^{7 - 1} = 4^6 = 4096 \\]
\\[ 4096 mod 7 = 1 (since 4096 = 585 \cdot 7 + 1) \\]


# Euler's theorem 

Euler's totient function, named **phi**, counts the positive integers up to a given $$n$$ that are coprime to $$n$$. We denote this function as:

\\[ \phi(n) \\]

In particular:
* if $$n$$ is prime, $$\phi(n) = n - 1$$ (i.e. all numbers up to $$n$$ are coprime to $$n$$ since $$n$$ is prime)
* if $$n$$ is semiprime and product of primes $$p$$ and $$q$$: $$\phi(n) = (p-1) \times (q-1)$$

Some examples:
* since $$7$$ is prime: $$\phi(7) = 7 - 1 = 6$$ (there are 6 numbers smaller than 7 that are coprime to 7 and these are 1, 2, 3, 4, 5, 6)
* since $$10$$ is semiprime and its prime factorization is $$2 \mult 5$$: $$\phi(10) = (2 - 1) \times (5 - 1) = 4$$, i.e. there are 4 numbers between 1 and 10 that are coprime with 10 and these are 1, 3, 7, 9.

Euler's theorem is a generalization of Fermat's little theorem to all numbers $$p$$ (prime or not). Instead of raising to the power of p, it raises to the power phi(p). if p is prime, then phi(p) = p-1 and we are back to Fermat's little theorem. if p is composite, the theorem holds.



Finally, Euler's theoream states that if $a$ and $n$ are coprime then $a ^ (\phi(n))

Euler's Theorem: if a and n are coprime then a ** phi(n) ≡ 1 mod n

\\[ a ^ \phi(n) \equiv 1 (mod n) \\]

Indeed, Fermat's Little Theorem is a special case since if n is a prime number then phi(n) = n - 1.


# RSA encryption

This idea is used in encryption (like RSA) to make sure messages "wrap around" in a predictable way but are hard to reverse without the key.

it is based on phi(n), which is trivial to find when you know p and q, but practically impossible to find for large values of n when you don't know p and q. it also relies on the notion of modular

Alice needs to choose two prime numbers in order to generate her public and private keys.

The magic of RSA works because ee and dd are chosen so that e⋅d≡1mod  ϕ(n)e⋅d≡1modϕ(n).
This ensures that raising C to the power d undoes the encryption, bringing back MM.

```python
# Choose two prime numbers
p = 13
q = 19

# Compute n
n = p * q

# Compute the Euler totient
phi = (p-1) * (q-1) = 12 * 18 = 216

# Choose the public exponent
# e must satisfy 1 < e < phi
# e must be coprime with phi
e = 5

# Calculate d (the private exponent)
# d is the modular multiplicative inverse of e modulo φ(n).
# we want d x e ≡ 1 mod(phi(n))
# (d * e) % φ(n) = 1
# all solutions are in the form (173 + 216k) with k an integer (negative, positive or null)
d = 173
# brute_force or 

# Keys
public_key = (247, 5)
private_key = (247, 173)

# The message to encrypt is "Hello World"
msg = "hello world"
ascii_msg = [ord(c) for c in msg]
# [104, 101, 108, 108, 111, 32, 119, 111, 114, 108, 100]

encrypted_msg = [i ** e % n for i in ascii_msg]
# [130,43,166,166,232,223,123,232,95,166,237]

decrypted_msg = [i ** d % n for i in encrypted_msg]
decrypted_msg = ''.join(chr(i) for i in decrypted_msg)
assert msg == decrypted_msg

```

The key idea is that e×d≡1(modϕ(n))e×d≡1(modϕ(n)) ensures that encryption and decryption are inverse operations. This is why raising the ciphertext CC to the power of dd (using the private key) recovers the original plaintext message MM. This key property is the foundation for RSA to work. It ensures that encryption and decryption can undo each other.

security is that even if someone knows n, it can't easily find p and q

what is hard is to compute phi(n). It is trivial if we know p and q, but not trivial if we don't know them.

How many prime numbers do we know
Prime estimate


RSA is used in secure communications, including HTTPS, digital signatures, and secure email encryption.

Quantum computing risk?

So, in RSA, the relationship e×d≡1(modϕ(n))e×d≡1(modϕ(n)) ensures that the encryption and decryption processes are inverses of each other, and the exponentiation “loops back” to the original message when we decrypt.