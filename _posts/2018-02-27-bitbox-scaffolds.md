---
layout: default
title:  "BITBOX Scaffolds"
date:   2018-02-27
---

The goal of BITBOX is to accelerate how quickly $BCH devs can create their apps. W/ that in mind today we're launching Scaffolds. This is a way to bootstrap a Bitcoin Cash application in under a minute.

When creating a new application on any platform too much time is spent setting everything up and configuring it. BITBOX scaffolds do the heavy lifting for you and let you focus on building your app.

## React

To quickly create a [React app w/ BITBOX web bindings](https://github.com/bigearth/bitbox-scaffold-react) follow these steps.

### Setup

1. Download the latest build from [bitbox.earth](https://www.bitbox.earth/) and compare [the checksums](https://github.com/bigearth/keys-n-hashes)
2. Start your local BITBOX
3. [Install `bitbox-cli`](https://www.npmjs.com/package/bitbox-cli) globally
  * `npm install bitbox-cli --global`
4. Scaffold a React app w/ BITBOX web bindings
  * `bitbox new myApp --scaffold react`
5. `cd` into new app
  * `cd myApp/`
6. Install dependencies
  * `npm install`
7. Start the app
  * `npm start`
8. Open a browser to `http://localhost:3000/` and confirm you are seeing a basic BIP44 wallet
9. Win

![Hello BITBOX]({{ "/assets/bip44.png" | absolute_url }})

## Angular

To quickly create an [Angular app w/ BITBOX web bindings](https://github.com/bigearth/bitbox-scaffold-angular) follow these steps.

### Setup

1. Download the latest build from [bitbox.earth](https://www.bitbox.earth/) and compare [the checksums](https://github.com/bigearth/keys-n-hashes)
2. Start your local BITBOX
3. [Install `bitbox-cli`](https://www.npmjs.com/package/bitbox-cli) globally
  * `npm install bitbox-cli --global`
4. Scaffold an Angular app w/ BITBOX web bindings
  * `bitbox new myApp --scaffold angular`
5. `cd` into new app
  * `cd myApp/`
6. Install dependencies
  * `npm install`
7. Start the app
  * `npm start`
8. Open a browser to `http://localhost:4200/` and confirm you are seeing a basic BIP44 wallet
9. Win

![Hello BITBOX]({{ "/assets/bip44.png" | absolute_url }})

## Node

To quickly create a [Node JS app w/ BITBOX bindings](https://github.com/bigearth/bitbox-scaffold-node) follow these steps.

### Setup

1. Download the latest build from [bitbox.earth](https://www.bitbox.earth/) and compare [the checksums](https://github.com/bigearth/keys-n-hashes)
2. Start your local BITBOX
3. [Install `bitbox-cli`](https://www.npmjs.com/package/bitbox-cli) globally
  * `npm install bitbox-cli --global`
4. Scaffold a NodeJS app w/ BITBOX bindings
  * `bitbox new myApp --scaffold node`
5. `cd` into new app
  * `cd myApp/`
6. Install dependencies
  * `npm install`
7. Start the app
  * `npm start`
8. Confirm you are seeing a basic BIP44 wallet
9. Win

![Hello BITBOX]({{ "/assets/nodebip44.png" | absolute_url }})

## Config

By default your new app will connect to BITBOX Cloud over [REST](https://rest.bitbox.earth/). If you want to connect to your ownn REST services pass in a config object when instantiating `bitbox-cli`.

```js
let BITBOX = new BITBOXCli({
  restURL: 'my-rest-url'
});
```

## Summary

W/ BITBOX Scaffolds you can now create a $BCH app in under a munute. This should accelerate how quickly developers can create Bitcoin Cash apps. Go developers go!
