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

![Hello BITBOX]({{ "/assets/hello-bitbox.png" | absolute_url }})

## Angular

To quickly create an [Angular app w/ BITBOX web bindings](https://github.com/bigearth/bitbox-scaffold-angular) follow these steps.

### Setup

1. Download the latest build from [bitbox.earth](https://www.bitbox.earth/) and compare [the checksums](https://github.com/bigearth/keys-n-hashes)
2. Start your local BITBOX
3. [Install `bitbox-cli`](https://www.npmjs.com/package/bitbox-cli) globally
  * `npm install bitbox-cli --global`
4. Create empty directory for your new app
  * `mkdir BCH4all && cd BCH4all`
5. Scaffold a React app w/ BITBOX web bindings
  * `bitbox scaffold --framework angular`
6. Install dependencies
  * `npm install`
7. Start the app
  * `npm start`
8. Open a browser to `http://localhost:4200/` and confirm you are seeing the `getinfo` method returning data from your local BITBOX
9. Win

![Hello BITBOX]({{ "/assets/hello-bitbox.png" | absolute_url }})

## Node

To quickly create a [Node JS app w/ BITBOX bindings](https://github.com/bigearth/bitbox-scaffold-node) follow these steps.

### Setup

1. Download the latest build from [bitbox.earth](https://www.bitbox.earth/) and compare [the checksums](https://github.com/bigearth/keys-n-hashes)
2. Start your local BITBOX
3. [Install `bitbox-cli`](https://www.npmjs.com/package/bitbox-cli) globally
  * `npm install bitbox-cli --global`
4. Create empty directory for your new app
  * `mkdir BCH4all && cd BCH4all`
5. Scaffold a React app w/ BITBOX web bindings
  * `bitbox scaffold --framework node`
6. Install dependencies
  * `npm install`
7. Start the app
  * `npm start`
8. Confirm you are seeing the `getinfo` method returning data from your local BITBOX
9. Win

## Config

By default your new app will connect to your local running BITBOX but if you want to connect to a remote running node update the `BITBOX` instantiation.

```js
let BITBOX = new BITBOXCli({
  protocol: 'http',
  host: 'remote-ip-address',
  port: 8332,
  username: 'rpc-username',
  password: 'rpc-password'
});
```

You'll need to allow CORS on your remote server to accept in coming requests if you're using the React scaffold.

## `--repo` flag

The `bitbox scaffold` command also supports a `--repo` flag which takes as an argument a path to a github repo.

The path must include the `.git` file extension. If both a `--framework` and a `--repo` flag are included the `--repo` flag will take precedence.

```
$ bitbox scaffold --repo https://github.com/bigearth/bitbox-scaffold-angular.git
```

## Summary

W/ BITBOX Scaffolds you can now create a $BCH app in under a munute. This should accelerate how quickly developers can create Bitcoin Cash apps. Go developers go!
