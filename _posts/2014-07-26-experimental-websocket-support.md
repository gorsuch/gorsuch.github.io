---
layout: post
title: Canary Gets Experimental Websocket Support
---

This morning, I upgraded the public version of `canaryd` to the 1.0.0 branch.  The big public-facing benefit here is websocket support for streaming down per-check measurements, rather than having to continously poll the `measurements` endpoint.

Big thanks to [Karl Kirch](https://twitter.com/joekarl) for [doing all of the work](https://github.com/canaryio/canaryd/pull/57).

You can test this yourself like so:

```sh
$ go get github.com/gorsuch/ws

$ ws -url wss://api.canary.io:443/ws/checks/https-github.com/measurements
{"check":{"id":"https-github.com","url":"https://github.com"},"id":"38dbd030-2821-4468-40f9-ed33c8eb11ab","location":"rax-syd","t":1406383947,"timestamp":"","exit_status":0,"http_status":200,"local_ip":"119.9.20.212","primary_ip":"192.30.252.131","namelookup_time":0.000561,"connect_time":0.213167,"starttransfer_time":0.864768,"total_time":1.0773899999999998,"size_download":15383}
#...
```

As stated in the title, this is considered experimental for the time being and is subject to change.

Enjoy, and show Karl some love!
