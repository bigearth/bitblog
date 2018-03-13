---
layout: default
title:  "Paper Wallet Generator"
date:   2018-03-06
---

Paper wallets let you print public and private keys as QR codes on to a piece of paper which can be stored offline. They are a very quick and convenient way to store coins safely (as long as the paper doesn't degrade) and are much cheaper than a hardware wallet while maintaining much of the security.

They are hugely popular and are an often requested feature. `bitbox-cli` now lets you generate $BCH paper wallets on the mainnet or testnet with legacy or cashaddr encoding addresses from a single command.

## Usage

To create a Bitcoin Cash paper wallet just run `bitbox paper` and a paper wallet will be generated at `paper-wallet.html` which you can print. It will show the private key in wallet import format, the public address in legacy or cashaddr encoding as well as the mnemonic, derivation path, encoding format and network.

```
> bitbox paper

Creating cashaddr paper wallet on mainnet

```

![Mnemonic]({{ "/assets/paper-wallet.png" | absolute_url }})

You can send $BCH to the public address and it will be safely stored offline. Later you can use a wallet such as [Bitcoin.com's iOS wallet](https://itunes.apple.com/us/app/bitcoin-wallet-by-bitcoin-com/id1252903728?mt=8) to scan the private key WIF and sweep the coins to that wallet for spending.

### Options

Pass in a flag for `--encoding` to change from the default `cashaddr` to `legacy`. Also pass in a flag for `--network` to change from the default `mainnet` to `testnet` if you're the type of person who backs up their testnet coins.

```
> bitbox paper --encoding legacy --network testnet

Creating legacy paper wallet on testnet
```

![Mnemonic]({{ "/assets/paper-wallet-legacy-testnet.png" | absolute_url }})

## `keypairsFromMnemonic`

`BITBOX.BitcoinCash` also has a utility method called `keypairsFromMnemonic` which takes as arguments a mnemonic and the number of keypairs to generate. It creates the addresses as the `n`th external change address of the first account from that mnemonic w/ this derivation path: `m/44'/145'/0'/0/n`.

This method is useful if you want to handle QR code generation yourself and just need the keypairs.

```js
// First create a mnemonic from 32 bytes of random entropy
let mnemonic = BITBOX.BitcoinCash.entropyToMnemonic(32);
// symptom owner ridge follow buffalo choose stem depend million jar lemon claw color credit remove model pudding slot fiber west heavy ranch bird wet

// Then call `keypairsFromMnemonic` and pass in your mnemonic and how many keypairs you'd like
BITBOX.BitcoinCash.keypairsFromMnemonic(mnemonic, 5);
// it returns the following array.
//
// [ { privateKeyWIF: 'Kz6b1TszeUGaypUpRCnfD2L17bQSW93o4j3VMpvT5e5BqaF9XkyP',
//     address: 'bitcoincash:qp8a4vzfk9kstwsl4ud4ym3z2tckdf7a4gfwkxvtfq' },
//   { privateKeyWIF: 'L5ZHQ2BdTQaTq2A8HNsdkHYKPLsfrHgvJyrVxHFFZyN9K3fmeoiG',
//     address: 'bitcoincash:qq5nxh27up6hcm0nn36lxtu7n8a7l6jsj52s8dvtex' },
//   { privateKeyWIF: 'KwyY3Z7STwbxnmQXe1vVmXhT8Y3W1BJQpRgteRhTWCyvvdro2j33',
//     address: 'bitcoincash:qzj9n9jmnmyeqfdc5k65kxta3c7ch0g3wudeyjeg3y' },
//   { privateKeyWIF: 'KxMG2mjL8DZQCaoXz8aFw5XYqguKiDHBb16JwDQMGa7ga7kfy9cE',
//     address: 'bitcoincash:qrhj0lesz6sn7l4hc5arh5tt8k583ahdaun6mcdjx8' },
//   { privateKeyWIF: 'Kz3qqJ8GFSSbDrBqtV7mfhBoDPkSmMKtp7Yk62psDgmRjyU8id8J',
//     address: 'bitcoincash:qp8xjllc75c2hgrpjy3f6kegtfqgmn72dqs0y20anv' } ]
```

## Summary

Creating paper wallets to securely store your Bitcoin Cash offline is now easier than ever w/ `bitbox paper`. More info about [bitbox-cli](https://www.npmjs.com/package/bitbox-cli).
