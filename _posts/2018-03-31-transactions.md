---
layout: default
title:  "TransactionBuilder"
date:   2018-03-31
---

The essense of Bitcoin Cash is all about sending and receiving transactions. The ability to move any amount of money to anyone on EARTH for nearly free is truly revolutionary. Today we're very happy to introduce the `TransactionBuilder` class so developers can quickly send $BCH transactions from their apps.

## `TransactionBuilder`

```js
// create 256 bit BIP39 mnemonic
let mnemonic = BITBOX.Mnemonic.generate(256)

// root seed buffer
let rootSeed = BITBOX.Mnemonic.toSeed(mnemonic);

// master HDNode
let masterHDNode = BITBOX.HDNode.fromSeed(rootSeed, 'bitcoincash');

// HDNode of BIP44 account
let account = BITBOX.HDNode.derivePath(masterHDNode, "m/44'/145'/0'");

// derive the first external change address HDNode which is going to spend utxo
let change = BITBOX.HDNode.derivePath(account, "0/0");

// instance of transaction builder
let transactionBuilder = new BITBOX.TransactionBuilder();
// original amount of satoshis in vin
let originalAmount = result[0].satoshis;

// index of vout
let vout = result[0].vout;

// txid of vout
let txid = result[0].txid;

// add input with txid and index of vout
transactionBuilder.addInput(txid, vout);

// get byte count to calculate fee. paying 1 sat/byte
let byteCount = BITBOX.BitcoinCash.getByteCount({ P2PKH: 1 }, { P2PKH: 1 });
// 192
// amount to send to receiver. It's the original amount - 1 sat/byte for tx size
let sendAmount = originalAmount - byteCount;

// add output w/ address and amount to send
transactionBuilder.addOutput(cashAddress, sendAmount);
// keypair
let keyPair = BITBOX.HDNode.toKeyPair(change);

// sign w/ HDNode
let redeemScript;
transactionBuilder.sign(0, keyPair, redeemScript, transactionBuilder.hashTypes.SIGHASH_ALL, originalAmount);

// build tx
let tx = transactionBuilder.build();

// output rawhex
let hex = tx.toHex();

// sendRawTransaction to running BCH node
BITBOX.RawTransactions.sendRawTransaction(hex).then((result) => {
  console.log(result);
}, (err) => {
  console.log(err);
});
```

## Limitations

`TransactionBuilder` currently only supports 1-to-1, 1-to-many and many-to-1 P2PKH SIGHASH_ALL scripts. More advanced scripts such as P2SH/P2MS are in the works.

## Summary

BITBOX now lets you generate entropy, mnemonics, root seeds, HDNodes, transactions and send them to the network. It's now easier than ever to add $BCH functionality to your applications.

## More Info

[TransactionBuilder](https://www.bitbox.earth/bitboxcli/transactionbuilder)
