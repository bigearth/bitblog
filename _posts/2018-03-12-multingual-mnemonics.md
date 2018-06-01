---
layout: default
title:  "Mnemonic Word Lists"
date:   2018-03-12
---

Bitcoin Cash is meant for people all over EARTH. Most of those people don't speak english. That's why today we're releasing `wordLists` so you can generate mnemonics in 8 different languages to help spread $BCH to people all over the planet.

## `wordLists`

`BITBOX.Mnemonic` now has a `wordLists` method which returns an object w/ the following keys:

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

Each of these keys contains an array w/ 2048 words in that language. 4 `BITBOX.Mnemonic` methods now accept that wordlist as their 2nd argument and will create and validate mnemonics in those languages.

The methods which now accepts a `wordslist` argument are

* `generate`
* `fromEntropy`
* `toEntropy`
* `validate`

### Examples

#### Chinese simplified

```js
BITBOX.Mnemonic.generate(128, BITBOX.Mnemonic.wordLists().chinese_simplified);
// 南 英 钉 油 冷 馏 扶 搬 特 规 波 顺
```

#### Chinese traditional

```js
BITBOX.Mnemonic.generate(128, BITBOX.Mnemonic.wordLists().chinese_traditional);
// 蒸 融 陣 默 甲 蓋 躺 靈 原 富 恆 份
```

#### French

```js
BITBOX.Mnemonic.generate(128, BITBOX.Mnemonic.wordLists().french);
// annonce ampleur sanglier peser acheter cultiver abroger embellir résoudre dialogue grappin lanterne
```

#### Italian

```js
BITBOX.Mnemonic.generate(128, BITBOX.Mnemonic.wordLists().italian);
// raschiato comodo petalo lira ipotesi mondina scettro ritmico bacino abrasivo attrito eletto
```

#### Japanese

```js
BITBOX.Mnemonic.generate(128, BITBOX.Mnemonic.wordLists().japanese);
// かいが　こける　つねづね　けおりもの　けむり　せんろ　しゃくほう　けんみん　あわせる　ひつぎ　みてい　たいない
```

#### Korean

```js
BITBOX.Mnemonic.generate(128, BITBOX.Mnemonic.wordLists().korean);
// 회색 제공 적성 만일 당장 확인 사람 화장 숫자 여군 대도시 하순
```

#### Spanish

```js
BITBOX.Mnemonic.generate(128, BITBOX.Mnemonic.wordLists().spanish);
// combate hundir trauma edad élite medio grave pie aduana donar pimienta bodega
```

## `bitbox paper`

`bitbox paper` has also been updated to now accept a `--language` flag which can set the mnemonic to any of the 8 languages listed above. The default is English.

![Korean Paper Wallet]({{ "/assets/paper-korean.png" | absolute_url }})

## BITBOX GUI

BITBOX GUI has also been updated to generate mnemonics in all of the above languages. There is now a language selector on the configuration screen. Select the language that you'd like to use and restart your BITBOX.

![]({{ "/assets/language-select.png" | absolute_url }})

### Chinese simplified
![]({{ "/assets/chinese-simplified.png" | absolute_url }})

### Chinese traditional
![]({{ "/assets/chinese-traditional.png" | absolute_url }})

### French
![]({{ "/assets/french.png" | absolute_url }})

### Italian
![]({{ "/assets/italian.png" | absolute_url }})

### Japanese
![]({{ "/assets/japanese.png" | absolute_url }})

### Korean
![]({{ "/assets/korean.png" | absolute_url }})

### Spanish
![]({{ "/assets/spanish.png" | absolute_url }})

## Summary

With `wordLists` it's now easier than ever to generate mnemonics in languages other than english to help spread $BCH all over the EARTH.

[More info](https://www.bitbox.earth/bitboxcli/bitcoincash)
