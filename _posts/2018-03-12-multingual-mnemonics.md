---
layout: default
title:  "Mnemonic Word Lists"
date:   2018-03-12
---

Bitcoin Cash is meant for people all over EARTH. Most of those people don't speak english. That's why today we're releasing wordlists so you can generate mnemonics in 8 different languages to help spread $BCH to people all over the planet.

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

The methods which now accepts a `wordslist` argunent are

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

## Summary

With `BITBOX.BitcoinCash.mnemonicWordLists` it's now easier than ever to generate mnemonics in languages other than english.
