---
layout: default
title:  "Complete setup"
date:   2018-04-01
---

So you want to send $BCH transactions? Worthy goal. Follow this guide and soon the transactions will flow like the champaign of the Illinois countryside.

## Installation

First things first. You're gonna need BITBOX. For that you'll need NodeJS and `npm`.

### NodeJS

NodeJS is a javascript runtime build on Chrome's V8 engine. Install it from [their homepage](https://nodejs.org).

### npm

`npm` is the package manager for NodeJS. Also install it from [it's homepage](https://www.npmjs.com).

### BITBOX

Once those are both installed you're ready to install BITBOX.

```
npm install bitbox-cli --global
```

This will install the `bitbox-cli` command line utility and give you `bitbox new`, `bitbox console` and `bitbox scaffold`

## Create a new app

Now BITBOX is installed and you can have it scaffold out an app.

```
bitbox new --title myApp

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
Creating test/ directory: ./myApp/tests
Creating bitbox.js configuration file
All done. ✅
Go get em! Remember--with great power comes great responsibility. 🚀
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
      password: ""
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
      password: ""
    },
    production: {
      protocol: "http",
      host: "ip.address.of.node",
      port: "8332",
      username: "l337",
      password: "h4x0r"
    }
  }
};
```

Great work. You've now installed NodeJS, `npm`, `bitbox-cli`, set up a full $BCH node and stubbed out an empty BITBOX app. The next step is to open a console.

## Console

BITBOX ships w/ a custom NodeJS repl that has entire BITBOX API built in. To use it run the following:

```
bitbox console --environment production
> BITBOX.Mnemonic.generateMnemonic(256);
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
let mnemonic = 'fresh base moral lunch jacket later shallow verify coffee answer gospel memory lawn economy cover legal slam help giggle enroll basic series essay during';
```

### BIP44 Account and addresses

Now we need a [BIP44](https://github.com/bitcoin/bips/blob/master/bip-0044.mediawiki) account. BIP44 derivation paths follow a certain format.

```
m / purpose' / coin_type' / account' / change / address_index
```

We'll be deriving the first hardened account or `m / 44' / 145' / 0'` and we'll cycle through it's `n`th external change address `0 / n`.


```js
// root seed buffer
let rootSeedBuffer = BITBOX.Mnemonic.mnemonicToSeedBuffer(mnemonic);
// master HDNode
let masterHDNode = BITBOX.HDNode.fromSeedBuffer(rootSeedBuffer, 'bitcoincash');
// HDNode of BIP44 account
let bip44BCHAccount0 = masterHDNode.derivePath("m/44'/145'/0'");
// derive the first external change address HDNode which is going to spend utxo
let changeAddressNode0 = bip44BCHAccount0.derivePath("0/0");
// get the cash address
let changeAddress0 = BITBOX.HDNode.toCashAddress(changeAddressNode0);
// bitcoincash:qqf038kcskv3ghd4ceyjvqkd24vjahmgmva3kvgu5p
```

### Prepare the utxo

In this demo I'm going to do 1-to-1, 1-to-many and many-to-1 P2PKH SIGHASH_ALL $BCH transactions. For that we need to set up 2 unspent transaction outputs.

Take the `changeAddress0` from the previous step and send it 2 small $BCH transactions. I created 2 utxo of 15370 satoshis or around $0.10 each.

* [0df2f582eb8954069f63be8f6a82d1572f9b1141296ac246a5470009235f6a35](https://blockchair.com/bitcoin-cash/transaction/0df2f582eb8954069f63be8f6a82d1572f9b1141296ac246a5470009235f6a35#o=1)
* [f48663cfb0502b8dfcb54f616e8f23e19d8a5330b6c677c46bc78b0f2880525e](https://blockchair.com/bitcoin-cash/transaction/f48663cfb0502b8dfcb54f616e8f23e19d8a5330b6c677c46bc78b0f2880525e#o=1)

### Live demo

Here is `changeAddress0` [on blockchair](https://blockchair.com/bitcoin-cash/address/qqf038kcskv3ghd4ceyjvqkd24vjahmgmva3kvgu5p) so you can see this address and transactions on the actual $BCH blockchain.

Now everything is set up and we're ready to send our 1st $BCH transaction.

## 1-to-1

The most basic type of $BCH transaction is a 1-to-1 Pay to PubKeyHash (P2PKH) SIGHASH_ALL script. That's a mouthful but basically that just means 1 person sending another person some $BCH. Let's see how that would look w/ BITBOX.

### `addInput`

Building on our previous code examples we need to create an instance of `TransactionBuilder`, get the keyPair of the node which wants to spend the utxo and pass it w/ the `txid` and `vin` of the utxo to `addInput`.

```js
// instance of transaction builder
let transactionBuilder = new BITBOX.TransactionBuilder('bitcoincash');
// keypair
let keyPair = changeAddressNode0.keyPair;
// txid of vin
let txid = '0df2f582eb8954069f63be8f6a82d1572f9b1141296ac246a5470009235f6a35';
// add input with txid and index of vin
transactionBuilder.addInput(txid, 1, keyPair);
```

### `addOutput`

Next we need to calculate the byte count and subtract that number from the original number of satoshis to get a fee of 1 sat/B and add the receive address and send amount to `addOutput`.

```js
// get byte count to calculate fee. paying 1 sat/byte
let byteCount = BITBOX.BitcoinCash.getByteCount({ P2PKH: 1 }, { P2PKH: 1 });
// original amount of satoshis in vin
let originalAmount = 15370;
// amount to send to receiver. It's the original amount - 1 sat/byte for tx size
let sendAmount = originalAmount - byteCount;
// add output w/ address and amount to send
transactionBuilder.addOutput('bitcoincash:qpuax2tarq33f86wccwlx8ge7tad2wgvqgjqlwshpw', sendAmount);
```

### `sign` and send to the network

Our transaction is almost ready to roll. Now we need to sign and build it. Then convert it to hex and send it to the $BCH network.

```js
// sign w/ HDNode
transactionBuilder.sign(0, originalAmount);
// build tx
let tx = transactionBuilder.build();
// output rawhex
let hex = tx.toHex();
// 0200000001356a5f23090047a546c26a2941119b2f57d1826a8fbe639f065489eb82f5f20d010000006b483045022100f796d251e8e30f84495d5123e73f30d88d84ae9daf20bab2a5cad5a53e1f5c4d022022111e426f577137d02b3b4a5012bf1a1939fc2fd8607d36d9c11e2b7951224d4121024e61dcf27b337780b14dcf5ae5976a16bb37ac1caa901c9cc0db9b822924cef0ffffffff014a3b0000000000001976a91479d3297d1823149f4ec61df31d19f2fad5390c0288ac00000000
// sendRawTransaction to running BCH node
BITBOX.RawTransactions.sendRawTransaction(hex).then((result) => { console.log(result); }, (err) => { console.log(err); });
// 450d3ce6b70f2f356061f8a2d3ee6a47bbc5b703e7c2452d57f21a6550c2f502
```

Note the call to `RawTransactions.sendRawTransaction` does an actual `POST` to the full node whose credentials we added to `bitbox.js`. If all went well we'll get returned a txid. In this case it was successful and we got [450d3ce6b70f2f356061f8a2d3ee6a47bbc5b703e7c2452d57f21a6550c2f502](https://blockchair.com/bitcoin-cash/transaction/450d3ce6b70f2f356061f8a2d3ee6a47bbc5b703e7c2452d57f21a6550c2f502).

## 1-to-many

Another type of $BCH transaction is a 1-to-many. In the case where both the `vins` and `vouts` of a tx are P2PKH you can send from 1 address to as many as 2936 before you hit the wall of 1kb per tx.

In our example we're going to send from 1-to-10. Close your BITBOX console via control^c and reopen it w/ `bitbox console  --environment production`. Paste in the same code as the previous example until we calculate the byteCount and add the outputs. Note that I updated the txid to the 2nd transaction we originally created.

```js
// mnemonic
let mnemonic = 'fresh base moral lunch jacket later shallow verify coffee answer gospel memory lawn economy cover legal slam help giggle enroll basic series essay during';
// root seed buffer
let rootSeedBuffer = BITBOX.Mnemonic.mnemonicToSeedBuffer(mnemonic);
// master HDNode
let masterHDNode = BITBOX.HDNode.fromSeedBuffer(rootSeedBuffer, 'bitcoincash');
// HDNode of BIP44 account
let bip44BCHAccount0 = masterHDNode.derivePath("m/44'/145'/0'");
// derive the first external change address HDNode which is going to spend utxo
let changeAddressNode0 = bip44BCHAccount0.derivePath("0/0");
// instance of transaction builder
let transactionBuilder = new BITBOX.TransactionBuilder('bitcoincash');
// keypair
let keyPair = changeAddressNode0.keyPair;
// txid of vin
let txid = 'f48663cfb0502b8dfcb54f616e8f23e19d8a5330b6c677c46bc78b0f2880525e';
// add input with txid and index of vin
transactionBuilder.addInput(txid, 1, keyPair);
```

Now we need to update the call to `getByteCount` to reflect that we're sending to 10 addresses. We also want to do a `for` loop where we derive a sibling of our `changeAddressNode0`. We then get that sibling's cash address and send it 1/10th of our original amount minus the tx fee.

```js
// get byte count to calculate fee. paying 1 sat/byte
let byteCount = BITBOX.BitcoinCash.getByteCount({ P2PKH: 1 }, { P2PKH: 10 });
// original amount of satoshis in vin
let originalAmount = 15370;
// amount to send to receiver. It's the original amount - 1 sat/byte for tx size
let sendAmount = originalAmount - byteCount;
// add output w/ address and amount to send
for(let i = 0; i < 10; i++) {
  let childNode = masterHDNode.derivePath(`m/44'/145'/0'/0/${i+1}`);
  let cashAddress = BITBOX.HDNode.toCashAddress(childNode);
  transactionBuilder.addOutput(cashAddress, Math.floor(sendAmount / 10));
}
// sign w/ HDNode's keyPair
transactionBuilder.sign(0, originalAmount);
// build tx
let tx = transactionBuilder.build();
// output rawhex
let hex = tx.toHex();
// 02000000015e5280280f8bc76bc477c6b630538a9de1238f6e614fb5fc8d2b50b0cf6386f4010000006a473044022063136391ad2bc25f256038a11b4816ca6e601887eeb96c9fe93874e8688f70e90220709ce5a856c68d8bf293fd1e49bd4027f841cde4d43b2c2fc2795d0a107743844121024e61dcf27b337780b14dcf5ae5976a16bb37ac1caa901c9cc0db9b822924cef0ffffffff0acf050000000000001976a914fa5c1df9cf0f19e994a13a595bd02f8d49ebf13788accf050000000000001976a914bcd90726a2d5d6a05c3098f9bfc723792ad8a00588accf050000000000001976a91438e3fb157f1aeec35640a3b323db427acadf065c88accf050000000000001976a9147839b1ed73725f6e82dc04cdf1d0ff1e434066d888accf050000000000001976a91489608c1629cfb2603dce15b84b793bf73387403e88accf050000000000001976a914c5eefec653b511bcf68409d9bb697b6996719ab988accf050000000000001976a914a83acc43ea289229d1698655a39bc8a88c69385588accf050000000000001976a9144e422cf666727cfff8fd698cfe968f15b350053788accf050000000000001976a914f44a0a98ed70b191c4443d05811ecf25238ae7d888accf050000000000001976a914015c1b97fe120ada76efd8cd31f04f51d838acd288ac00000000
BITBOX.RawTransactions.sendRawTransaction(hex).then((result) => { console.log(result); }, (err) => { console.log(err); });
// dcbaaed9961e94481d07d0a3cbe5f316f5215ccdfa008f7cd926da11672a5c7c
```

Again we can see this was successful and `sendRawTransaction` returned a txid. In this case [dcbaaed9961e94481d07d0a3cbe5f316f5215ccdfa008f7cd926da11672a5c7c](https://blockchair.com/bitcoin-cash/transaction/dcbaaed9961e94481d07d0a3cbe5f316f5215ccdfa008f7cd926da11672a5c7c).

## many-to-1

Finally we want to consolidate the 10 newly created utxo from the previous step back in to a single address. For that we'll again exit our BITBOX console w/ control^c and reopen it w/ `bitbox console  --environment production`. Paste in the same code as the previous 2 examples until we add outputs and inputs. Note that I've updated the `txid` to that which was returned from our 2nd transaction.

```js
// mnemonic
let mnemonic = 'fresh base moral lunch jacket later shallow verify coffee answer gospel memory lawn economy cover legal slam help giggle enroll basic series essay during';
// root seed buffer
let rootSeedBuffer = BITBOX.Mnemonic.mnemonicToSeedBuffer(mnemonic);
// master HDNode
let masterHDNode = BITBOX.HDNode.fromSeedBuffer(rootSeedBuffer, 'bitcoincash');
// HDNode of BIP44 account
let bip44BCHAccount0 = masterHDNode.derivePath("m/44'/145'/0'");
// derive the first external change address HDNode which is going to spend utxo
let changeAddressNode0 = bip44BCHAccount0.derivePath("0/0");
// instance of transaction builder
let transactionBuilder = new BITBOX.TransactionBuilder('bitcoincash');
// keypair
let keyPair = changeAddressNode0.keyPair;
// txid of vin
let txid = 'dcbaaed9961e94481d07d0a3cbe5f316f5215ccdfa008f7cd926da11672a5c7c';
```

Now we need to update the call to `getByteCount` to have 10 vins and 1 vout and subtract it's result from the originalAmount for the fee. In a many-to-1 tx the `originalAmount` is the sum of the `vins`.

```js
let byteCount = BITBOX.BitcoinCash.getByteCount({ P2PKH: 10 }, { P2PKH: 1 });

let vinAmount = 1487;

let originalAmount = vinAmount * 10;

let amount = originalAmount - byteCount;

transactionBuilder.addOutput('bitcoincash:qpuax2tarq33f86wccwlx8ge7tad2wgvqgjqlwshpw', amount);
```

Next we need to do another `for` loop but this time we're going to derive the `i`th sibling HDNode which we sent the satoshis to in the 1-to-many tx.

We have to finish adding all the inputs before we can sign the tx which is why we have two successive `for` loops.

```js
// add input with txid and index of vin
for(let i = 0; i < 10; i++) {
  let node = masterHDNode.derivePath(`m/44'/145'/0'/0/${i+1}`);
  let keyPair = node.keyPair;
  transactionBuilder.addInput(txid, i, keyPair);
}

for(let i = 0; i < 10; i++) {
  // sign w/ HDNode's keypair
  transactionBuilder.sign(i, vinAmount);
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
// f28856f19e19750bd216ba2a2f49e01f52b1ea752cf5e17d18b819fbc5e81fee
```

As you can see `sendRawTransaction` returned a `txid` which means it was successful. [f28856f19e19750bd216ba2a2f49e01f52b1ea752cf5e17d18b819fbc5e81fee](https://blockchair.com/bitcoin-cash/transaction/f28856f19e19750bd216ba2a2f49e01f52b1ea752cf5e17d18b819fbc5e81fee).

## Summary

If you made it this far you're a champion. You're now ready to change the world and make a fortune.

Please let me know if you have any questions or spot any bugs in the examples. Now go forth and flood the network w/ transactions!
