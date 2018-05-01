---
layout: default
title:  "REST"
date:   2018-05-01
---

BITBOX has so far focused on giving you tools to build great $BCH apps. However once you have a working app you need cloud infrastructure to deploy, host and scale your app. You need an ecosytem of add-ons to take your app to the next level. For that there's [BITBOX Cloud](https://cloud.bitbox.earth/). Today we're rolling out part 1 of BITBOX Cloud&mdash;[REST](https://rest.bitbox.earth).

![rest.bitbox.earth]({{ "/assets/swagger1.png" | absolute_url }})

### Modern architecture

Modern app architectures consist of a native or web client which a user interacts w/. That client sends HTTP requests to a cloud service which returns data that the client shows to the user.

REST give you access to 100% of the Bitcoin Cash RPC over HTTP. It speaks idiomatic REST and makes proper use of HTTP verbs.

### From HTTP to JSON

When reading from the chain REST expects `GET` requests. When writing to the chain REST expects `POST` requests. [All requests return JSON](https://rest.bitbox.earth/v1/blockchain/getTxOut/d94b98cf5b192f15abaac6201a407f500ce2cba23d4cbb79c712e3aa4e1bb7cf/0) data from `bitcoind`.

![json]({{ "/assets/swagger3.png" | absolute_url }})

### REST GUI

In addition to interacting w/ REST over HTTP you can also use a GUI. This lets you quickly paste in values to see returned data before writing a script.

![swagger]({{ "/assets/swagger2.png" | absolute_url }})

## BITBOX Cloud

REST is just the first step in BITBOX Cloud which will be your "Blockchain as a Service" provider. [BITBOX](https://www.bitbox.earth/) is committed to providing the hiqhest quality open source tools so developers can continue to change the world w/ Bitcoin Cash.
