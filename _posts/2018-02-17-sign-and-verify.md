---
layout: default
title:  "Sign & Verify"
date:   2018-02-17
---

BITBOX enables you to cryptographically prove ownership of an address via signing messages. You can also verify that ownershup of an address from another person.

If, for example, you lost access to an account you could prove you were the owner that had been sending payments to that account by signing a specific message from that address.

Or someone could potentially give you a loan or credit using a signature from a wallet holding a certain amount of funds as proof that you could repay them.

## Sign a message

You can sign a message w/ any address in your BITBOX. It doesn't matter if it's in cashaddr or legacy encoding. Just paste the address into the address field and type the message that you want to sign. Then click the 'Sign' button and if you have entered a valid address that has keys in your BITBOX you'll see a signature appear.

![Converter]({{ "/assets/verify-cashaddr.png" | absolute_url }})

You can share this message, address and signature w/ anyone and they can verify that you are the owner of that address.

## Verify a message

You can verify a message from any address on the Bitcoin Cash network. Just paste in the message, address and signature provided and click the 'Verify' button. If the address and signature are both valid format and the signature matches the message and the address you'll get a green 'Valid message' response.

![Converter]({{ "/assets/verify-legacy.png" | absolute_url }})

If the address or signature are incorrectly formatted or the signature doesn't match the message and address you'll get an error message.

![Converter]({{ "/assets/verify-fail.png" | absolute_url }})

## Using the command line

BITBOX supports the entire Bitcoin Cash RPC. [`bitbox-cli`](https://www.npmjs.com/package/bitbox-cli) now supports `signmessage`, `signmessagewithprivkey` and `verifymessage` in cashaddr (w/ or w/out the prefix) and legacy.

```
> BITBOX.signmessage("bitcoincash:qrqxcnj3z6rsg4k6lcshsh3a4lftxa03aqyy258tm7", "BCH FTW")
> H1K8O9ho7qhDmNpzxKoi98RAKpvOiC/jXx4eow1GT0JaR2QipI2B2JOvOEp++EaPuLcjWsbEsgoV6jSrknb17j4=
```

```
> BITBOX.verifymessage("bitcoincash:qrqxcnj3z6rsg4k6lcshsh3a4lftxa03aqyy258tm7", "H1K8O9ho7qhDmNpzxKoi98RAKpvOiC/jXx4eow1GT0JaR2QipI2B2JOvOEp++EaPuLcjWsbEsgoV6jSrknb17j4=", "BCH FTW")
> true
```

```
> BITBOX.signmessagewithprivkey("KwM8QgcNy2aLnicbQYjXMJUMWYPvLW8ZD86ZTqCaQ8pQmQoS6YY4", "Winning")
> IJ/A0vEIy+RdZ1DRVwlkVWh2ggTJu612DhW0sgj88w4iCfc+1BP1Ib/3ZZYT6YAUyWuG8c8/wlV+uwE0s1kUE8s=
```
