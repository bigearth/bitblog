---
layout: default
title:  "Multiple Environments"
date:   2018-02-24
---

Imagine you have a $BCH app which you'd like to deploy. However before doing that it seems like you should test it. BITBOX console lets you quickly switch between sending commands to your local instance or your full node running on the cloud. That way you can test your code locally before deploying to a remote server w/ production running $BCH node.

## Create new app

Let's say our newly created $BCH app is just a simple call to `getnetworkinfo` but of course it could be much more complex as `BITBOX` supports the entire $BCH RPC.

```js
BITBOX.getnetworkinfo()
  .then((result) => { console.log(result); },
  (err) => { console.log(err);
})
```

To test our code first stub out an app w/ production credentials

```
bitbox new --title myApp --username myUser --password str0ngP@ssw0rd --host ip.address.of.abc.node --environment production
```

Next open up the newly created `bitbox.js` file and add config for your local BITBOX.

```js
networks: {
  development: {
    protocol: "http",
    host: "localhost",
    port: "8332",
    username: "",
    password: ""
  },
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

Now when you fire up your BITBOX console you can tell it which environment to connect to. First let's test locally. For that you don't need to pass `bitbox console` an `--environment` flag.

```
> bitbox console

⚡️  BITBOX ⚡️   BITBOX.getnetworkinfo()
               .then((result) => {
                 console.log(result); },
                 (err) => { console.log(err); }
               );

{ version: 130100,
  subversion: '/Bitcoin ABC:0.16.2(EB8.0)/',
  protocolversion: 70014,
  localservices: '000000000000000d',
  localrelay: true,
  timeoffset: -19,
  connections: 8,
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
  relayfee: 5000,
  localaddresses:
   [ { address: '0600:3c03::f03c:91ff:fe89:dfc4',
       port: 8333,
       score: 4 } ],
  warnings: '' }
```

As you can see our code works against our local BITBOX. Now let's test it against our production running node. To do that we pass `bitbox console` the `--environment production` flag

```
> bitbox console --environment production

⚡️  BITBOX ⚡️   BITBOX.getnetworkinfo()
               .then((result) => {
                 console.log(result); },
                 (err) => { console.log(err); }
               );

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

Here you can see our code works against a running $BCH node.

## Summary

This is just a small example to show how BITBOX can accelerate your $BCH workflow. You can stub out an app scaffold w/ configuration for `development` and `production` environments. You have the full $BCH RPC available on the `BITBOX` object and you can quickly switch between sending commands to different environments.
