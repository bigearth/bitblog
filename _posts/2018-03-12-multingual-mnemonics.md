---
layout: default
title:  "Mnemonic Word Lists"
date:   2018-03-12
---

Bitcoin Cash is meant for people all over EARTH. Most of those people don't speak english. That's why today we're releasing `mnemonicWordLists` so you can generate mnemonics in 8 different languages to help spread $BCH to people all over the planet.

We're also releasing `translateMnemonic` to make it quick and easy to translate your mnemonics between any of the 8 supported languages.

## `mnemonicWordLists`

`BITBOX.BitcoinCash` now has a `mnemonicWordLists` method which returns an object w/ the following keys:

```js
{
  chinese_simplified: [],
  chinese_traditional: [],
  english: [],
  french: [],
  italian: [],
  japanese: [],
  korean: [],
  spanish: []
}
```

Each of these keys contains an array w/ 2048 words in that language. 4 `BITBOX.BitcoinCash` methods now accept that wordlist as their 2nd argument and will create and validate mnemonics in those languages.

The methods which now accepts a `wordslist` argument are

* `generateMnemonic`
* `entropyToMnemonic`
* `mnemonicToEntropy`
* `validateMnemonic`

### Examples

#### Chinese simplified

```js
BITBOX.BitcoinCash.generateMnemonic(128, BITBOX.BitcoinCash.mnemonicWordLists().chinese_simplified);
// 南 英 钉 油 冷 馏 扶 搬 特 规 波 顺
```

#### Chinese traditional

```js
BITBOX.BitcoinCash.generateMnemonic(128, BITBOX.BitcoinCash.mnemonicWordLists().chinese_traditional);
// 蒸 融 陣 默 甲 蓋 躺 靈 原 富 恆 份
```

#### French

```js
BITBOX.BitcoinCash.generateMnemonic(128, BITBOX.BitcoinCash.mnemonicWordLists().french);
// annonce ampleur sanglier peser acheter cultiver abroger embellir résoudre dialogue grappin lanterne
```

#### Italian

```js
BITBOX.BitcoinCash.generateMnemonic(128, BITBOX.BitcoinCash.mnemonicWordLists().italian);
// raschiato comodo petalo lira ipotesi mondina scettro ritmico bacino abrasivo attrito eletto
```

#### Japanese

```js
BITBOX.BitcoinCash.generateMnemonic(128, BITBOX.BitcoinCash.mnemonicWordLists().japanese);
// かいが　こける　つねづね　けおりもの　けむり　せんろ　しゃくほう　けんみん　あわせる　ひつぎ　みてい　たいない
```

#### Korean

```js
BITBOX.BitcoinCash.generateMnemonic(128, BITBOX.BitcoinCash.mnemonicWordLists().korean);
// 회색 제공 적성 만일 당장 확인 사람 화장 숫자 여군 대도시 하순
```

#### Spanish

```js
BITBOX.BitcoinCash.generateMnemonic(128, BITBOX.BitcoinCash.mnemonicWordLists().spanish);
// combate hundir trauma edad élite medio grave pie aduana donar pimienta bodega
```

## `translateMnemonic`

BITBOX enables you to translate your mnemonics between any of the 8 supported languages.

```js
// create korean mnemonic
let koreanMnemonic = BITBOX.BitcoinCash.generateMnemonic(256, BITBOX.BitcoinCash.mnemonicWordLists().korean);
// 상대 조직 피곤 기간 장면 저런 서쪽 신고 연예인 고춧가루 활짝 세종대왕 거울 대충 벨트 제일 저곳 남녀 수술 수학 학원 금년 유학 인공
// translate it to spanish
let spanishMnemonic = BITBOX.BitcoinCash.translateMnemonic(koreanMnemonic, 'korean', 'spanish')
// gato razón torero bobina pintor poema grieta leer mirar aparato vivaz hembra alambre cielo esencia rabia poder buscar incapaz instante trofeo bicho oído pájaro
// translate back to korean
BITBOX.BitcoinCash.translateMnemonic(spanishMnemonic, 'spanish', 'korean')
// 상대 조직 피곤 기간 장면 저런 서쪽 신고 연예인 고춧가루 활짝 세종대왕 거울 대충 벨트 제일 저곳 남녀 수술 수학 학원 금년 유학 인공
```

## `bitbox paper`

`bitbox paper` has also been updated to now accept a `--language` flag which can set the mnemonic to any of the 8 languages listed above. The default is English.

![Korean Paper Wallet]({{ "/assets/paper-korean.png" | absolute_url }})

## BITBOX GUI

BITBOX GUI has also been updated to generate mnemonics in all of the above languages. There is now a language selector on the configuration screen. Select the language that you'd like to use and restart your BITBOX.

![]({{ "/assets/language-select.png" | absolute_url }})
![]({{ "/assets/chinese_simplified.png" | absolute_url }})
![]({{ "/assets/chinese_traditional.png" | absolute_url }})
![]({{ "/assets/french.png" | absolute_url }})
![]({{ "/assets/italian.png" | absolute_url }})
![]({{ "/assets/japanese.png" | absolute_url }})
![]({{ "/assets/korean.png" | absolute_url }})
![]({{ "/assets/spa.png" | absolute_url }})

## Summary

With `mnemonicWordLists` it's now easier than ever to generate mnemonics in languages other than english to help spread $BCH all over the EARTH.

[More info](https://www.bitbox.earth/bitboxcli/bitcoincash)
