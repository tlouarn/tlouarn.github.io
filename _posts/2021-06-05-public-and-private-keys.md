---
title: Public and private keys
subtitle: Meet Alice and Bob in their prime.
tags: algorithm
---

# The problem

Alice wants to send a confidential document to Bob but she does not have access to a private courier and needs to use a public communication channel.

Alice could place the document in a box, lock it with a padlock, and send it to Bob. This way, anyone intercepting the box wouldn't be able to open it. But neither would Bob, since he doesn't have the key. So that does not work.

Alice could send the locked box in a parcel and the key in another. A thief would need to intercept both parcels in order to open the box. Although slightly more secure, this approach is still not good enough.

Now let us assume Bob also has a padlock. The solution becomes:
* Bob sends his padlock to Alice (but keeps the key)
* Alice places the document in the box and locks it with Bob's padlock
* Bob receives the box and can unlock it.

In this example, Bob's padlock is called a **public key** and Bob's key is called a **private key**. And we are able to replicate the same mechanism in the software world using properties of prime numbers.

# A very special clock

Let us assume that the message Alice wants to send to Bob boils down to one number. She needs to encrypt it and transmit it to Bob in such a way that only Bob can decipher it.

Let us now imagine a very special clock where the number of hours can be arbitrarily set. This clock has only one hand and can only move forward from hour to hour (no backward move is possible). 

In this example, we will assume that the clock has 33 hours, Bob's public key is **(7, 33)** and Bob's secret key is **(3, 33)**. Remember, anyone can have access to Bob's public key but only Bob himself knows the private key. 

In order to encrypt the number 4, Alice does the following operation: starting at number $$4$$, she turns the hand by a fixed number of ticks equal to $$4^7$$. Since the clock has 33 hours, the final position of the hand will be $$4^7 mod \ 33$$ which is $$16$$. Alice sends this number to Bob.

Anyone intercepting the message knows that the clock has 33 hours, that the original number was raised to the power $$7$$ and that the result of the operation is $$16$$. But the clock only moves clockwise, so no one can reverse the operation to find what was the original number. In order to get back to the original number, there is only one way forward.

Bob has the same clock with him. In order to decipher the message, he does the following operation: starting with numebr $$16$$, he turns the hand by a fixed number of ticks equal to $$16^3$$. The final position of the hand will be $$16^3 mod \ 33 = 4$$. And *voilà*!

# Not your everyday clock

This sequence of operations works because the numbers $$3$$, $$7$$ and $$33$$ were not chosen randomly. They are connected by special properties of prime numbers. 

Let's start with the number of ticks, or **modulus**: it was chosen to be the product of two prime numbers (namely $$3$$ and $$11$$). The public key $$7$$ was chosen so that it is **coprime** to $$33$$. And finally the private key $$3$$ is the **modular multiplicative inverse** of $$7$$ modulo $$\phi(33)$$.

The security in RSA comes from the fact that **modular exponentiation** is a one-way function: it is easy to turn the clock hand forward by a given number, but it's hard to reverse the operation. In order to cancel out the initial operation, one needs to raise the result by the modular multiplicative inverse, i.e. keep turning the hand forward by a specific number of ticks. This is the private key and only Bob knows it. Computing the modular multiplicative inverse is easy for for a semiprime number N when you know how to factor it into prime numbers. 

The real security in RSA comes from the fact that factoring $$n$$ into a product of prime numbers is hard when $$n$$ is very big. If an attacker manages to find the two prime numbers $$p$$ and $$q$$ so that $$p \cdot q = n$$, then computing $$\phi(n)$$ is trivial and deriving the private key $$d$$ for a given $$e$$ is relatively easy.

# Different flavours of RSA

**RSA-1024** uses a modulus of 1024 bits which is 309 digits.


