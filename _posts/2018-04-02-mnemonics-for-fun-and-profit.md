---
layout: default
title:  "Mnemonics for fun and profit"
date:   2018-04-02
---

Pssst. You want mnemonics? Oh yea we got mnemonics. We got all kinds of mnemonics.

Mnemonics are human readable phrases which can be used to create Bitcoin Cash keypairs for signing transactions, for generating addresses and for signing/verifying messages. Mnemonics can be created from sources of randomness and are much more secure than having a user think of a phrase or password.

BITBOX offers a `Mnemonic` class w/ a suite of methods for generating and validating [BIP39 mnemonics](https://github.com/bitcoin/bips/blob/master/bip-0039.mediawiki) in 8 languages including English, Chinese (traditional/simplified), Korean, Japanese, Spanish, Italian and French. Let's dig in.

## Generating mnemonics

Remember that mnemonics are a human readabe representation of underlying randomness or entropy. This entropy can be from 16 bytes up to 32 bytes. The lower the amount of entropy the less strong and secure your mnemonic will be.

Depending on the number of bytes of entropy you use mnemonics can be in phrases of 12, 15, 18, 21 and 24 words for 128/16, 160/20, 192/24, 224/28 and 256/32 bits/bytes respectively.

### generate

The most straightforward way to create a mnemonic is `generate`. It takes as it's 1st argument the number of bytes to use and the 2nd argument is an optional wordlist. By default BITBOX will create english mnemonics.

#### wordLists

A quick sidenote about `wordLists`. As mentioned above BITBOX supports 8 languages for mnemonics. Any methods in the `Mnemonic` class which take a 2nd optional wordlists argument will get their 2048 wordlist from `wordLists` which returns an object w keys for each supported language.

```js
BITBOX.Mnemonic.wordLists();
 // {
 //   chinese_simplified: [...],
 //   chinese_traditional: [...],
 //   english: [...],
 //   french: [...],
 //   italian: [...],
 //   japanese: [...],
 //   korean: [...],
 //   spanish: [...]
 // }

BITBOX.Mnemonic.wordLists().french;
// [ 'abaisser',
//   'abandon',
//   'abdiquer',
//   'abeille',
//   'abolir',
//   'aborder',
//   'aboutir',
//   'aboyer',
//   'abrasif',
//   ...
```

[More info on wordLists](https://www.bitbox.earth/bitboxcli/mnemonic#wordLists)

Now lets generate some mnemonics in different lengths and languages.

```js
// generate 12 english word mnemonic
BITBOX.Mnemonic.generate(128);
// boil lonely casino manage habit where total glory muffin name limit mansion

// generate 15 english word mnemonic
BITBOX.Mnemonic.generate(160);
// steak prevent estate save dance design close noise cheap season among train sleep ketchup gas

// generate 18 french word mnemonic
BITBOX.Mnemonic.generate(192, BITBOX.Mnemonic.wordLists().french);
// germe tissu avide raconter griffure fureur vivipare brusque famille tituber cuillère burin rythme moulin éolien banquier lubie remuer

// generate 21 spanish word mnemonic
BITBOX.Mnemonic.generate(224, BITBOX.Mnemonic.wordLists().spanish);
// broma jabón caballo mostrar joroba cordón serie cumbre infiel dardo nido fijo mafia balanza índice leyenda altivo resina rapaz vereda cómodo

// generate korean mnemonic from 256 bits of entropy
BITBOX.Mnemonic.generate(256, BITBOX.Mnemonic.wordLists().korean);
// 일기 궁극적 고집 명절 습기 과학 졸음 의논 냉동 모금 남대문 신문 불행 학술 불교 만족 형편 일기 형제 앨범 갑자기 연결 길이 눈썹

// generate japanese mnemonic from 128 bits of entropy
BITBOX.Mnemonic.generate(128, BITBOX.Mnemonic.wordLists().japanese);
// ひほう　まわり　むぎちゃ　つける　すはだ　かなざわし　ことがら　ふっかつ　くうかん　すいえい　しゃおん　れんさい

// generate a chinese simplified mnemonic from 192 bits of entropy
BITBOX.Mnemonic.generate(128, BITBOX.Mnemonic.wordLists().chinese_simplified);
// 扭 嘴 挂 乃 赏 限 桂 曰 漏 绅 算 梯

// generate a chinese traditional mnemonic from 160 bits of entropy
BITBOX.Mnemonic.generate(128, BITBOX.Mnemonic.wordLists().chinese_traditional);
// 騰 編 洛 星 傷 盧 影 訊 寸 士 血 壩
```

[More info on generate](https://www.bitbox.earth/bitboxcli/mnemonic#generate)

### fromEntropy

In the previous examples we just told BITBOX how many bits of entropy and in what language we wanted our mnemonic and it handled generating the randomness. But what if we already have a source of enropy which we'd like to turn in to a mnemonic. For that there is `fromEntropy`.

For this demonstration we'll use `BITBOX.Crypto.randomBytes` to get our entropy but your entropy could come from another source.

```js
// create 16 bytes of entropy
let entropy = BITBOX.Crypto.randomBytes(16);
// create 12 english word mnemonic
BITBOX.Mnemonic.fromEntropy(entropy)
// fee wasp nurse help alley roof jump maze hill enroll enlist teach

// create 20 bytes of entropy
let entropy = BITBOX.Crypto.randomBytes(20);
// create chinese traditional mnemonic from 20 bytes of entropy
BITBOX.Mnemonic.fromEntropy(entropy, BITBOX.Mnemonic.wordLists().chinese_traditional)
// 閃 索 衝 研 腔 顯 中 控 牆 織 劉 桃 婚 戰 沫

// create 24 bytes of entropy
let entropy = BITBOX.Crypto.randomBytes(24);
// create 18 french word mnemonic
BITBOX.Mnemonic.fromEntropy(entropy, BITBOX.Mnemonic.wordLists().french)
// phoque aviser matière asticot enfance pyramide bastion pépite myrtille copain flamme débattre lanterne fendoir tailler nomade tiroir école

// create 28 bytes of entropy
let entropy = BITBOX.Crypto.randomBytes(28);
// create korean mnemonic from 28 bytes of entropy
BITBOX.Mnemonic.fromEntropy(entropy, BITBOX.Mnemonic.wordLists().korean)
// 쌍둥이 공원 엉망 방법 방지 빗방울 하늘 식료품 금년 논리 명칭 입학 수화기 언어 제공 주문 자격 입력 순서 조직 통역

// create 32 bytes of entropy
let entropy = BITBOX.Crypto.randomBytes(32);
// create japanese mnemonic from 32 bytes of entropy
BITBOX.Mnemonic.fromEntropy(entropy, BITBOX.Mnemonic.wordLists().japanese)
// ふうふ　ばかり　ともだち　さんせい　うりあげ　ぬんちゃく　さわやか　むりょう　さつたば　だむる　にんよう　ひこく　そとづら　とうきゅう　いさん　すてる　ぬんちゃく　ずひょう　たいちょう　ばしょ　せぼね　めぐまれる　こんすい　にいがた
```

[More info on fromEntropy](https://www.bitbox.earth/bitboxcli/mnemonic#fromEntropy)

### toEntropy

Since mnemonics are created from entropy it make sense that we could turn a mnemonic back to entropy. That's what `toEntropy` is for.

```js
let entropy = '549ef25e358069775e3c4c6ba9652b6f';
let mnemonic = BITBOX.Mnemonic.fromEntropy(Buffer.from(entropy, 'hex'));
BITBOX.Mnemonic.toEntropy(mnemonic)
// <Buffer 54 9e f2 5e 35 80 69 77 5e 3c 4c 6b a9 65 2b 6f>

let entropy = '9d115d4d172dca804052a79dc5f18dedf97a3eff';
let mnemonic = BITBOX.Mnemonic.fromEntropy(Buffer.from(entropy, 'hex'));
BITBOX.Mnemonic.toEntropy(mnemonic)
// <Buffer 9d 11 5d 4d 17 2d ca 80 40 52 a7 9d c5 f1 8d ed f9 7a 3e ff>
```

[More info on toEntropy](https://www.bitbox.earth/bitboxcli/mnemonic#toEntropy)

## validate

Now that we are able to generate mnemonics we can validate that they are the correct length and contain words from the original wordlist from which they were generated w/ `validate`. It takes as it's 1st argument a mnemonic and as it's 2nd argument the wordlist to validate from.

It will also suggest the closest word in the wordlist if an invalid word is supplied. This works in all 8 supported languages

```js
BITBOX.Mnemonic.validate('ea', BITBOX.Mnemonic.wordLists().english);
// ea is not in wordlist, did you mean eager?

BITBOX.Mnemonic.validate('ふう', BITBOX.Mnemonic.wordLists().japanese);
// ふう is not in wordlist, did you mean ふうけい?

BITBOX.Mnemonic.validate('ba', BITBOX.Mnemonic.wordLists().italian)
// ba is not in wordlist, did you mean babele?

// too long
BITBOX.Mnemonic.validate('boil lonely casino manage habit where total glory muffin name limit mansion boil lonely casino manage habit where total glory muffin name limit mansion boil lonely casino manage habit where total glory muffin name limit mansion boil lonely casino manage habit where total glory muffin name limit mansion', BITBOX.Mnemonic.wordLists().english);
// Invalid mnemonic

// too short
BITBOX.Mnemonic.validate('boil lonely casino', BITBOX.Mnemonic.wordLists().english);

// Valid mnemonic
BITBOX.Mnemonic.validate('boil lonely casino manage habit where total glory muffin name limit mansion', BITBOX.Mnemonic.wordLists().english);
```

[More info on validate](https://www.bitbox.earth/bitboxcli/mnemonic#validate)

### findNearestWord

If you don't need to fully validate a mnemonic but would like to find the nearest matching word from the original wordlist you can use `findNearestWord`. It takes as arguments a word and a wordlist and returns the nearest matching word. Of course it works for all 8 supported languages.

```js
// english
let word = 'ab';
let wordlist = BITBOX.Mnemonic.wordLists().english;
BITBOX.Mnemonic.findNearestWord(word, wordlist);
// abandon

// french
let word = 'octu';
let wordlist = BITBOX.Mnemonic.wordLists().french;
BITBOX.Mnemonic.findNearestWord(word, wordlist);
// octupler

// spanish
let word = 'foobaro';
let wordlist = BITBOX.Mnemonic.wordLists().spanish;
BITBOX.Mnemonic.findNearestWord(word, wordlist);
// forro

// italian
let word = 'nv';
let wordlist = BITBOX.Mnemonic.wordLists().italian;
BITBOX.Mnemonic.findNearestWord(word, wordlist);
// neve
```

[More info on findNearestWord](https://www.bitbox.earth/bitboxcli/mnemonic#findNearestWord)

## toSeed

Another way of turning a mnemonic to a root seed is as a [buffer](https://nodejs.org/api/buffer.html) w/ `toSeed`.

```js
// english mnemonic to root seed buffer
BITBOX.Mnemonic.toSeed('boil lonely casino manage habit where total glory muffin name limit mansion', '');
// <Buffer e9 06 23 6a b5 eb ec 8f bf f9 94 88 07 a6 f5 d2 aa 6f 35 e8 bc bc da 99 e2 2f 90 48 32 3c dc 07 55 b7 81 78 2e e1 cc e4 00 07 bc f9 00 59 3e d2 66 7e ... >

// japanese mnemonic to root seed buffer
BITBOX.Mnemonic.toSeed('먹이 비극 이튿날 여건 타자기 시간 캠페인 여고생 세미나 중단 제한 글자', '');
// <Buffer ae 74 b7 bc 43 12 0f bf b6 a7 6b ec 97 0e ee 94 80 6b 81 b6 ce 4a b4 8b e2 5c 2c 0e 1c 96 83 a3 6d 4d bc db de 30 b6 22 a6 cb 97 35 f6 3a bc 6a 7e 35 ... >

// italian mnemonic to root seed buffer
BITBOX.Mnemonic.toSeed('ginepro erigere sgonfiare mugnaio aria rilassato scatenare derapata avvenire stelo governo sbruffone emanato mini misurare cadetto festivo flamenco', '')
// <Buffer 38 9a 60 b6 f2 a2 29 f2 7d 60 9b 0d 77 0a da 9c af 3d fc 26 8e 12 a4 af fa b3 86 b9 1f 7e 59 57 65 81 ae 34 ee 6d 29 ca f3 47 21 b5 d4 4d 1b 9a 3f 5b ... >

// chinese simplified mnemonic to root seed buffer
BITBOX.Mnemonic.toSeed('烈 珠 据 牙 蔡 米 津 熔 贺 祖 逃 怎 鲁 穿 胡 听 近 杯', '')
// <Buffer e1 48 3c 89 8f d5 3a 4d b5 40 8e 17 5d 42 c6 50 d7 ba a1 67 8a c4 3a b9 f1 6f 36 58 ae a5 53 93 fd dd 7d 46 c6 f8 25 34 25 b6 36 3f bf 54 ce 9d b7 72 ... >

// french mnemonic to root seed buffer
BITBOX.Mnemonic.toSeed('incarner pulpe libre acclamer vernir vorace obstacle saluer rocheux officier pinceau ossature pivoter ennuyeux fatal sénateur phrase poney', '')
// <Buffer 0d 8a 77 2a 92 03 a1 bb a9 6a 09 49 0e d2 fd 17 84 fc da e7 20 9d f2 cb 1a a3 1f 6a ea 6f 6f f9 ac 04 dd 86 81 f3 66 c4 9a a9 6f 71 e8 72 89 52 e7 c3 ... >
```

[More info on toSeed](https://www.bitbox.earth/bitboxcli/mnemonic#toSeed)

## toKeypairs

BITBOX offers a helper method for quickly generating keypairs from a mnemonic. It generates the addresses as the `n`th external change address of the 1st account from that mnemonic w/ this derivation path: `m/44’/145’/0’/0/n`.

```js
// 1st create a mnemonic from 32 bytes of random entropy
let entropy = BITBOX.Crypto.randomBytes(32);
let mnemonic = BITBOX.Mnemonic.fromEntropy(entropy);
// symptom owner ridge follow buffalo choose stem depend million jar lemon claw color credit remove model pudding slot fiber west heavy ranch bird wet

// Then call toKeypairs and pass in your mnemonic and how many keypairs you'd like
BITBOX.Mnemonic.toKeypairs(mnemonic, 5)
// [ { privateKeyWIF: 'Kz6b1TszeUGaypUpRCnfD2L17bQSW93o4j3VMpvT5e5BqaF9XkyP',
// address: 'bitcoincash:qp8a4vzfk9kstwsl4ud4ym3z2tckdf7a4gfwkxvtfq' },
// { privateKeyWIF: 'L5ZHQ2BdTQaTq2A8HNsdkHYKPLsfrHgvJyrVxHFFZyN9K3fmeoiG',
// address: 'bitcoincash:qq5nxh27up6hcm0nn36lxtu7n8a7l6jsj52s8dvtex' },
// { privateKeyWIF: 'KwyY3Z7STwbxnmQXe1vVmXhT8Y3W1BJQpRgteRhTWCyvvdro2j33',
// address: 'bitcoincash:qzj9n9jmnmyeqfdc5k65kxta3c7ch0g3wudeyjeg3y' },
// { privateKeyWIF: 'KxMG2mjL8DZQCaoXz8aFw5XYqguKiDHBb16JwDQMGa7ga7kfy9cE',
// address: 'bitcoincash:qrhj0lesz6sn7l4hc5arh5tt8k583ahdaun6mcdjx8' },
// { privateKeyWIF: 'Kz3qqJ8GFSSbDrBqtV7mfhBoDPkSmMKtp7Yk62psDgmRjyU8id8J',
// address: 'bitcoincash:qp8xjllc75c2hgrpjy3f6kegtfqgmn72dqs0y20anv' } ]

// again in w/ a japanese mnemonic
let entropy = BITBOX.Crypto.randomBytes(32);
let mnemonic = BITBOX.Mnemonic.fromEntropy(entropy, BITBOX.Mnemonic.wordLists().japanese);
BITBOX.Mnemonic.toKeypairs(mnemonic, 5)
// ふおん　ふっき　たぶん　めいうん　ゆうめい　とうし　じゅしん　おっと　むのう　はぶらし　りけん　ふきん　じゅんばん　たとえる　こよい　けんお　りせい　らくご　いっせい　いせき　ひいき　じてん　ひてい　むぎちゃ

// [ { privateKeyWIF: 'L17qQgmbX9YgSMPbJKiiWQym2LKwf76fKNeCJnRdp6zQjJNK3MY1',
//     address: 'bitcoincash:qpzf48wla4s3u9nwtpsgkuf3cmwp8a7zasnt6f6w8l' },
//   { privateKeyWIF: 'Kwr65M1aabJf4fY4arALUySgmLpykNmJpLsi6hNPH6qnBXdZ1Xtf',
//     address: 'bitcoincash:qpzj6hcs925umt834hkekesr833l0496wv55awyumj' },
//   { privateKeyWIF: 'L5N5c8Gg6xNMo4KNFjpXiXCJs4eAKDC5etQ4Xtz1fsmKQ4s7h4NU',
//     address: 'bitcoincash:qz42r45avxy9y7pxjqefw5kah9mm3klfecln36d3t7' },
//   { privateKeyWIF: 'L2yDWTnCpZ66m5HQe3jUG3i2VT8xQYt9HgLNYH2CzPGw2U4c8obV',
//     address: 'bitcoincash:qpy8d8cjcp8kmssxn2kldet39ga6xw7smcxwqzsugw' },
//   { privateKeyWIF: 'L2K63GoJcwBCBPKQr1TCDdP44JhFHDVmYJx6CqkjEBq4QAh35JXs',
//     address: 'bitcoincash:qpkrgvqqc6wx90mva4mr76nskau0ydqqkqvrxtvxhl' } ]
```

[More info on toKeypairs](https://www.bitbox.earth/bitboxcli/mnemonic#toKeypairs)


## Summary

Mnemonics generated from strong entropy are the most secure way of generating Bitcoin Cash root seeds, HD Nodes and keypairs. BITBOX's `Mnemonic` class gives you the tools you need to generate and validate mnemonics in 8 languages.
