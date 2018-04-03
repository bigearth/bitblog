---
layout: default
title:  "BIP32 HD Wallets"
date:   2018-04-02
---

Hierarchical Deterministic (HD) wallets are the state of the art when it comes to generating, storing and managing Bitcoin Cash keypairs. They were described in [BIP32](https://github.com/bitcoin/bips/blob/master/bip-0032.mediawiki) and BITBOX offers a suite of methods for creating and managing them.

In the BIP32 wiki there is a picture an HD wallet. This post is to show which BITBOX methods map to which parts of that picture.

![Export 1]({{ "/assets/derivation.png" | absolute_url }})

## Entropy

Generating randomness

* [BITBOX.Crypto.randomBytes](https://www.bitbox.earth/bitboxcli/crypto#randomBytesl)

## Mnemonic

Entropy to mnemonic

* [BITBOX.Mnemonic.entropyToMnemonic](https://www.bitbox.earth/bitboxcli/mnemonic#entropyToMnemonic)
* [BITBOX.Mnemonic.generateMnemonic](https://www.bitbox.earth/bitboxcli/mnemonic#generateMnemonic)

## Master Seed

Mnemonic to master seed

* [BITBOX.Mnemonic.mnemonicToSeedHex](https://www.bitbox.earth/bitboxcli/mnemonic#mnemonicToSeedHex)
* [BITBOX.Mnemonic.mnemonicToSeedBuffer](https://www.bitbox.earth/bitboxcli/mnemonic#mnemonicToSeedBuffer)

## Master Node

Master seed to master HD Node

* [BITBOX.HDNode.fromSeedHex](https://www.bitbox.earth/bitboxcli/hdnode#fromSeedHex)
* [BITBOX.HDNode.fromSeedBuffer](https://www.bitbox.earth/bitboxcli/hdnode#fromSeedBuffer)

## Account, Change and Address

HD Node to children accounts and internal/external change/receive addresses

* [BITBOX.HDNode.derivePath](https://www.bitbox.earth/bitboxcli/hdnode#derivePath)
* [BITBOX.HDNode.derive](https://www.bitbox.earth/bitboxcli/hdnode#derive)
* [BITBOX.HDNode.deriveHardened](https://www.bitbox.earth/bitboxcli/hdnode#deriveHardened)
