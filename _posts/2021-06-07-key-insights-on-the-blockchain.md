---
title: Key insights on the blockchain
tag: blockchain
toc: true
---
<<<<<<< HEAD




=======
![846e4f4d-1664-462b-8acd-7268755df8a2](https://github.com/user-attachments/assets/45275135-24ee-4624-897a-166cc4dae2b3)

>>>>>>> 8a922856c1268811ca9a6b50a16d88c7e238ea2a
I've found myself diving into blockchain articles once again. Guilty as charged! Here are three *notes to self* I wish to remember about this technology.




* Will be replaced with the ToC, excluding the "Contents" header
{:toc}

# Property becomes probabilistic

This is, to me, the most remarkable consequence of the Bitcoin blockchain technology.

In a traditional setup, dubbed TradFi, the proof of property is **centralized** but **deterministic**. For instance, if you have a bank account at Barclays with 1,000 GBP on it, only Barclays can validate that you indeed have 1,000 GBP (it is centralized) but when it does, it guarantees that you own this amount (it is deterministic). There is only one version of the truth and this truth is certain.

On the blockchain however, the proof of property is **decentralized** but **probabilistic**: if you own a wallet with 1 bitcoin in it, anyone downloading a version of the blockchain (emphasis on the version) can look through historical transactions and see that this bitcoin balance in your wallet is valid, but it is only valid on the very version of the blockchain it was looked into. As a matter of fact, some nodes may well be working on a different version of the blockchain (a fork) on which your wallet does not have this bitcoin.

And even if the transactions amounting to your current balance of 1 bitcoin stay on the main branch of the blockchain, it is always possible, though less and less likely as time goes by, that a different branch coming from different nodes (and on which you don't have 1 bitcoin) wins the general consensus, at which point you will no longer own this bitcoin.

> In other words, there are many possible versions of the truth and none of them is certain, although one of them generally gathers a wide consensus. 

The generally accepted heuristic to consider a transaction as properly approved after having been integrated to a block is to wait for the next 6 blocks to be mined, which takes around an hour. If after 6 blocks your transaction is still in the blockchain, it is considered immutable.

# Anonymity but no privacy

In a centralized system, the central authority is the only one able to validate property rights, therefore it can maintain a certain level of privacy.

To reuse the previous example, if you have an account open at Barclays with 1,000 GBP in it, no one else except you and Barclays need to know that. Barclays does not give away its ledger with the account names and balances!

On the contrary, in a decentralized system, each node has access to the list of accounts (addresses) and is able to recompute the current balances.

The ledger is public, therefore the privacy is gone.

> Privacy is not to be mixed up with anonymity. Although lacking privacy, the blockchain is still anonymous in the sense that the public ledger is made of addresses behind which one or several individuals can hide.

Since the ledger is public, let's look into who has the biggest wallet!

At the time of writing, it's address `34xp4vRoCGJym3xR7yCVPFHoCNxv4Twseo` containing 293,427 BTC, which amounts to a comfortable 10.6 billions USD equivalent (live update on [this website](https://bitinfocharts.com/top-100-richest-bitcoin-addresses.html) among many others).

# The 51% attack carries its own doom


