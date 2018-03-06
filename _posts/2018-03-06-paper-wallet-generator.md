---
layout: default
title:  "Paper Wallet Generator"
date:   2018-03-05
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

You can send $BCH to the public address and it will be safely stored offline. Later you can use a wallet such as [Bitcoin.com's iOS wallet](https://itunes.apple.com/us/app/bitcoin-wallet-by-bitcoin-com/id1252903728?mt=8) to scap the private key WIF and sweep the coins to that wallet for spending.

### Options

Pass in a flag for `--encoding` to change from the default `cashaddr` to `legacy`. Also pass in a flag for `--network` to change from the default `mainnet` to `testnet` if you're the type of person who backs up their testnet coins.

```
> bitbox paper --encoding legacy --network testnet

Creating legacy paper wallet on testnet
```

![Mnemonic]({{ "/assets/paper-wallet-legacy-testnet.png" | absolute_url }})

## Summary

Creating paper wallets to securely store your Bitcoin Cash offline is now easier than every w/ `bitbox paper`.
