---
Title: Key insights on the blockchain
---

# Property becomes probabilistic

This is, to me, the most remarkable consequence of the Bitcoin blockchain technology.

In a traditional setup, dubbed TradFi, the proof of property is **centralized** but **deterministic**. For instance, if you have a bank account at Barclays with 1,000 GBP on it, only Barclays can validate that you indeed have 1,000 GBP (it is centralized) but when it does, it guarantees that you own this amount (it is deterministic). There is only one version of the truth and this truth is certain.

On the blockchain however, the proof of property is **decentralized** but **probabilistic**: if you own a wallet with 1 bitcoin in it, anyone downloading a version of the blockchain (emphasis on the version) can look through historical transactions and see that this bitcoin balance in your wallet is valid, but it is only valid on the very version of the blockchain it was looked into. As a matter of fact, some nodes may well be working on a different version of the blockchain (a fork) on which your wallet does not have this bitcoin.

And even if the transactions amounting to your current balance of 1 bitcoin stay on the main branch of the blockchain, it is always possible, though less and less likely as time goes by, that a different branch coming from different nodes (and on which you don't have 1 bitcoin) wins the general consensus, at which point you will no longer own this bitcoin.

> In other words, there are many possible versions of the truth and none of them is certain, although one of them generally gathers a wide consensus. 

The generally accepted heuristic to consider a transaction as properly approved after having been integrated to a block is to wait for the next 6 blocks to be mined, which takes around an hour. If after 6 blocks your transaction is still in the blockchain, it is considered immutable.
