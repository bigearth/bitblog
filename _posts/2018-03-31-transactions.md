---
layout: default
title:  "TransactionBuilder"
date:   2018-03-31
---

The essense of Bitcoin Cash is all about sending and receiving transactions. The ability to move any amount of money to anyone on EARTH for nearly free is truly revolutionary. Today we're very happy to introduce the `TransactionBuilder` class so developers can quickly send $BCH transactions from their apps.

## `TransactionBuilder`

```js
// the mnemonic for your root seed. Can be generated w/ BITBOX.Mnemonic.generateMnemonic(256)
let mnemonic = 'your mnemonic';

// root seed buffer
let seed = BITBOX.Mnemonic.mnemonicToSeedBuffer(mnemonic);

// master HDNode
let master = BITBOX.HDNode.fromSeedBuffer(seed, 'bitcoincash');

// get byte count to calculate fee. paying 1 sat/byte
let byteCount = BITBOX.BitcoinCash.getByteCount({ P2PKH: 1 }, { P2PKH: 1 });

// node of address which is going to spend utxo
let node = master.derivePath(`m/44'/145'/0'/0/0`);

// keypair
let keyPair = node.keyPair;

// amount of satoshis in vin
let originalAmount = 14438;

// amount to send to receiver. It's the original amount - 1 sat/byte for tx size
let amount = originalAmount - byteCount;

// txid of vin
let txid = 'txid';

// instance of transaction builder
let txb = new BITBOX.TransactionBuilder(keyPair, 'bitcoincash');

// add input
txb.addInput(txid, 0);

// add as many outputs as you wish
txb.addOutput('bitcoincash:to-address', amount);

// sign w/ node's keyPair
txb.sign(0, originalAmount);

// build tx
let tx = txb.build();

// output rawhex
let hex = tx.toHex();

// sendRawTransaction to running BCH node
BITBOX.RawTransactions.sendRawTransaction(hex).then((result) => { console.log(result); }, (err) => { console.log(err); });
```

## Limitations

`TransactionBuilder` currently only supports 1-to-1 or 1-to-many P2PKH SIGHASH_ALL scripts. More advanced scripts such as many to one and P2SH and P2MS are in the works.

## More Info

[TransactionBuilder](https://www.bitbox.earth/bitboxcli/transactionbuilder)
