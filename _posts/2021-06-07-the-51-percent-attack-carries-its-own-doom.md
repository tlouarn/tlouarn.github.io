---
title: The 51% attack carries its own doom
tag: blockchain
---

![Illustration](/assets/images/2021-06-07-the-51-percent-attack-carries-its-own-doom.jpg)

The blockchain lives thanks to an army of nodes validating transactions and building blocks.

The majority of these nodes are honest in the sense that they only validate legitimate transactions by checking that the bitcoins spent by a given address were previously received by said address. But some nodes could be malicious and insert into the blocks illegitimate transactions that would serve their own interest.

In order to profit from these malicious transactions, the malicious nodes would then need to win consensus, i.e. to broadcast the altered blockchain and have it accepted by the rest of the network. Since the majority of other nodes are honest, the altered version of the blockchain would immediately be rejected, and the malicious nodes would only be owning bitcoins in their own branch of the blockchain like kings only in their own world.

The only way to win the consensus is the control the network, which boils down to controlling the hash rate, i.e. controlling more than 50% of the total available computing power plugged to the network. This is known as the 51% attack.

The problem with this attack is that **it carries its own doom**.

Since the goal of any miner is to accumulate Bitcoin by mining the blockchain, the nature of their reward is intrinsically linked to the general value of the network. In other words, if a malicious node was to gain over 50% control of the network and start broadcasting and validating its double-spends, the majority of the public (including other malicious users trying to cheat the system in their own interest!) would lose trust in this blockchain and stop using it. As a result, the malicious node would be unable to spend its illegally acquired bitcoins.

At the time of writing, the Bitcoin network hash rate is about 150,000,000 TH/s and the top three mining pools are AntPool with 21% of the hash rate, followed by F2Pool with 20% and ViaBTC with 13%. So the top mining pool is still a long way from controlling half of the network, and the risk is further reduced by the fact that a pool is in turn composed of individual miners who freely choose to allocate their computing power and can switch it to a different pool or mine for themselves anytime if they detect suspicious activity.