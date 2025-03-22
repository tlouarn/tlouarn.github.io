---
title: Public and private keys
subtitle: Meet Alice and Bob in their prime.
tags: algorithm
---

# The problem

Alice wants to send a confidential document to Bob but she does not have access to a private courier and needs to use a public communication channel.

Alice could place the document in a box, lock it with a padlock, and send it to Bob. This way, anyone intercepting the box wouldn't be able to open it. But neither would Bob, since he doesn't have the key. So that does not work.

Alice could send the locked box in a parcel and the key in another. The thief would need to intercept both parcels in order to open the box. Although slightly more secure, this approach is still not good enough.

Now let us assume Bob also has a padlock. The solution becomes:
* Bob sends his padlock to Alice (but keeps the key)
* Alice places the document in the box and locks it with Bob's padlock
* Bob receives the box and can unlock it.

In this example, Bob's padlock is called a **public key** and Bob's key is called a **private key**. And we are able to replicate the same mechanism in the software world using properties of prime numbers.

# A very special clock

Let us assume that the message Alice wants to send to Bob boils down to one number. She needs to encrypt it and transmit it to Bob in such a way that only Bob can decipher it.

Let us now imagine a very special clock where the number of hours can be arbitrarily set and where there is only 1 hand that can move clockwise from hour to hour. Let us assume Bob's public key is **(7, 33)** and Bob's secret key is **(3, 33)**.

In order to encrypt the number 4, Alice does the following operation: she turns the hand by a fixed number of ticks equal to $$4^7$$. Since the "clock" has 33 ticks, the final position of the hand will be $$4^7 mod 33$$ which is $$16$$ and she sends this number to Bob.

Bob has the same clock with him. In order to decipher the message, he does the following operation: he turns the hand by a fixed number of ticks equal to $$16^3$$. The final position of the hand will be $$16^3 mod 33 = 4$$. And *voilà*!

# Not your everyday clock

Now this sequence of operations works because the numbers $$3$$, $$7$$ and $$33$$ were not chosen randomly. They are connected by special properties of prime numbers. Let's start with the number of ticks, or **modulus**: it was chosen to be the product of two prime numbers (namely $$3$$ and $$11$$). The public key $$7$$ was chosen so that it is **coprime** to $$33$$. And finally the private key $$3$$ is the **modular multiplicative inverse** of $$7$$ modulo $$\phi(33)$$. $$\phi$$ is Euler's totient function.

The real security in RSA comes from the fact that factoring $$n$$ into a product of prime numbers is hard when $$n$$ is very big. If an attacker manages to find the two prime numbers $$p$$ and $$q$$ so that $$p \cdot q = n$$, then computing $$\phi(n)$$ is trivial and deriving the private key $$d$$ for a given $$e$$ is relatively easy.

# Different flavours of RSA

**RSA-1024** uses a modulus of 1024 bits which is 309 digits.
