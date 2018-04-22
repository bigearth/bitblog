---
layout: default
title:  "Complete transaction setup"
date:   2018-04-01
---

So you want to send $BCH transactions? Worthy goal. Follow this guide and soon the transactions will flow like the champaign of the Illinois countryside.

## Installation

First things first. You're gonna need BITBOX. For that you'll need NodeJS and `npm`.

### NodeJS and npm

[NodeJS](https://nodejs.org) is a javascript runtime built on Chrome's V8 engine. [npm](https://www.npmjs.com) is the package manager for NodeJS. Both can be installed via `nvm` the [node version manager](https://github.com/creationix/nvm).

Install `nvm` w/

```
curl -o- https://raw.githubusercontent.com/creationix/nvm/v0.33.8/install.sh | bash
```

Once you have `nvm` install NodeJS v9.4.0 w/

```
nvm install 9.4.0
```

This also installs `npm` v5.6.0.

### BITBOX

Once those are both installed you're ready to install BITBOX.

```
npm install bitbox-cli --global
```

This will install the `bitbox-cli` command line utility and give you `bitbox new`, `bitbox console` and `bitbox scaffold`

## Create a new app

Now BITBOX is installed and you can have it scaffold out an app.

```
bitbox new myApp

******   ** ********** ******     *******   **     **
/*////** /**/////**/// /*////**   **/////** //**   **
/*   /** /**    /**    /*   /**  **     //** //** **
/******  /**    /**    /******  /**      /**  //***
/*//// **/**    /**    /*//// **/**      /**   **/**
/*    /**/**    /**    /*    /**//**     **   ** //**
/******* /**    /**    /*******  //*******   **   //**
///////  //     //     ///////    ///////   //     //

Creating myApp/ directory
Creating src/ directory: ./myApp/src
Creating tests/ directory: ./myApp/tests
Creating bitbox.js configuration file: ./myApp/bitbox.js
All done. ✅
Go get em! Remember--with great power comes great responsibility. 🚀

cd myApp/
```

### bitbox.js

`bitbox new` creates an empty BITBOX app directory structure w/ `bitbox.js` configuration file. `bitbox.js` contains credentials to connect to your local running BITBOX or a remote full $BCH node.

```js
exports.config = {
  networks: {
    development: {
      protocol: "http",
      host: "localhost",
      port: "8332",
      username: "",
      password: "",
      corsproxy: false
    }
  }
};
```

If you open `bitbox.js` you'll see that it comes w/ creds for a `development` environment. We'll soon add some `production` credentials.

### Full $BCH node

For you to send the transactions which you're about to generate to the actual $BCH network you're going to need a full running $BCH node. Follow my steps on [how to setup a $BCH node on Digital Ocean](https://gist.github.com/cgcardona/843f81f5865f755b4c90513c61ae7612).

Once you've done that add the credentials to your `bitbox.js` file by adding a `production` object.

```js
exports.config = {
  networks: {
    development: {
      protocol: "http",
      host: "localhost",
      port: "8332",
      username: "",
      password: "",
      corsproxy: false
    },
    production: {
      protocol: "http",
      host: "ip.address.of.node",
      port: "8332",
      username: "l337",
      password: "h4x0r",
      corsproxy: false
    }
  }
};
```

Great work. You've now installed NodeJS, `npm`, `bitbox-cli`, set up a full $BCH node and stubbed out an empty BITBOX app. The next step is to open a console.

## Console

BITBOX ships w/ a custom NodeJS repl that has the entire BITBOX API built in. To use it run the following:

```
bitbox console --environment production
> BITBOX.Mnemonic.generate(256);
// fresh base moral lunch jacket later shallow verify coffee answer gospel memory lawn economy cover legal slam help giggle enroll basic series essay during

> BITBOX.BitcoinCash.toSatoshi(9);
900000000
```

If you don't pass in an `--environment` flag it will connect to the `development` environment by default.

## Setup

We're almost ready to create and send $BCH transactions. A couple more things to set up first.

### Mnemonic

For the rest of this demo you'll need a mnemonic to generate keypairs w/. I'll use the one I generated above.

```js
let mnemonic = 'hamster lend force inch sort account task saddle output world olive affair oval govern length churn zoo design off element forget current situate beauty';
```

### BIP44 Account and addresses

Now we need a [BIP44](https://github.com/bitcoin/bips/blob/master/bip-0044.mediawiki) account. BIP44 derivation paths follow a certain format.

```
m / purpose' / coin_type' / account' / change / address_index
```

We'll be deriving the first hardened account or `m / 44' / 145' / 0'` and we'll cycle through it's `n`th external change address `0 / n`.


```js
// root seed buffer
let rootSeed = BITBOX.Mnemonic.toSeed(mnemonic);

// master HDNode
let masterHDNode = BITBOX.HDNode.fromSeed(rootSeed, 'bitcoincash');

// HDNode of BIP44 account
let bip44BCHAccount = BITBOX.HDNode.derivePath(masterHDNode, "m/44'/145'/0'");

// get the xpriv so we can recreate this node later
let xpriv = BITBOX.HDNode.toXPriv(bip44BCHAccount);
// xprv9ycRUKofpapUBBdkNbBrLAZodKr4Q7LhgmcLppwS7yicaGuEQ9egswHEsgJj8sjsxgab79GMkmVXRNSJVT8Dsp5Q8MBmAe5ELN1yUbP9Vb1

// derive the first external change address HDNode which is going to spend utxo
let changeAddressNode0 = BITBOX.HDNode.derivePath(bip44BCHAccount, "0/0");

// get the cash address
let changeAddress0 = BITBOX.HDNode.toCashAddress(changeAddressNode0);
// bitcoincash:qqavrxsgx7hufrudzjq0mc8sk9rw09jy0cmgrkdm3q
```

### Prepare the utxo

In this demo I'm going to do 1-to-1, 1-to-many, many-to-many, many-to-1 and OP_RETURN P2PKH SIGHASH_ALL $BCH transactions. For that we need to set up 2 unspent transaction outputs.

Take the `changeAddress0` from the previous step and send it 2 small $BCH transactions. I created 2 utxo.

* [bb2dff2544db7908cec8a41d9003a17f2c02814aa13095448e4bb31b283f5844](https://blockchair.com/bitcoin-cash/transaction/bb2dff2544db7908cec8a41d9003a17f2c02814aa13095448e4bb31b283f5844)
* [72eb365f0f8d692edb5aa8b0d32948c21a80938b8f2870b68a5d58cefdabd513](https://blockchair.com/bitcoin-cash/transaction/72eb365f0f8d692edb5aa8b0d32948c21a80938b8f2870b68a5d58cefdabd513)

### Live demo

Here is `changeAddress0` [on blockchair](https://blockchair.com/bitcoin-cash/address/qqavrxsgx7hufrudzjq0mc8sk9rw09jy0cmgrkdm3q) so you can see this address and transactions on the actual $BCH blockchain.

Now everything is set up and we're ready to send our 1st $BCH transaction.

## 1-to-1

The most basic type of $BCH transaction is a 1-to-1 Pay to PubKeyHash (P2PKH) SIGHASH_ALL script. That's a mouthful but basically that just means 1 person sending another person some $BCH. Let's see how that would look w/ BITBOX.

### `addInput`

Building on our previous code examples we need to create an instance of `TransactionBuilder`, get the keyPair of the node which wants to spend the utxo and pass it w/ the `txid` and `vin` of the utxo to `addInput`.

```js
// instance of transaction builder
let transactionBuilder = new BITBOX.TransactionBuilder('bitcoincash');
// original amount of satoshis in vin
let originalAmount = 75827;
// txid of vin
let txid = 'bb2dff2544db7908cec8a41d9003a17f2c02814aa13095448e4bb31b283f5844';
// add input with txid and index of vout
transactionBuilder.addInput(txid, 0);
```

### `addOutput`

Next we need to calculate the byte count and subtract that number from the original number of satoshis to get a fee of 1 sat/B and add the receive address and send amount to `addOutput`.

```js
// get byte count to calculate fee. paying 1 sat/byte
let byteCount = BITBOX.BitcoinCash.getByteCount({ P2PKH: 1 }, { P2PKH: 1 });
// amount to send to receiver. It's the original amount - 1 sat/byte for tx size
let sendAmount = originalAmount - byteCount;
// add output w/ address and amount to send
transactionBuilder.addOutput('bitcoincash:qpuax2tarq33f86wccwlx8ge7tad2wgvqgjqlwshpw', sendAmount);
```

### `sign` and send to the network

Our transaction is almost ready to roll. Now we need to sign and build it. Then convert it to hex and send it to the $BCH network.

```js
// keypair
let keyPair = BITBOX.HDNode.toKeyPair(changeAddressNode0);
// sign w/ HDNode
let redeemScript;
transactionBuilder.sign(0, keyPair, redeemScript, transactionBuilder.hashTypes.SIGHASH_ALL, originalAmount);
// build tx
let tx = transactionBuilder.build();
// output rawhex
let hex = tx.toHex();
// 020000000144583f281bb34b8e449530a14a81022c7fa103901da4c8ce0879db4425ff2dbb000000006b48304502210080da7c89e203c0fbfd28ce88508aaa2af81e38136e6dbf6d884b53842eac84ca02206e0edbf92c535273368a4cf8d1da359661972e03e6957ddadeafa1834e727b1f4121022d426ef365d6480b127b4980afa4b9415cad5e6f0a9e11b1536d3523597197f3ffffffff0173270100000000001976a91479d3297d1823149f4ec61df31d19f2fad5390c0288ac00000000
// sendRawTransaction to running BCH node
BITBOX.RawTransactions.sendRawTransaction(hex).then((result) => { console.log(result); }, (err) => { console.log(err); });
// 4238d4319e526b090aced641370366a5b830a796e96ef110783c53ebaa287678
```

Note the call to `RawTransactions.sendRawTransaction` does an actual `POST` to the full node whose credentials we added to `bitbox.js` and returns a `Promise`. If all went well we'll get returned a txid. In this case it was successful and we got [4238d4319e526b090aced641370366a5b830a796e96ef110783c53ebaa287678](https://blockchair.com/bitcoin-cash/transaction/4238d4319e526b090aced641370366a5b830a796e96ef110783c53ebaa287678).

## 1-to-many

Another type of $BCH transaction is a 1-to-many. In the case where both the `vins` and `vouts` of a tx are P2PKH you can send from 1 address to as many as 2936 before you hit the wall of 100kb per tx.

In our example we're going to send from 1-to-5. Close your BITBOX console via control^c and reopen it w/ `bitbox console  --environment production`. Notice that we're recreating the `bip44BCHAccount` HDNode via the `xpriv` instead of via a mnemonic.

```js
let xpriv = 'xprv9ycRUKofpapUBBdkNbBrLAZodKr4Q7LhgmcLppwS7yicaGuEQ9egswHEsgJj8sjsxgab79GMkmVXRNSJVT8Dsp5Q8MBmAe5ELN1yUbP9Vb1';

// HDNode of BIP44 account
let bip44BCHAccount = BITBOX.HDNode.fromXPriv(xpriv);

// derive the first external change address HDNode which is going to spend utxo
let changeAddressNode0 = BITBOX.HDNode.derivePath(bip44BCHAccount, "0/0");

// instance of transaction builder
let transactionBuilder = new BITBOX.TransactionBuilder('bitcoincash');

// txid of vin
let txid = 'c2aa3c36c77c836f99b73210994d86067db49977920a0e49e04041bac137923d';

// original amount of satoshis in vin
let originalAmount = 15221;

// add input with txid and index of vin
transactionBuilder.addInput(txid, 0);
```

Now we need to update the call to `getByteCount` to reflect that we're sending to 5 addresses. We also want to do a `for` loop where we derive siblings of `changeAddressNode0`. We then get each sibling's cash address and send it 1/5th of our original amount minus the tx fee.

```js
// get byte count to calculate fee. paying 1 sat/byte
let byteCount = BITBOX.BitcoinCash.getByteCount({ P2PKH: 1 }, { P2PKH: 5 });

// amount to send to receiver. It's the original amount - 1 sat/byte for tx size
let sendAmount = originalAmount - byteCount;

// add output w/ address and amount to send
for(let i = 0; i < 5; i++) {
  let childNode = BITBOX.HDNode.derivePath(bip44BCHAccount, `0/${i+1}`);
  let cashAddress = BITBOX.HDNode.toCashAddress(childNode);
  transactionBuilder.addOutput(cashAddress, Math.floor(sendAmount / 5));
}

// keypair
let keyPair = BITBOX.HDNode.toKeyPair(changeAddressNode0);

// sign w/ HDNode's keyPair
let redeemScript;
transactionBuilder.sign(0, keyPair, redeemScript, transactionBuilder.hashTypes.SIGHASH_ALL, originalAmount);

// build tx
let tx = transactionBuilder.build();

// output rawhex
let hex = tx.toHex();
// 02000000015e5280280f8bc76bc477c6b630538a9de1238f6e614fb5fc8d2b50b0cf6386f4010000006a473044022063136391ad2bc25f256038a11b4816ca6e601887eeb96c9fe93874e8688f70e90220709ce5a856c68d8bf293fd1e49bd4027f841cde4d43b2c2fc2795d0a107743844121024e61dcf27b337780b14dcf5ae5976a16bb37ac1caa901c9cc0db9b822924cef0ffffffff0acf050000000000001976a914fa5c1df9cf0f19e994a13a595bd02f8d49ebf13788accf050000000000001976a914bcd90726a2d5d6a05c3098f9bfc723792ad8a00588accf050000000000001976a91438e3fb157f1aeec35640a3b323db427acadf065c88accf050000000000001976a9147839b1ed73725f6e82dc04cdf1d0ff1e434066d888accf050000000000001976a91489608c1629cfb2603dce15b84b793bf73387403e88accf050000000000001976a914c5eefec653b511bcf68409d9bb697b6996719ab988accf050000000000001976a914a83acc43ea289229d1698655a39bc8a88c69385588accf050000000000001976a9144e422cf666727cfff8fd698cfe968f15b350053788accf050000000000001976a914f44a0a98ed70b191c4443d05811ecf25238ae7d888accf050000000000001976a914015c1b97fe120ada76efd8cd31f04f51d838acd288ac00000000

BITBOX.RawTransactions.sendRawTransaction(hex).then((result) => { console.log(result); }, (err) => { console.log(err); });
// 72eb365f0f8d692edb5aa8b0d32948c21a80938b8f2870b68a5d58cefdabd513
```

Again we can see this was successful and `sendRawTransaction` returned a txid. In this case [72eb365f0f8d692edb5aa8b0d32948c21a80938b8f2870b68a5d58cefdabd513](https://blockchair.com/bitcoin-cash/transaction/72eb365f0f8d692edb5aa8b0d32948c21a80938b8f2870b68a5d58cefdabd513).

## many-to-many

Now that we've sent satoshis from 1 to 5 addresses we want to send from 5-to-5. Exit your console w/ control^c and restart it w/ `bitbox console --environment production`. Again we're recreating the `bip44BCHAccount` node from the `xpriv`.

```js
let xpriv = 'xprv9ycRUKofpapUBBdkNbBrLAZodKr4Q7LhgmcLppwS7yicaGuEQ9egswHEsgJj8sjsxgab79GMkmVXRNSJVT8Dsp5Q8MBmAe5ELN1yUbP9Vb1';

// HDNode of BIP44 account
let bip44BCHAccount = BITBOX.HDNode.fromXPriv(xpriv);

// instance of transaction builder
let transactionBuilder = new BITBOX.TransactionBuilder('bitcoincash');

// txid of vin
let txid = '72eb365f0f8d692edb5aa8b0d32948c21a80938b8f2870b68a5d58cefdabd513';

// add input with txid and index of vin
for(let i = 0; i < 5; i++) {
  transactionBuilder.addInput(txid, i);
}

let originalAmount = 14890;
```

We also have to update the call to `getByteCount` to reflect that we're sending from 5 to 5.

```js
// get byte count to calculate fee. paying 1 sat/byte
let byteCount = BITBOX.BitcoinCash.getByteCount({ P2PKH: 5 }, { P2PKH: 5 });

// amount to send to receiver. It's the original amount - 1 sat/byte for tx size
let sendAmount = originalAmount - byteCount;

// add output w/ address and amount to send
for(let i = 5; i < 10; i++) {
  let childNode = BITBOX.HDNode.derivePath(bip44BCHAccount, `0/${i+1}`);
  let cashAddress = BITBOX.HDNode.toCashAddress(childNode);
  transactionBuilder.addOutput(cashAddress, Math.floor(sendAmount / 5));
}

let redeemScript;
for(let i = 0; i < 5; i++) {
  // sign w/ HDNode's keyPair
  let childNode = BITBOX.HDNode.derivePath(bip44BCHAccount, `0/${i+1}`);
  transactionBuilder.sign(i, BITBOX.HDNode.toKeyPair(childNode), redeemScript, transactionBuilder.hashTypes.SIGHASH_ALL, 2978);
}

// build tx
let tx = transactionBuilder.build();
// output rawhex
let hex = tx.toHex();
// 020000000513d5abfdce585d8ab670288f8b93801ac24829d3b0a85adb2e698d0f5f36eb72000000006b483045022100c80969a9b9333ca6223ff75186e99a7e98ee8a7950fad8f54aee9e2e6160b60102204758d8db0562cc8d9832f702dc4d29dfd0cc9f3e4081d47154e391b8fb9aad54412102de80682cd27e513fc57f86d5323761422dc8b59cc5d2ba3903c928b533532734ffffffff13d5abfdce585d8ab670288f8b93801ac24829d3b0a85adb2e698d0f5f36eb72010000006a4730440220280d4a9954c5afe24089bdd545466bd7a8caad8b295e30de9d3cb5e56fccf64e022036663b2c53b5fac674b4b935b53e2a4ea88dfc71c9b879870976d82887542ab4412102969479fa9bea3082697dce683ac05b13ae63016b41d5ca1a450ad40f6c543751ffffffff13d5abfdce585d8ab670288f8b93801ac24829d3b0a85adb2e698d0f5f36eb72020000006b483045022100ba2c3b717e023966cb16df65ca83f77029e2a5b80c47c47b6956474ac9ff281302201d48ee3292439e284a6654a0e79ac2b8f7fff5c6b0d715260aa296501a239c6441210259a3f18d4982fe1afc9e11ee4b376b42297e1581932e643d384314b1605ab15bffffffff13d5abfdce585d8ab670288f8b93801ac24829d3b0a85adb2e698d0f5f36eb72030000006b483045022100ee602988d358468404cb5aad86520419b54a9a3d72d7b93c8319bdd13cd115b002202a35ea83fb453eb2e90ceb636ddc8e0b6f6d6c8a30318f0921991410522fb5d44121039b3ff05f3db13a71bb394da22c545ca2a0eb6bebc687d2adeee9ab5b118f6da1ffffffff13d5abfdce585d8ab670288f8b93801ac24829d3b0a85adb2e698d0f5f36eb72040000006b483045022100fbdca1650e49b7a2e9ac355742169533bf894f69d1b67879baf502fa996e9c1002202b4825441e146f5bd10d1a45798ab2d8e8ab0b062b2d9e432d4cf36887d8d844412103be9b4460171a2495c1bfcdd4a42191c4d7242c0a280c68e51b8355fda41c6988ffffffff05ea0a0000000000001976a914c4dd01fe9359ee69c18cfd17a99c0fdf932c46d188acea0a0000000000001976a9140b7088dd957fcfbbc2c0d5e0f08ea4e69c5df18788acea0a0000000000001976a914738172d921de96c57e771edf7fd33d63e3a415c288acea0a0000000000001976a914dd6a28961399731111afee0e28ea80e218c3387288acea0a0000000000001976a9147df8e9ea7ed021cc400a361f04933d3428290ab988ac00000000

BITBOX.RawTransactions.sendRawTransaction(hex).then((result) => { console.log(result); }, (err) => { console.log(err); });
// 67d32ac84d78caa7eaaf8bab6197a66a0945625a282595e75622c33aaccfa1b9
```

This was successful and `sendRawTransaction` returned a txid. In this case [67d32ac84d78caa7eaaf8bab6197a66a0945625a282595e75622c33aaccfa1b9](https://blockchair.com/bitcoin-cash/transaction/67d32ac84d78caa7eaaf8bab6197a66a0945625a282595e75622c33aaccfa1b9).

## many-to-1

Finally we want to consolidate the 5 newly created utxo from the previous step back in to a single address. For that we'll again exit our BITBOX console w/ control^c and reopen it w/ `bitbox console  --environment production`.

```js
let xpriv = 'xprv9ycRUKofpapUBBdkNbBrLAZodKr4Q7LhgmcLppwS7yicaGuEQ9egswHEsgJj8sjsxgab79GMkmVXRNSJVT8Dsp5Q8MBmAe5ELN1yUbP9Vb1';

// HDNode of BIP44 account
let bip44BCHAccount = BITBOX.HDNode.fromXPriv(xpriv);

// instance of transaction builder
let transactionBuilder = new BITBOX.TransactionBuilder('bitcoincash');

// txid of vin
let txid = '67d32ac84d78caa7eaaf8bab6197a66a0945625a282595e75622c33aaccfa1b9';
```

Now we need to update the call to `getByteCount` to have 5 vins and 1 vout and subtract it's result from the originalAmount for the fee. In a many-to-1 tx the `originalAmount` is the sum of the `vins`.

```js
let byteCount = BITBOX.BitcoinCash.getByteCount({ P2PKH: 5 }, { P2PKH: 1 });

let vinAmount = 2794;

let originalAmount = vinAmount * 5;

let amount = originalAmount - byteCount;

transactionBuilder.addOutput('bitcoincash:qpuax2tarq33f86wccwlx8ge7tad2wgvqgjqlwshpw', amount);
```

Next we need to do another `for` loop and derive the `i`th sibling HDNode which we sent the satoshis to in the 1-to-many tx.

We have to finish adding all the inputs before we can sign the tx which is why we have two successive `for` loops.

```js
// add input with txid and index of vin
for(let i = 0; i < 5; i++) {
  transactionBuilder.addInput(txid, i);
}

let redeemScript;
for(let i = 5; i < 10; i++) {
  // sign w/ HDNode's keypair
  let node = BITBOX.HDNode.derivePath(bip44BCHAccount, `0/${i+1}`);
  let keyPair = BITBOX.HDNode.toKeyPair(node);

  transactionBuilder.sign(i-5, keyPair, redeemScript, transactionBuilder.hashTypes.SIGHASH_ALL, vinAmount);
}
```

Now sign, build and send the raw hex to the $BCH network.

```js
// build tx
let tx = transactionBuilder.build();
// output rawhex
let hex = tx.toHex();
// 020000000a7c5c2a6711da26d97c8f00facd5c21f516f3e5cba3d0071d48941e96d9aebadc000000006b483045022100bb01adf71e4a3e8fe35256f87198c368646da2d2ccbb1e3e79abef3e657a95590220407ab69ca6899ce117a5c19a53231ca0a4c1934046c7092dc747d720feb27d6841210356d2f623aeaac4e8b03a79fd3bd0daff0dde04a633e27bba272d00763c85e624ffffffff7c5c2a6711da26d97c8f00facd5c21f516f3e5cba3d0071d48941e96d9aebadc010000006b48304502210097a184bc8bc66ad4502b34237dc591531bd78d26a8c3f401b5760c4df80347f0022011b4354cb02a62442ecdf21fd9e81de2a18a4dbcbdcd554c4343a9616fe5ed334121031c181414441c18141567d03ddca2cbc6594aad7ca570b1e31eb8b18b92026e80ffffffff7c5c2a6711da26d97c8f00facd5c21f516f3e5cba3d0071d48941e96d9aebadc020000006a47304402206cc790156041c1a76961149f860189f344a0059678b65edb6c243722c64b362a022070e3531e2c14cc4f8fbc45d392a73233d6f9ee254b5c09543a518f5a02735e374121037b5e3041ac872adb29580d9f0c573f452f0f0e78780d80193b86e515ba76d67cffffffff7c5c2a6711da26d97c8f00facd5c21f516f3e5cba3d0071d48941e96d9aebadc030000006a473044022059bc2a0851a0c30b7cecd9828f856b16947d70a3ebe75693996ff4972bec983b022069fe4630a0564fca9a4c70b6c619930dc86a8f5e4f1df7ea236dc51b930049224121034c148d91b046f71618344ca27912d2f90d18e5d7e6f8e36de8b3509d383a0e30ffffffff7c5c2a6711da26d97c8f00facd5c21f516f3e5cba3d0071d48941e96d9aebadc040000006a473044022069a5316bee13caf57f4f0d3f3b2eefd9a1f21c0d346234fd066cf0d1c2d1e36002200a80609efbd3d48ddeb85a91eb5a09b4101636177ff233ef5c4d44bf2b6ddba84121032658d2f1a21d956d10a75e6c145098f90f4769caa69527633d232c1254e143cfffffffff7c5c2a6711da26d97c8f00facd5c21f516f3e5cba3d0071d48941e96d9aebadc050000006a47304402205b90e07c8374312906123d7c2d392afbb3247bc028508ad00b5a547a345bf7d502204f6a1db6236f63573b10fcc33bafe298a8ecc0e8bbd9ae6d4156191a618314254121021ac6c44f198477d44c1544f9adac4b397b4697cf00778e51f448cbabbaf27761ffffffff7c5c2a6711da26d97c8f00facd5c21f516f3e5cba3d0071d48941e96d9aebadc060000006b48304502210096644f34edd4d673f964916c4bbc6b18fc4de9a37fd22489899dd8a7f6352d880220137611bb458b8f2dfc950db002242c4ee08fa2f7f760494d61cac20e52864443412102963beefaf2231bba12a4e4340c6d87314a9485bce1a6c3df73a6b3ad64157519ffffffff7c5c2a6711da26d97c8f00facd5c21f516f3e5cba3d0071d48941e96d9aebadc070000006a473044022054d7f60de750de8d242f3379ff4e7bfbb8e8ef476e44f12c8abf7aefe76d611b0220291e08d838e95901d1f6793e827a65c803cbffceb4671afd4d6e94c73b925760412102e7a01e6d04d1cd69d5850d6d2ca3b66dfca33efaa88ec25b4ae0e05e5f8ffc52ffffffff7c5c2a6711da26d97c8f00facd5c21f516f3e5cba3d0071d48941e96d9aebadc080000006a473044022045b069c430e904a2cf4105594ba2cdcddb6510e16332610c6fde7fd1d9cf18f102201174acd68022d5970db0c876f387b39afd2f9ac28c02a0b5ced92b9a800f07484121037652b17ea5edb1e7751d76b7507373862168f725f65c1ce81c723ae5b8665524ffffffff7c5c2a6711da26d97c8f00facd5c21f516f3e5cba3d0071d48941e96d9aebadc090000006a47304402201132689bce414a05fa992d0bd2103a0c3921bf35b429f6e32b81a199b7ae3e1e022009892366a4b64e8e3e1416db02a2786b18d1affde9e8b1c6861763cfb1eee81f4121025c87b65e739def6453728c2cc9e652d16daca7bea1970a5b1488a2b79fcb7c67ffffffff0122340000000000001976a91479d3297d1823149f4ec61df31d19f2fad5390c0288ac00000000

BITBOX.RawTransactions.sendRawTransaction(hex).then((result) => { console.log(result); }, (err) => { console.log(err); });
// 40a265bcf7fac883568eb50a394887e653eeaa8446699ca0134a7a5dc1f5ee15
```

As you can see `sendRawTransaction` returned a `txid` which means it was successful. [40a265bcf7fac883568eb50a394887e653eeaa8446699ca0134a7a5dc1f5ee15](https://blockchair.com/bitcoin-cash/transaction/40a265bcf7fac883568eb50a394887e653eeaa8446699ca0134a7a5dc1f5ee15).

## OP_RETURN

The `OP_RETURN` OP code allows you to write arbitrary data to the blockchain. It's been limited to 80 bytes but on May 15th the $BCH network will upgrade to support 220 bytes.

Let's write `#BCHForEveryone` to the blockchain.

```js
// instance of transaction builder
let transactionBuilder = new BITBOX.TransactionBuilder('bitcoincash')

// amount of satoshis at vout
let originalAmount = 7651;

// txid
let txid = "40e974d91a269b333adcb6d3ca7ac871681262ab84170b7e741141193013bad0";

// add input txid, vout index and amount of satoshis
transactionBuilder.addInput(txid, 1)

// encode #BCHForEveryone as a buffer
let buf = new Buffer('#BCHForEveryone');

// encode w/ OP_RETURN
let data = BITBOX.Script.encode([
  BITBOX.Script.opcodes.OP_RETURN,
  buf
])

// get size of tx to calculate fee
let byteCount = BITBOX.BitcoinCash.getByteCount({ P2PKH: 1 }, { P2PKH: 2 });

// calculate fee @ 1 sat/B
let sendAmount = originalAmount - byteCount;

// add cash address output
transactionBuilder.addOutput("bitcoincash:qpuax2tarq33f86wccwlx8ge7tad2wgvqgjqlwshpw", sendAmount)

// add OP_RETURN and data as output w/ 0 satoshis
transactionBuilder.addOutput(data, 0)

// xpriv
let xpriv = 'xprv9ycRUKofpapUBBdkNbBrLAZodKr4Q7LhgmcLppwS7yicaGuEQ9egswHEsgJj8sjsxgab79GMkmVXRNSJVT8Dsp5Q8MBmAe5ELN1yUbP9Vb1';

// instance of bip44BCHAccount node
let node = BITBOX.HDNode.fromXPriv(xpriv);

// first external change address node
let childNode = BITBOX.HDNode.derivePath(node, `0/0`);
let key = BITBOX.HDNode.toKeyPair(childNode);

let redeemScript;
// sign tx
transactionBuilder.sign(0, key, redeemScript, transactionBuilder.hashTypes.SIGHASH_ALL, originalAmount)

// to raw hex
let hex = transactionBuilder.build().toHex()
// 0200000001d0ba1330194111747e0b1784ab62126871c87acad3b6dc3a339b261ad974e940010000006b483045022100a284b4ac5ed55ac0e2baa02b6bdabbf06f98d2bf6b3fdcea1aea50f55766afd002206181c8e60f738116ba6e16177f3d408e8da2c463c69b27c2531fab84b63a63b24121022d426ef365d6480b127b4980afa4b9415cad5e6f0a9e11b1536d3523597197f3ffffffff02011d0000000000001976a91479d3297d1823149f4ec61df31d19f2fad5390c0288ac0000000000000000116a0f23424348466f7245766572796f6e6500000000

// POST to full $BCH node
BITBOX.RawTransactions.sendRawTransaction(hex).then((result) => { console.log(result); }, (err) => { console.log(err); });
// 0f5c2dbde622734d9221afe1a896add4508bfa49045274d3110ab879a2562992
```

`sendRawTransaction` returned a `txid` `0f5c2dbde622734d9221afe1a896add4508bfa49045274d3110ab879a2562992` which means our data was written to the blockchain. You can see it at this unspendable UTXO:  [d-694556d7d579da66fc57a52b06fc7ac6](https://blockchair.com/bitcoin-cash/address/d-694556d7d579da66fc57a52b06fc7ac6).

If you decode the raw hex you'll see `OP_RETURN 23424348466f7245766572796f6e65` which you can decode back to the original string w/ BITBOX.

```js
let fromASM = BITBOX.Script.fromASM("OP_RETURN 23424348466f7245766572796f6e65");
let arr = BITBOX.Script.decode(fromASM)
arr[1].toString('ascii')
// #BCHForEveryone
```

## Summary

If you made it this far you're a champion. You're now ready to change the world and make a fortune.

Please let me know if you have any questions or spot any bugs in the examples. Now go forth and flood the network w/ transactions!
