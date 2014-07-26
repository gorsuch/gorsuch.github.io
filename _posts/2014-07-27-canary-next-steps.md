---
layout: post
title: Canary Next Steps
---

Now that I have my feet on the ground and my [new gig](https://www.simple.com), it's time to start talking straight about the future of [canary](http://www.canary.io).  Over the past 2 months, I've observed a few things:

1. the original components are in rough shape. I barely knew go then, and know just enough now to see how things could be made better
2. I am very interested in continuing to develop a high quality SaaS offering around these concepts
3. I am very interested in continuing to make it easy to for individuals to run their own Canary infra
4. points 2 and 3 are in apparent conflict with each other - it takes much different infrastructure to make canary operable for a larger audience, and it is not fair to impose those heavier requirements on the DIY user
5. there is a reasonable path forward that addresses the previous points!

So let's talk about this.

## Canary: The Service

Canary as a Service has a simple goal: to help you become aware of your site's recent (past 5 minutes) top-level availability.  It does this by taking measurements of your site at a high resolution, and providing various services around that data that give you visiblity into your overall availability and assist in tracking down problems.  In the longer-term, we'd like to provide more advanced services such as alerting and trending, but I think it is better define a service in a way the differentiates itself from any perceived competition.

In order to help you have confidence in the service, I believe it is important for it to remain completely open source.  This allows you to take a look under the covers and see _exactly_ how availability is being measured and reported.

It goes without saying that [the current proof of concept](http://www.canary.io/) is a far cry from the polished product I am describing.  We have a lot of work on our plates, and are excited to get moving on this.  You can see the new ground being broken in [this repository](https://github.com/canaryio/canary), and you are invited to follow along.

But what about the existing canary projects like [canaryd](https://github.com/canaryio/canaryd) and [sensord](https://github.com/canaryio/sensord)?  Read on!

## Canary: DIY

I'm a big fan of making it easy for some form of Canary to be put to use by others outside of the main service.  Being able to run a Canary sensor in your own environment and integrate that output with your existing operational infrastructure is a HUGE win.

I plan on making the following changes so that we can continue to support this:

### Replace existing sensord component

A new `sensord` component will be born inside the [canaryio/canary](https://github.com/canaryio/canary) repository.  Its spec is defined [here](), but the main takeaway:

* measurements can optionally be written to stdout, one JSON item per line
* measurements can optinally be sent to a [statsd](https://github.com/etsy/statsd/) aggregator, allowing you to get them into the metrics backend of your choosing
* measurements can optioally be delivered via HTTP POST to a URL of your choosing
* removal of `go-curl` dependency, instead pushing us to go's `net/http` package
* removal of `go-metrics` for operational metrics, relying on `statsd` instead

The current [canary/sensord](https://github.com/canaryio/sensord) component will be deprecated. The repo will remain, but we will not accept further issues or pull requests.

### Replace existing canaryd component

Similar to `sensord`, a new component will be created as a subpackage of [canaryio/canary](https://github.com/canaryio/canary).  The spec is [here](), the main points being:

* continue to use redis a backend
* receive measurements via HTTP
* use SSE instead of websockets for near-realmtime delivery
* optionally publish to redis pubsub
* remove `go-metrics`, use `statsd` for operational metrics
