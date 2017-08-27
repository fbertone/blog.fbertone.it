---
layout: post
title: "[EN] Write Data in Bitcoin Blockchain"
date: 2017-08-27
categories: []
tags: [javascript, node, bitcoin, blockchain]
---

# Write Data in Bitcoin Blockchain

[Bitcoin](http://bitcoin.org) needs no introduction. It is known to be the first and most popular cryptocurrency based on the blockchain. However not everybody is aware that apart from financial transactions, it can be also used to store custom data in various ways. While this is somehow discouraged as it bloats the blockchain with information that goes outside its primary scope, apparently [Satoshi](https://en.bitcoin.it/wiki/Satoshi_Nakamoto) himself inserted some messages already in the [first blocks](https://bitcoinstrings.com/), followed shortly by the [first miners](https://bitcointalk.org/index.php?topic=38007.0). A **very** interesting review of some of the hidden messages found in the blockchain is [here](http://www.righto.com/2014/02/ascii-bernanke-wikileaks-photographs.html) (Also check [cryptograffiti](http://www.cryptograffiti.info/) and [doge yip](https://geo-gs.github.io/dogeyip/))

Most of the methods usable have negative inpacts, like “*destroying*” bitcoins or wasting nodes memory, but starting from [version 0.9 of the protocol](https://github.com/bitcoin/bitcoin/blob/57b34599b2deb179ff1bd97ffeab91ec9f904d85/doc/release-notes/release-notes-0.9.0.md#op_return-and-data-in-the-block-chain) a new technique was made possible, reducing somehow the issues. This is called OP_RETURN and allows to create secondary transactions that does not involve transferring bitcoins, while allowing to embed a small quantity of data. 
Initially the limit of insertable data was set to 40 bytes, but in [version 0.11 was raised to 80](https://github.com/bitcoin/bitcoin/blob/57b34599b2deb179ff1bd97ffeab91ec9f904d85/doc/release-notes/release-notes-0.11.0.md#block-and-transaction-handling). (Even so, apparently is still possible to insert data overcoming those limits.)
One example of service that exploits OP_RETURN to store data (in this case hash of files) is [proof of existence](https://proofofexistence.com/about).

To store custom data with OP_RETURN we need:

* a bitcoin wallet
* bitcoins to pay miner fees
* data to store

TO BE CONTINUED
