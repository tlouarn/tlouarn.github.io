---
title: Public and Private Keys
subtitle: Meet Alice and Bob in their prime.
tags: algorithm
---

Let's now look at a classic problem in computer science: Alice wants to send a confidential document to Bob and wants to make sure that only Bob can open it.

* If Alice sends the document in a box and does not lock it, anyone intercepting the box on the way to Bob would be able to open it and read the document. So obviously that doesn't work.
* Alice could place the document in a box, lock it with a padlock, and send it to Bob. This way, anyone intercepting the box wouldn't be able to open it. But neither would Bob, since he doesn't have the key. So that does not work.
* Alice could send the locked box as a first parcel and the key in a separate parcel. The thief would need to intercept both parcels in order to open the box, which is less likely but not foolproof (yes I'm talking to you, corporate workers still sending spreadsheet passwords in a separate email).

Now let's equip the box with two padlocks and let's give a padlock to each of Alice and Bob. Each of Alice and Bob keep their keys. Now the process becomes:
* Alice places the document in the box, locks it with her padlock and sends to Bob
* Bob receives the box, places his own padlock next to Alice's and sends back to Alice
* Alice receives the box, removes her padlock, and sends back to Bob
* Bob receives the box again, this time with only his padlock, and can open it

This way, anyone intercepting the box at any time would be unable to open it.

# Euler's Totient Function

Let's make a little detour by the number theory.

A **prime** number is an integer greater than 1 that cannot be exactly divided by any whole number other than itself and 1. The first prime numbers are 2, 3, 5, 7, 11 etc.

A **semiprime** number is the product of exactly two prime numbers, which may or may not be the same.
* 15 is a semiprime (product of 3 * 5)
* 49 is a semiprime number (product of 7 x 7)
* 8 is not a semiprime number (product of 4 x 2 and 4 is not prime)

Two numbers are **coprime** if they have no other common factor than 1. They don't need to be prime numbers themselves. For instance: 3 and 10 are coprime (3 is prime and there is no integer i such that 3 * i = 10). But 5 and 20 are not coprime since 5 * 4 = 20.

Consider a 12-hour clock. Both 3AM and 3PM have the same position. We say that 15 is congruent to 3 modulo 12, meaning that:
* both 15 mod 12 and 3 mod 12 yield the same result (3) -> they have the same remainders when divided by 12
* 12 is a divisor of their difference (15 - 3 = 12, which is a multiple of 12)

15 and 3 are not equal in standard arithmetic but fall in the same group in modular arithmetic -> they are equivalent within a modular system. 

Fermat's **little theorem** states that if p is a prime number, then for any integer a, the number a ** p - a is a multiple of p. In modular arithmetic this is written as:

\\[ a^p \equiv a (mod p) \\]

In the case where a is coprime to p, the statement becomes:

\\[ a^(p-1) \equiv 1 (mod p) \\]

Euler's totient function counts the positive integers up to a given n that are coprime with n. We denote this function as:

\\[ \phi(n) \\]

* if n is prime, phi(n) = n - 1 (i.e. all numbers up to n are coprime with n since n is prime)
* if n is semiprime and product of primes p and q, phi(n) = (p-1) * (q-1)

Some examples:
* phi(7): 7 is prime, therefore phi(7) = 6
* phi(10): the prime factorization of 10 is 2 * 5, therefore phi(10) = (2 - 1) * (5 - 1) = 4 and the totative numbers are 1, 3, 7, 9

**Congruence** is defined as: a≡b(modn)a≡b(modn) means that a and b have the same remainder when divided by nn. In other words, a and b are congruent modulo n. The two numbers are "equivalent" in some modular system.

Euler's Theorem: if a and n are coprime then a ** phi(n) ≡ 1 mod n

\\[ a ^ \phi(n) \equiv 1 (mod n) \\]

Indeed, Fermat's Little Theorem is a special case since if n is a prime number then phi(n) = n - 1.


# RSA encryption

Alice needs to choose two prime numbers in order to generate her public and private keys.

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