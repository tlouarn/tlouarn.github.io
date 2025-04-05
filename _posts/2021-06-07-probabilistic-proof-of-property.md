---
title: Probabilistic proof of property
tags: blockchain
---

In a traditional setup, dubbed TradFi, the proof of property is **centralized** and **deterministic**. 

For instance, if you have a bank account with one GBP in it, only the bank can validate that you own the one GBP (it is centralized) but when it does, it guarantees that you own this amount (it is deterministic). There is only one version of the truth and this truth is certain.

On the blockchain however, the proof of property is **decentralized** and **probabilistic**. 

Owning one bitcoin means having your address credited with one bitcoin on the main branch of the blockchain. It is therefore decentralized in the sense that anyone downloading the main branch can quickly look through historical transactions and see that the balance in your wallet is valid. But it remains probabilistic in the sense that there could be other versions of the blockchain ("forks") where your address never received the bitcoin. As long as the branch on which you received the bitcoin is the main branch, you "own" the bitcoin. But it is always possible, though exponentially less likely as time goes by, that a different branch coming from different nodes (and on which you don't have one bitcoin) wins the general consensus, at which point you will no longer "own" this bitcoin. In other words, there are many possible versions of the truth and none of them is certain, although one of them generally gathers a wide consensus. 

The common heuristic to consider a transaction as properly approved after having been integrated to a block is to wait for the next 6 blocks to be mined, which takes around an hour. If after 6 blocks your transaction is still in the blockchain, it is considered immutable.