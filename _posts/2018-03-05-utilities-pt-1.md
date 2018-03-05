---
layout: default
title:  "Utilities part 1"
date:   2018-03-05
---

`bitbox-cli` recently added 2 new Objects full of utility methods to help you accelerate your $BCH workflow. The <code>BitcoinCash</code> methods convert between satoshis and whole units. They also convert legacy addresses to cashaddr and reverse as well as detecting address formats, types and networks.

The <code>Crypto</code> methods let you create sha256 and ripemd160 hashes as well as generate random bytes. Together the <code>BitcoinCash</code> and <code>Crypto</code> utilities let you quickly build out a great $BCH application.

## <code>BitcoinCash</code>

```js
BITBOX.BitcoinCash.toSatoshi(9)
// 900000000

BITBOX.BitcoinCash.toBitcoinCash(900000000)
// 9

BITBOX.BitcoinCash.toLegacyAddress('bitcoincash:qzm47qz5ue99y9yl4aca7jnz7dwgdenl85jkfx3znl')
// 1HiaTupadqQN66Tvgt7QSE5Wg13BUy25eN

BITBOX.BitcoinCash.toCashAddress('1HiaTupadqQN66Tvgt7QSE5Wg13BUy25eN')
// bitcoincash:qzm47qz5ue99y9yl4aca7jnz7dwgdenl85jkfx3znl

BITBOX.BitcoinCash.isLegacyAddress('bitcoincash:qqfx3wcg8ts09mt5l3zey06wenapyfqq2qrcyj5x0s')
// false

BITBOX.BitcoinCash.isCashAddress('bitcoincash:qqfx3wcg8ts09mt5l3zey06wenapyfqq2qrcyj5x0s')
// true

BITBOX.BitcoinCash.isMainnetAddress('bitcoincash:qqfx3wcg8ts09mt5l3zey06wenapyfqq2qrcyj5x0s')
// true

BITBOX.BitcoinCash.isTestnetAddress('bitcoincash:qqfx3wcg8ts09mt5l3zey06wenapyfqq2qrcyj5x0s')
//false

BITBOX.BitcoinCash.isP2PKHAddress('bitcoincash:qqfx3wcg8ts09mt5l3zey06wenapyfqq2qrcyj5x0s')
// true

BITBOX.BitcoinCash.isP2SHAddress('bitcoincash:qqfx3wcg8ts09mt5l3zey06wenapyfqq2qrcyj5x0s')
// false

BITBOX.BitcoinCash.detectAddressFormat('bitcoincash:qqfx3wcg8ts09mt5l3zey06wenapyfqq2qrcyj5x0s')
// cashaddr

BITBOX.BitcoinCash.detectAddressNetwork('bitcoincash:qqfx3wcg8ts09mt5l3zey06wenapyfqq2qrcyj5x0s')
// mainnet

BITBOX.BitcoinCash.detectAddressType('bitcoincash:qqfx3wcg8ts09mt5l3zey06wenapyfqq2qrcyj5x0s');
// p2pkh
```

[More Info](https://www.bitbox.earth/bitboxcli#bitcoinCash)

## <code>Crypto</code>

```js
let data = 'EARTH';
BITBOX.Crypto.createHash(data, 'sha256')
// bcfee25a8baf6808fce5ff4e63cf21c8d114853ca7eacdcc3c210d73c58dab66

BITBOX.Crypto.createSHA256Hash(data)
// bcfee25a8baf6808fce5ff4e63cf21c8d114853ca7eacdcc3c210d73c58dab66

BITBOX.Crypto.createRIPEMD160Hash(data)
// ca700bba3bd37304b9bd923652245f598ece8afe

BITBOX.Crypto.randomBytes(32)
// 6e1453357f6f99d19d2a6554f35eab65b6c27f6572e31d7f2faa696cac57759b
```

[More Info](https://www.bitbox.earth/bitboxcli#crypto)
## Credits

`bitbox-cli` leverages several really great libraries. Please show these people support.

* https://nodejs.org/api/crypto.html
* https://github.com/bitcoinjs/bitcoinjs-lib
* https://github.com/bitcoinjs/bip39
* https://github.com/bitcoincashjs/bchaddrjs
* https://github.com/dawsbot/satoshi-bitcoin
