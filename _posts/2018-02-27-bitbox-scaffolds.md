---
layout: default
title:  "BITBOX Scaffolds"
date:   2018-02-27
---

The goal of BITBOX is to accelerate how quickly $BCH devs can create their apps. W/ that in mind today we're launching Scaffolds. This is a way to bootstrap a Bitcoin Cash application in under a minute.

When creating a new application on any platform too much time is spent setting everything up and configuring it. BITBOX scaffolds do the heavy lifting for you and let you focus on building your app.

## Setup

1. Download the latest build from [bitbox.earth](https://www.bitbox.earth/) and compare [the checksums](https://github.com/bigearth/keys-n-hashes)
2. Start your local BITBOX
3. [Install `bitbox-cli`](https://www.npmjs.com/package/bitbox-cli) globally
  * `npm install bitbox-cli --global`
4. Create empty directory for your new app
  * `mkdir BCH4all && cd BCH4all`
5. Scaffold a React app w/ BITBOX web bindings
  * `bitbox scaffold --framework react`
6. Install dependencies
  * `npm install`
7. Start the app
  * `npm start`
8. Open a browser to `http://localhost:3000/` and confirm you are seeing the `getinfo` method returning data from your local BITBOX
9. Win

## React

This initial release only supports React but the intention is to have a registry where people can post scaffolds and an API for `bitbox` to install them.

## Summary

W/ BITBOX Scaffolds you can now create a $BCH app in under a munute. This should accelerate how quickly developers can create Bitcoin Cash apps. Go developers go!
