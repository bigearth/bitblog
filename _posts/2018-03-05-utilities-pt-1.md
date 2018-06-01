---
layout: default
title:  "Utilities part 1"
date:   2018-03-05
---

`bitbox-cli` recently added 3 new Objects full of utility methods to help you accelerate your $BCH workflow. The <code>BitcoinCash</code> methods convert between satoshis and whole units.

The <code>Address</code> methods let you convert legacy addresses to cashaddr and reverse as well as detecting address formats, types and networks.

The <code>Crypto</code> methods let you create sha256 and ripemd160 hashes as well as generate random bytes. Together the <code>BitcoinCash</code> and <code>Crypto</code> utilities let you quickly build out a great $BCH application.

## <code>BitcoinCash</code>

```js
BITBOX.BitcoinCash.toSatoshi(9)
// 900000000

BITBOX.BitcoinCash.toBitcoinCash(900000000)
// 9
```

[More Info](https://www.bitbox.earth/bitboxcli/bitcoincash)

## <code>Address</code>

```js
BITBOX.Address.toLegacyAddress('bitcoincash:qzm47qz5ue99y9yl4aca7jnz7dwgdenl85jkfx3znl')
// 1HiaTupadqQN66Tvgt7QSE5Wg13BUy25eN

BITBOX.Address.toCashAddress('1HiaTupadqQN66Tvgt7QSE5Wg13BUy25eN')
// bitcoincash:qzm47qz5ue99y9yl4aca7jnz7dwgdenl85jkfx3znl

BITBOX.Address.isLegacyAddress('bitcoincash:qqfx3wcg8ts09mt5l3zey06wenapyfqq2qrcyj5x0s')
// false

BITBOX.Address.isCashAddress('bitcoincash:qqfx3wcg8ts09mt5l3zey06wenapyfqq2qrcyj5x0s')
// true

BITBOX.Address.isMainnetAddress('bitcoincash:qqfx3wcg8ts09mt5l3zey06wenapyfqq2qrcyj5x0s')
// true

BITBOX.Address.isTestnetAddress('bitcoincash:qqfx3wcg8ts09mt5l3zey06wenapyfqq2qrcyj5x0s')
//false

BITBOX.Address.isP2PKHAddress('bitcoincash:qqfx3wcg8ts09mt5l3zey06wenapyfqq2qrcyj5x0s')
// true

BITBOX.Address.isP2SHAddress('bitcoincash:qqfx3wcg8ts09mt5l3zey06wenapyfqq2qrcyj5x0s')
// false

BITBOX.Address.detectAddressFormat('bitcoincash:qqfx3wcg8ts09mt5l3zey06wenapyfqq2qrcyj5x0s')
// cashaddr

BITBOX.Address.detectAddressNetwork('bitcoincash:qqfx3wcg8ts09mt5l3zey06wenapyfqq2qrcyj5x0s')
// mainnet

BITBOX.Address.detectAddressType('bitcoincash:qqfx3wcg8ts09mt5l3zey06wenapyfqq2qrcyj5x0s');
// p2pkh
```

[More Info](https://www.bitbox.earth/bitboxcli/address)

## <code>Crypto</code>

```js
let buffer = Buffer.from('0101010101010101', 'hex')
BITBOX.Crypto.sha256(buffer)
// <Buffer 04 ab c8 82 1a 06 e5 a3 09 37 96 7d 11 ad 10 22 1c b5 ac 3b 52 73 e4 34 f1 28 4e e8 71 29 a0 61>

let buffer = Buffer.from('0101010101010101', 'hex')
BITBOX.Crypto.ripemd160(buffer)
// <Buffer 58 25 70 1b 4b 97 67 fd 35 06 3b 28 6d ca 35 82 85 3e 06 30>

let buffer = Buffer.from('0101010101010101', 'hex')
BITBOX.Crypto.hash256(buffer)
// <Buffer 72 83 38 d9 9f 35 61 75 c4 94 5e f5 cc cf a6 1b 7b 56 14 3c bb f4 26 dd d0 e0 fc 7c fe 8c 3c 23>

let buffer = Buffer.from('0101010101010101', 'hex')
BITBOX.Crypto.hash160(buffer)
// <Buffer ab af 11 19 f8 3e 38 42 10 fe 8e 22 2e ac 76 e2 f0 da 39 dc>

BITBOX.Crypto.randomBytes(16)
// <Buffer 8c 5e 76 62 2f 4c bf cc 07 db b2 4f ea ea 1e d6>
```

[More Info](https://www.bitbox.earth/bitboxcli/crypto)

## Credits

`bitbox-cli` leverages several really great libraries. Please show these people support.

* [https://nodejs.org/api/crypto.html](https://nodejs.org/api/crypto.html)
* [https://github.com/bitcoinjs/bitcoinjs-lib](https://github.com/bitcoinjs/bitcoinjs-lib)
* [https://github.com/bitcoinjs/bip39](https://github.com/bitcoinjs/bip39)
* [https://github.com/bitcoincashjs/bchaddrjs](https://github.com/bitcoincashjs/bchaddrjs)
* [https://github.com/dawsbot/satoshi-bitcoin](https://github.com/dawsbot/satoshi-bitcoin)
