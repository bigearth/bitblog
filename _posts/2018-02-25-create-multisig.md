---
layout: default
title:  "Multiple Environments"
date:   2018-02-25
---

Bitcoin Cash allows you to create multisignature addresses involving multiple people. Then for the funds to be spend a certain number of those people must agree. For example you can create a multisignature address between 3 people. Then set the threshold where 2 of those people have to sign a transaction to spend the funds. This introduces a level of security and accountability which has many implications for how groups can handle money.

BITBOX lets you create multisignature Bitcoin Cash addresses in 2 ways.

## `addmultisigaddress`

`addmultisigaddress` takes two arguments. The first is the number of required signatures which must be met in order for any funds sent to this address to then be spent. The second is an array of addresses.

The addresses can be legacy base58Check or cashaddr encoded w/ or w/out the prefix or hex encoded public keys.

So for example to create a 2 of 3 $BCH multisig address from a hex encoded public key, cashaddr address w/ no prefix and legacy base58check address.

```
>  BITBOX.addmultisigaddress(2, ["0346db96eff101f088fe6618929fefa7e8455ef9f2b949e3187c88bf827dc7a533", "qr2zmxsg9etcxcnanfll4a2eklder8m82qyp42ymvm", "1Q2AYcnwSy1eSNfW9G6rYzxLa5kxkXB76A"])

> 3Kyj2Z4Q1NXvZZR7vJS2DMueCXDXgbqrdW
```

## `createmultisig`

BITBOX also supports `createmultisig`. It has the same API as `addmultisigaddress` but it returns the redeem script as well as the address.

```
> BITBOX.createmultisig(2, ["bitcoincash:qz5d0jgf4x5ay5hlqa0s5rg2u4z2swt33qszjkrydk", "bitcoincash:qqw0wasdkty6v8zgnrduaxa4mtcn5kuw5qyfspy6hn", "bitcoincash:qqxapkw0xv5tjn7ugudl6yr8hf567f2vzu2t0e9qnt"]).then((result) => { console.log('result', result); })

> {
    address: '3NtK3eEa8GUxvwcujCygmrS3hxuYQo4GTf',
    redeemScript: '522102dee2a62a4b0993c20c548a63dc7613b744eb988dd997c68adc4cfde387a6693021038fdfde0b8cdc7a53951be2f57043ab80f10b36e42f007805920c71fd553aff4d21028e27dd0f73dfb00fc4a1bb5d1a96bdfcaa96dfa66b129c23f88ead3fc884e36f53ae'
}
```

  That `redeemScript` decodes to

  ```js
  {
	"result": {
		"asm": "2 02dee2a62a4b0993c20c548a63dc7613b744eb988dd997c68adc4cfde387a66930 038fdfde0b8cdc7a53951be2f57043ab80f10b36e42f007805920c71fd553aff4d 028e27dd0f73dfb00fc4a1bb5d1a96bdfcaa96dfa66b129c23f88ead3fc884e36f 3 OP_CHECKMULTISIG",
		"reqSigs": 2,
		"type": "multisig",
		"addresses": [
			"bitcoincash:qz5d0jgf4x5ay5hlqa0s5rg2u4z2swt33qszjkrydk",
			"bitcoincash:qqw0wasdkty6v8zgnrduaxa4mtcn5kuw5qyfspy6hn",
			"bitcoincash:qqxapkw0xv5tjn7ugudl6yr8hf567f2vzu2t0e9qnt"
		],
		"p2sh": "3NtK3eEa8GUxvwcujCygmrS3hxuYQo4GTf"
	},
	"error": null,
	"id": null
}
```

## Summary

BITBOX lets you leverage Bitcoin Cash's multisig addresses in 2 ways. These method calls will work against your local BITBOX or remote production nodes.
