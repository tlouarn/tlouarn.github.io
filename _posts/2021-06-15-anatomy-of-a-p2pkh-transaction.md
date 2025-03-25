---
title: Anatomy of a P2PKH transaction
tags: blockchain bitcoin
---

> We define an electronic coin as a chain of digital signatures. Each owner transfers the coin to the next by digitally signing a hash of the previous transaction and the public key of the next owner and adding these to the end of the coin. A payee can verify the signatures to verify the chain of ownership.
> 
> Satoshi Nakamoto

While the term **Smart Contract** is often associated with Ethereum, the Bitcoin blockchain protocol also incorporates embedded logic.

A Bitcoin transaction is a little more complex than "Alice gives X to Bob". Instead, it operates as **"Alice permits Bob to unlock X bitcoins when he chooses to spend them"**. Each transaction includes a script detailing how can Bob unlock the bitcoins.

This script is written in a language named `Script` (you can't make that up). Script is a simple, stack-based language designed to be reliable and avoid failures.

The vast majority of transactions follow the same instruction code saying that if the recipient of the funds can sign his/her own subsequent transactions with a signature matching the HASH, then the transaction is validated. This script is named **P2PKH**, or Pay To PubKey Hash.

Consider the following example of a locking script from a [transaction](https://www.blockchain.com/btc/tx/7e8dffd030f716d7f5971f640f8d62fec10bccbb5ba931e548a05c604071a9c1) transaction on blockchain.com:

```cmd
OP_DUP 
OP_HASH160
9d229cc07a96fd4d7a79e81354ab40ab6e394104 
OP_EQUALVERIFY 
OP_CHECKSIG
```

In plain English, it means: "whoever has a public key hash equal to `9d229...` and can digitally sign this message can spend the output".

References:
* [https://www.coursera.org/learn/wharton-cryptocurrency-blockchain-introduction-digital-currency]
* [https://www.coursera.org/learn/cryptocurrency]
* [https://www.bitcoin.com/bitcoin.pdf]
* [https://learnmeabitcoin.com/technical/p2pkh]
* [http://royalforkblog.github.io/2014/11/20/txn-demo/]