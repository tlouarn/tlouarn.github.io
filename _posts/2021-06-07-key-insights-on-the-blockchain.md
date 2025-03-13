---
title: Key insights on the blockchain
tag: blockchain
toc: true
---
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

The blockchain lives thanks to an army of nodes validating transactions and building blocks.

The majority of these nodes are honest in the sense that they only validate legitimate transactions by checking that the bitcoins spent by a given address were previously received by said address. But some nodes could be malicious and insert into the blocks illegitimate transactions that would serve their own interest.

In order to profit from these malicious transactions, the malicious nodes would then need to win consensus, i.e. to broadcast the altered blockchain and have it accepted by the rest of the network. Since the majority of other nodes are honest, the altered version of the blockchain would immediately be rejected, and the malicious nodes would only be owning bitcoins in their own branch of the blockchain like kings only in their own world.

The only way to win the consensus is the control the network, which boils down to controlling the hash rate, i.e. controlling more than 50% of the total available computing power plugged to the network. This is known as the 51% attack.

The problem with this attack is that **it carries its own doom**.

Since the goal of any miner is to accumulate Bitcoin by mining the blockchain, the nature of their reward is intrinsically linked to the general value of the network. In other words, if a malicious node was to gain over 50% control of the network and start broadcasting and validating its double-spends, the majority of the public (including other malicious users trying to cheat the system in their own interest!) would lose trust in this blockchain and stop using it. As a result, the malicious node would be unable to spend its illegally acquired bitcoins.

At the time of writing, the Bitcoin network hash rate is about 150,000,000 TH/s and the top three mining pools are AntPool with 21% of the hash rate, followed by F2Pool with 20% and ViaBTC with 13%. So the top mining pool is still a long way from controlling half of the network, and the risk is further reduced by the fact that a pool is in turn composed of individual miners who freely choose to allocate their computing power and can switch it to a different pool or mine for themselves anytime if they detect suspicious activity.
