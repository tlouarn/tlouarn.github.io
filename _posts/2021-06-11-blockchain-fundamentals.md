---
title: Blockchain Fundamentals (1)
tags: bitcoin, blockchain, hash
---

![Illustration](/assets/images/tlouarn-blockchain-fundamentals.jpg)

This post is part of a series where I try to explain the basic principles of the blockchain technology in English.

In this first post, I'm introducing two key concepts that will be reused throughout the series.
I try to remain high-level and do not expose all the underlying complexity of the concepts used.

Prerequisites: if you are not familiar with the concepts of hash functions and public/private keys, I encourage you to read these posts first before the blockchain.

# A decentralized network





Hash rate definition
Public / Private key definition. The classic Alice and Bob example.
Illustration with the double chain.
In terms of code
SH-256
Bitcoin: the public key is the wallet, the private key is necessary to open the wallet.


Private: look at the whales and the structure of the blockchain. Chainalysis. We don’t know who is address XXX, but we know it owns X bitcoins.

Blockchain Fundamentals
Notes from various readings on the subject.
In this post I am going to try to describe the bitcoin blockchain with words.
A high-level description of the network was published in 2009 by anonymous author Satoshi Nakamoto. The paper is available here.
The paper describes a decentralized system Bitcoin transactions are recorded on a public, distributed ledger called the blockchain, without a central authority like a bank or government controlling the network
The blockchain is literally a chain of blocks. Each blocks contains a number of transactions.  The blockchain is a decentralized system, which means there is no central storage.
The nodes are independent entities connected to a decentralized network and mining blocks. Anyone can create a node by downloading the bitcoin software and running it on a computer. Nodes are being added to and removed from the network all the time.
From the point of view of a node
The system is decentralized, meaning that when a node connects to the network, there is nowhere to go to “download” the latest version of the blockchain and start mining blocks. Instead, the node can only connect to neighbouring nodes and see what blockchain they have. Information transits from node to node. Information contains past transactions aggregated into blocks as well as pending transactions. 

From the point of view of a transaction:
Say you buy a coffee using bitcoin, the payment terminal is connected to a few nodes on the network and starts broadcasting the transaction (ie the coffee shop’s bitcoin address is receiving an amount from your wallet). Each node receiving the transaction is adding it to its pending transactions and transmitting it forward (TBC: push or pull?). So each node has some version of the blockchain and pending transactions. Each nodes tries to aggregate the transactions into blocks. TBC orders of magnitude: how many pending transactions at any given time and how many transactions per block?
Let’s now focus on a particular node which received your transaction to mine. The first step is trivial: the node will look into all historical transactions and recompute the balance of your wallet to make sure you “own” what you spend. This is very fast (TBC at time of writing how many blocks, how many transactions, how long to parse and check). Once validated, the node checks the next transaction. Once a given number of transactions has been validated, the block starts the mining process. Mining is like rubber stamping a block of transaction.
Proof of workThis is the hard part. In order to rubber stamp its block, the node needs to solve a cryptographic problem (hence the name crypto) by solving for a hash function.
A hash function is a function so that it is trivial to calculate F(X) = Y but impossible to analytically solve X for a given Y.
Example of hash: hash(“Hello”) = 83737 is fast to calculate. But which input should be used for say 83838 (very close output)? This is not solvable analytically, ie there is no formula. Instead, one could start trying all possibilities until it finds the right one. This is called “brute-forcing” and is exactly what the nodes are doing when “mining a block”. The “network” sets the current difficulty of the problem to be resolved, and the nodes are all looping to solve their own blocks. Remember that the network is decentralized, so at this stage your transaction may be on the blocks of nodes A, B and D but not C and E. At any given time, each node is working on a different block. Not all nodes know of all the transactions at any given time so each block is different.
Solving the problem requires a lot of computing power. We measure the power in hash rate i.e. how many hashes can be tried in 1 second. At the time of writing, the network as a whole has a hashrate of X (TBC).
Let’s imagine the node manages to solve the problem. It adds the block to its version of the blockchain, publishes it to the neighouring nodes and starts working on the next block. The other nodes will receive the new block, check that the transactions in it are valid and that the cryptographic puzzle was solved, which is instantaneous, then drop what they were working on, add the new block to their copy of the chain, broadcast it in turn, check which of their pending transactions are already included in the new block, remove them from their own pending transactions, and start working on new block.
Orders of magnitude:- how fast does a new block propagate through the network?- how many transactions are pending at any given time?

Earlier on I said that the network 

The bitcoin software is open source and available on GitHub (link). Many third-party software implement the bitcoin protocol.

Quantum software and bitcoin: cryptographic risk.
BTCUSD: what do we mean? There is only BTC in the blockchain, no USD. Transfers between wallets are only bitcoin. So what happens when you trade BTCUSD? There are two types of settlement in the traditional industry named FOP and DVP. Bitcoin is FOP ie “send X bitcoins to my address and I promise I’ll send you Y USD to your bank account”. Transactions on the blockchain can be gifts or payments against goods and services settled in the real world. It’s like looking at a bank statement.

Why the cryptographic puzzle? Why not an instant verification like the Visa or Mastercard network? Because in the case of Visa, Visa has the responsibility to check. It is centralized. In the case of Bitcoin.
The notion of reward. Once block is added, the node sees itself rewarded. The bitcoin received as compensation for the work is decided by the network. This is the only bitcoin creation mechanism. All the bitcoins in circulation were minted by miners who then spent them ie sent to other wallets. There will only be 21m bitcoins in circulation. The reward decreases over time (“halving”). Every X number of blocks, each block takes around 10 minutes. Assuming there is a constant inflow of transactions to validate, we can forecast the halvings. We also know how much bitcoin is already in circulation and how much is yet to be created.
Bitcoin is both the network and the currency. Value vs USD has no fundamental other than the amount of electricity consumed to solve the cryptographic puzzle.
What if faster verification? Litecoin etc.What if more functionalities added to the blockchain like smart contracts etc? Ethereum.



Exchanges: you don’t have your own wallet
Cold storage: definition


Software to check whether some text was generated by chatgpt as firefox extension (gpt-score). Can chatgpt identify whether a text was produced by chatgpt?






Bitcoin hacking:- private key stolen- exchange hacking- 51% attack
Lost keysDormant walletsMultiple wallets for one individual



