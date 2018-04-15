---
layout: default
title:  "MemoBCH Utility Methods"
date:   2018-04-15
---

The pace of innovation in the Bitcoin Cash space is truly amazing. Each week we see new services launch which truly show the strength of the Blockchain. Yesterday [MemoBCH](https://memo.cash/) launched which is a "Decentralized on-chain social network built on Bitcoin Cash."

While I think the site is cool IMO the real innovation is [the protocol](https://memo.cash/protocol). They're leveraging `OP_RETURN` to write data to the blockchain and prefixing it w/ one of 7 prefixes to fadd sematic meaning. This is a really clever idea and w/ `OP_RETURN` going from 80 bytes to 220 bytes on the upcoming May 15th Bitcoin Cash upgrade this idea will only get better.


## Writing Utility Methods

BITBOX can quickly encode `OP_RETURN` scripts so I decided to write some helper methods to quickly encode data and prefix it per the MemoBCH protocol. This will accelerate how quickly people can write MemoBCH data to the blockchain

```js
let setName = (name) => {
  let script = [BITBOX.Script.opcodes.OP_RETURN, Buffer.from('6d01', 'hex'), Buffer.from(name)];
  return BITBOX.Script.compile(script)
}

let postMemo = (memo) => {
  let script = [BITBOX.Script.opcodes.OP_RETURN, Buffer.from('6d02', 'hex'), Buffer.from(memo)];
  return BITBOX.Script.compile(script)
}

let like = (txHash) => {
  let script = [BITBOX.Script.opcodes.OP_RETURN, Buffer.from('6d04', 'hex'), Buffer.from(txHash)];
  return BITBOX.Script.compile(script)
}

let follow = (address) => {
  let script = [BITBOX.Script.opcodes.OP_RETURN, Buffer.from('6d06', 'hex'), Buffer.from(address)];
  return BITBOX.Script.compile(script)
}

let unfollow = (address) => {
  let script = [BITBOX.Script.opcodes.OP_RETURN, Buffer.from('6d07', 'hex'), Buffer.from(address)];
  return BITBOX.Script.compile(script)
}
```

### Examples

#### set name

Prefix: `6d01`

Data: satoshi

OP_RETURN: `OP_RETURN 6d01 7361746f736869`

[https://blockchair.com/bitcoin-cash/address/d-863b50cc9d6f20a43cf65bad298bdcca](https://blockchair.com/bitcoin-cash/address/d-863b50cc9d6f20a43cf65bad298bdcca)

#### post memo

Prefix: `6d02`

Data: MemoBCH protocol FTW!

OP_RETURN: `OP_RETURN 6d02 4d656d6f4243482070726f746f636f6c2046545721`

[https://blockchair.com/bitcoin-cash/address/d-433a972749493aa5f15ad08b38627e6a](https://blockchair.com/bitcoin-cash/address/d-433a972749493aa5f15ad08b38627e6a)

#### like

Prefix: `6d04`

Data: 8966169557a6e65b3a1ab1e4d2caf6c199000f557acb7e0bec6c516de1668f6f

OP_RETURN: `OP_RETURN 6d04 38393636313639353537613665363562336131616231653464326361663663313939303030663535376163623765306265633663353136646531363638663666`

[https://blockchair.com/bitcoin-cash/address/d-614beec4231967e9f3fb145537c01467](https://blockchair.com/bitcoin-cash/address/d-614beec4231967e9f3fb145537c01467)

#### follow

Prefix: `6d06`

Data: 1F5GrRezwGokQhxmF4fYaBbbNrPPaeBqMm

OP_RETURN: `OP_RETURN 6d06 314635477252657a77476f6b5168786d46346659614262624e725050616542714d6d`

[https://blockchair.com/bitcoin-cash/address/d-9cde8776aae709f5983616f50332c0b8](https://blockchair.com/bitcoin-cash/address/d-9cde8776aae709f5983616f50332c0b8)

#### unfollow

Prefix: `6d07`

Data: 1F5GrRezwGokQhxmF4fYaBbbNrPPaeBqMm

OP_RETURN: `OP_RETURN 6d07 314635477252657a77476f6b5168786d46346659614262624e725050616542714d6d`

[https://blockchair.com/bitcoin-cash/address/d-ba9cdd8ad24c41593c36ac5da7ea5c42](https://blockchair.com/bitcoin-cash/address/d-ba9cdd8ad24c41593c36ac5da7ea5c42)

## Reading Utility Methods

You can also read transaction which follow the MemoBCH protocol using BITBOX. Here's an example

```js
BITBOX.RawTransactions.getRawTransaction('ade7aa70f0cf99281ff844284a2f6c3d63d1630a3d160bac9ba487d3896ee377').then((result) => {
  BITBOX.RawTransactions.decodeRawTransaction(result).then((result) => {
    let res = JSON.parse(result);
    // need to know index of vout w/ OP_RETURN data
    let asm = res.vout[1].scriptPubKey.asm;
    // OP_RETURN 0x6d01 746573742032
    let fromASM = BITBOX.Script.fromASM(asm);
    let decodedArr = BITBOX.Script.decode(fromASM);

    // prefix
    let memoBCHType = decodedArr[1];
    memoBCHType.toString('hex');
    // 6d01

    // message
    let message = decodedArr[2];
    message.toString('ascii');
    // satoshi

  }, (err) => { console.log(err); });
}, (err) => { console.log(err); });
```

## Summary

Innovation is alive and well on the Bitcoin Cash chain. Weekly we're seeing new services which push the boundaries of what is possible w/ blockchain technology. BITBOX hopes to enable developers to accelerate how quickly they can do amazing things w/ Bitcoin Cash.
