---
title: The 51% attack carries its own doom
tags: blockchain
---

![Illustration](/assets/images/2021-06-07-the-51-percent-attack-carries-its-own-doom.jpg)

Doom article. So far we have considered that all the nodes in the network were honest. 

The blockchain lives thanks to an army of nodes validating transactions and building blocks.

The majority of these nodes are honest in the sense that they only validate legitimate transactions by checking that the bitcoins spent by a given address were previously received by said address. But some nodes could be malicious and insert illegitimate transactions into the blocks for their own benefit.

Then, the malicious nodes would need to win consensus, which means to broadcast the altered blockchain and have it accepted by the rest of the network. Since the majority of other nodes are honest, the altered version of the blockchain would immediately be rejected, and the malicious nodes would only be owning illegitimate bitcoins in their own branch of the blockchain like kings in their own world.

The only way to win the consensus is the control the network, which boils down to controlling the hash rate, i.e. controlling more than 50% of the total available computing power plugged to the network. This is known as the 51% attack.

The problem with this attack is that **it carries its own doom**.

Since the goal of any miner is to accumulate Bitcoin by mining the blockchain, the nature of their reward is intrinsically linked to the general value of the network. In other words, if a malicious node was to gain 51% control of the network and start broadcasting and validating its double-spends, the majority of the public (including other malicious users trying to cheat the system in their own interest!) would lose trust in this blockchain and stop using it. As a result, the malicious node would be unable to spend its illegitimate bitcoins.

At the time of writing, the Bitcoin network hash rate is about 150,000,000 TH/s and the top three mining pools are AntPool with 21% of the hash rate, followed by F2Pool with 20% and ViaBTC with 13%. So the top mining pool is still a long way from controlling half of the network.