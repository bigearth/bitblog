---
layout: default
title:  "Account Creation"
date:   2018-02-15 06:14:19 -0600
---

Bitcoin Cash wallets are really just collections of key pairs. Hiearchical Deterministic wallets are the state of the art for reliably creating and backing up collections of related keypairs. For more detail read Andreas's in depth explation about how they work in [Mastering Bitcoin chapter 5](https://github.com/bitcoinbook/bitcoinbook/blob/develop/ch05.asciidoc).

BITBOX is an HD wallet which aims to be compliant w/ BIPS 32, 39, 43, 44. When you first fire up BITBOX we create a random mnemonic from 16 bytes of entropy which we then use to create a root seed, master key and 10 default accounts.

## Generate mnemonic

You can configure how much entropy BITBOX uses to create your mnemonic in the config section. More entropy means more words in your mnemonic. By default BITBOX will use 16 bytes and create a 12 word mnemonic.

![Entropy Slider]({{ "/assets/entropy-slider.png" | absolute_url }})


Entropy (bits/bytes) | Mnemonic length (words)
--- | ---
128/16 | 12
160/20 | 15
192/24 | 18
224/28 | 21
256/32 | 24

To do this we're using NodeJS's [`crypto.randomBytes`](https://nodejs.org/api/crypto.html#crypto_crypto_randombytes_size_callback) and the wonderful [BIP39.js](https://github.com/bitcoinjs/bip39).

```javascript
let randomBytes = Crypto.randomBytes(bytes);
let mnemonic = BIP39.entropyToMnemonic(randomBytes);
```

![Mnemonic]({{ "/assets/mnemonic.png" | absolute_url }})

## Root seed

Next we take an optional password and combined w/ the mnemonic create a root seed. The number of potential seeds is 2^512.

![Optional Password`]({{ "/assets/password.png" | absolute_url }})

```javascript
let password = 'l337';
let rootSeed = BIP39.mnemonicToSeed(mnemonic, password);
```

## Master key

Finally we're using [bitcoinlib-js](https://github.com/bitcoinjs/bitcoinjs-lib) to create a masterkey from the rootSeed.

```javascript
let masterkey = Bitcoin.HDNode.fromSeedBuffer(rootSeed);
```

This masterkey can be used to create 4 billion child keys. Each of those child keys can produce 4 billion child keys recursively in a derivation path.

When you combine the number of potential mnemonics, seeds and nested child keys the potential Bitcoin Cash address space is unfathomably large.

## Derivation Path

Due to the huge number of potential derivation paths BIPs 43 and 44 introduced conventions for adding meaning to the path. Specifically

```
m / purpose' / coin_type' / account' / change / address_index
```

`purpose'` is always set to 44 to signify that the wallet is BIP 44 compliant. `coin_type'` is set to 145 for $BCH. Then we create 10 accounts which are siblings and get the first private key in Wallet Import Format.

```javascript
let purpose = "44'";
let coin = "145'";
let path = `m/${purpose}/${coin}`;
let addresses = [];
let account;
for (let i = 0; i < 10; i++) {
  account = masterkey.derivePath(path);
  addresses.push(account.derive(0).keyPair.toWIF());
};
```

![Derivation Path]({{ "/assets/hd-path.png" | absolute_url }})

## Display to user

We don't want to display the private key WIF by default to the user because it's unsafe. Instead we get the publicKey and display that by default allowing the user to toggle visibility of the privateKeyWIF w/ a button click.

![Private WIF]({{ "/assets/private-wif.png" | absolute_url }})

```javascript
let publicKey = Bitcoin.ECPair.fromWIF(privKeyWIF).getAddress();
```

![Base58Check]({{ "/assets/base58check.png" | absolute_url }})

## CashAddr

`publicKey` from the previous step is Base58Check encoded. You can toggle displaying the address in [CashAddr](https://www.bitcoinabc.org/cashaddr) via [bchaddr.js](https://github.com/bitcoincashjs/bchaddrjs).

```javascript
bchaddr.toCashAddress(publicKey)
```

![CashAddr]({{ "/assets/cashaddr-public.png" | absolute_url }})

## Testnet

You can also toggle generating keys on the $BCH testnet.

![Testnet private]({{ "/assets/testnet-wif.png" | absolute_url }})

![Testnet public]({{ "/assets/testnet-public.png" | absolute_url }})

## Summary

BITBOX aims to be an HD wallet which is compliant w/ BIPs 32, 39, 43 and 44. You can change the strength of the mnemonic, how many accounts are created, the derivation path, whether the addresses are displayed as Base58Check or CashAddr encoded and main or testnet.

## More info

* [BIP 32](https://github.com/bitcoin/bips/blob/master/bip-0032.mediawiki)
* [BIP 39](https://github.com/bitcoin/bips/blob/master/bip-0039.mediawiki)
* [BIP 43](https://github.com/bitcoin/bips/blob/master/bip-0043.mediawiki)
* [BIP 44](https://github.com/bitcoin/bips/blob/master/bip-0044.mediawiki)
