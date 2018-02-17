---
layout: default
title:  "Address Conversion"
date:   2018-02-17
---

When Bitcoin Cash began as a fork of $BTC on Aug 1st 2017 it kept the legacy address format. This ended up causing quite a bit of confusion and resulted in lost funds as users accidentially sent $BCH to $BTC addresses and vice versa.

To fix this [cashaddr](https://github.com/bitcoincashorg/spec/blob/master/cashaddr.md) was introduced. It's a base32 endcoding based on Bech32.

## Conversion Tool

BITBOX recently added it's own conversion tool so you can quickly and easily convert legacy base58Check encoded Bitcoin Cash addresses in to the cashaddr format.

Simply paste in a legacy or cashaddr address and BITBOX will convert it to both formats and generate QR codes.

![Converter]({{ "/assets/converter.png" | absolute_url }})

We're using [bchaddr.js](https://github.com/bitcoincashjs/bchaddrjs) along w/ [qrcode.react](https://github.com/zpao/qrcode.react).

```js
let cashaddr = bchaddr.toCashAddress(value);
let base58Check = bchaddr.toLegacyAddress(value);
let cashaddrQR = <QRCode value={cashaddr} />;
let base58CheckQR = <QRCode value={base58Check} />;
```
