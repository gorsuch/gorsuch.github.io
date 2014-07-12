---
layout: post
title: The Importance of Top-Level Monitoring
---

![](http://f.cl.ly/items/1R2m082F3H3s3G3F0U1F/Image%202014-07-11%20at%2013.58.53.png)

The above diagram shows a typical time-series chart, this one focusing on HTTP response time for a specific URL.  This measurement data is produced by [canary.io](http://www.canary.io/), and the graph is being rendered by a [hosted version](http://watch.canary.io) of Jeremy Green's [canary-ember](https://github.com/jagthedrummer/canary-ember) tool.

Canary takes measurements of a given URL at a rate of several times per second.  Measurements timeout if they cannot complete in 10 seconds, and are currently taken from 5 distinct geographic locations.

Towards the left side, we see a broken lines hovering around the 10 second mark.  This tells us that most of the measurements are not even completing, a clear sign of a problem.

Somewhere around 13:57:30 or so, we see the latency coming down into the subsecond range and everything smoothing out - an indication that the problems have subsided.
