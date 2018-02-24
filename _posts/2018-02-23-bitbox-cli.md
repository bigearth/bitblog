---
layout: default
title:  "bitbox-cli"
date:   2018-02-17
---

BITBOX aims to be the go-to toolset for $BCH developers. We want you to be able to run your code and tests against your BITBOX before deploying to live environments and aim for 100% Bitcoin Cash JSON RPC API coverage.

Today we're happy to announce that `bitbox-cli` has 100% of the Bitcoin ABC JSON RPC. You can now use `bitbox-cli` to have your code interface directly w/ your live running Bitcoin ABC node. Here's how that would look.

## Installation

First install `bitbox-cli` via npm

```
npm install bitbox-cli --global
```

This gives you access to the `bitbox` command line utility.

## Command line

You can now have `bitbox` stub out an app scaffold and configuration. Here we'll have it create a new app and give it the user, password and host of our running Bitcoin ABC node and set the environment to `production`.

```
bitbox new --title myApp --username myUser --password str0ngP@ssw0rd --host ip.address.of.abc.node --environment production
```

This will create a scaffold directory structure for a new $BCH app along w/ a configuration file which looks like:

```js
networks: {
  production: {
    protocol: "http",
    host: "ip.address.of.abc.node",
    port: "8332",
    username: "myUser",
    password: "str0ngP@ssw0rd"
  }
}
```

## Console

Now you can fire up `bitbox`'s console to send some commands to your running $BCH node.

```
bitbox console --environment production
```

This will load the console and create a `BITBOX` object w/ the entire $BCH JSON RPC available. That `BITBOX` object can send commands to the running Bitcoin ABC node that you configured in the previous step.

```
⚡️  BITBOX ⚡️   BITBOX.getnetworkinfo().then((result) => { console.log(result); }, (err) => { console.log(err); });

{ version: 160200,
  subversion: '/Bitcoin ABC:0.16.2(EB8.0)/',
  protocolversion: 70015,
  localservices: '0000000000000025',
  localrelay: true,
  timeoffset: 0,
  networkactive: true,
  connections: 15,
  networks:
   [ { name: 'ipv4',
       limited: false,
       reachable: true,
       proxy: '',
       proxy_randomize_credentials: false },
     { name: 'ipv6',
       limited: false,
       reachable: true,
       proxy: '',
       proxy_randomize_credentials: false },
     { name: 'onion',
       limited: true,
       reachable: false,
       proxy: '',
       proxy_randomize_credentials: false } ],
  relayfee: 0.00001,
  incrementalfee: 0.00001,
  localaddresses: [ { address: 'ip.address.of.abc.node', port: 8333, score: 1964 } ],
  warnings: '' }
```
