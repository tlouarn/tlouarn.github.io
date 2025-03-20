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

# The code

Now obviously, we are not going to use padlocks to protect information. Instead, we will use prime numbers, but not your average prime, very big prime numbers.

The system works as follows: we take a very big number and get the 2 prime numbers that, multiplied together, give this big nuumber.
