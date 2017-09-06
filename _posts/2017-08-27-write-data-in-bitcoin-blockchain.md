---
layout: post
title: "[EN] Write Data in Bitcoin Blockchain"
date: 2017-08-27
categories: []
tags: [javascript, node, bitcoin, blockchain]
---

# Write Data in Bitcoin Blockchain #

[Bitcoin](http://bitcoin.org) needs no introduction. It is known to be the first and most popular cryptocurrency based on the blockchain. However not everybody is aware that apart from financial transactions, it can be also used to store custom data in various ways. While this is somehow discouraged as it bloats the blockchain with information that goes outside its primary scope, apparently [Satoshi](https://en.bitcoin.it/wiki/Satoshi_Nakamoto) himself inserted some messages already in the [first blocks](https://bitcoinstrings.com/), followed shortly by the [first miners](https://bitcointalk.org/index.php?topic=38007.0). A **very** interesting review of some of the hidden messages found in the blockchain is [here](http://www.righto.com/2014/02/ascii-bernanke-wikileaks-photographs.html) (Also check [cryptograffiti](http://www.cryptograffiti.info/) and [doge yip](https://geo-gs.github.io/dogeyip/))

Most of the methods usable have negative inpacts, like “*destroying*” bitcoins or wasting nodes memory, but starting from [version 0.9 of the protocol](https://github.com/bitcoin/bitcoin/blob/57b34599b2deb179ff1bd97ffeab91ec9f904d85/doc/release-notes/release-notes-0.9.0.md#op_return-and-data-in-the-block-chain) a new technique was made possible, reducing somehow the issues. This is called OP_RETURN and allows to create secondary transactions that does not involve transferring bitcoins, while allowing to embed a small quantity of data. 
Initially the limit of insertable data was set to 40 bytes, but in [version 0.11 was raised to 80](https://github.com/bitcoin/bitcoin/blob/57b34599b2deb179ff1bd97ffeab91ec9f904d85/doc/release-notes/release-notes-0.11.0.md#block-and-transaction-handling). (Even so, apparently is still possible to insert data overcoming those limits.)
One example of service that exploits OP_RETURN to store data (in this case hash of files) is [proof of existence](https://proofofexistence.com/about).

To store custom data with OP_RETURN we need:

* a bitcoin wallet
* bitcoins to pay miner fees
* data to store

To keep things simple, we will not use an actual node of the network, instead we will relay on API from a third party service, for example [Blocktrail](https://www.blocktrail.com). To use the API we need (free) API keys. Register an account on blocktrail and request your keys. We will also use the nodejs SDK provided by them.

Install the SDK running:
```bash
npm install blocktrail-sdk
```

Since bitcoins nowadays are worth big money, we will use the [testnet](https://en.bitcoin.it/wiki/Testnet) instead of the official blockchain.

OK, let's start building our client.

First of all, we have to import the SDK that we installed in previous step:

```javascript
const blocktrail = require('blocktrail-sdk')
```

Next we set some constant for later use, use your own values:

```javascript
const apiKey = '<YOUR_API_KEY>'
const apiSecret = '<YOUR_API_SECRET>'
const testnet = true

const walletName = 'mywallet'
const walletPass = 'mypass'
let myWallet
let myAddress
```

apiKey and apiSecret are the tokens we received from blocktrail, while walletName and walletPass are the name and the password for our wallet. You can choose whatever you want. Note the constant testnet set to true to signalate that we want to use the testnet.

The next step is the creation of a client, instantiated with our API keys.

```javascript
const client = blocktrail.BlocktrailSDK({
    apiKey : apiKey,
    apiSecret : apiSecret,
    testnet: testnet
})
```

After that, we need to create a new wallet (or initialise an existing one).

```javascript
client.createNewWallet(walletName, walletPass, walletCB)
```

Let's define walletCB this way:

```javascript
function walletCB(err, wallet, backupInfo) {
    if (err) {
      console.log(err) // maybe wallet already exists, try to initialize it
      client.initWallet(walletName, walletPass, function(err, wallet) {
        if (err) {
          console.log(err)
        } else {
          getAddress(wallet)
        }
      })
    } else {
      getAddress(wallet)
    }
}
```

In getAddress we create a new address and initialize it with some coinsfrom the faucet:

```javascript
function getAddress(wallet) {
  myWallet = wallet
  wallet.getNewAddress(function(err, address) {
    if (err) {
      console.log(err)
    } else {
      myAddress = address
      client.faucetWithdrawl(address, blocktrail.toSatoshi(0.001), faucetCB)
    }
  })
}
```

Now we are ready to create a new transaction with our embedded data!

```javascript
function faucetCB(err, res) {
  if (err) {
    console.log(err)
  } else {
    console.log(res)
    let transaction = {}
    transaction[myAddress] = blocktrail.toSatoshi(0.001)
    transaction[blocktrail.Wallet.OP_RETURN] = 'https://blog.fbertone.it/2017/08/27/write-data-in-bitcoin-blockchain.html'

    myWallet.pay(transaction, function(err, result) {
      if (err) {
        return console.log(err)
      }
      console.log('Transaction id: ' + result)
    })
  }
}
```

And the result is: 

> [aefc5c1eec6af8f33ec14d7432d2d47eb4e9017ab3114569e25939a8b1a71eb1](https://www.blocktrail.com/tBTC/tx/aefc5c1eec6af8f33ec14d7432d2d47eb4e9017ab3114569e25939a8b1a71eb1#tx_messages)

## Conclusions ##

Blockchain is a disruptive technology still in its early days (sort of). In this brief tutorial we have seen a simple trick to store a simple string inside the Bitcoin network. In next posts we will go further by storing files and retrieving them afterwards.
