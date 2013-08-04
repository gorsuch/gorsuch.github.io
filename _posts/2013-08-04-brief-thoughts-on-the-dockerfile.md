---
layout: post
title: Brief thoughts on the Dockerfile
---

I've been playing with [Docker](http://www.docker.io/) recently, digging in to see if we use it to decouple our ever growing number of applications at [GitHub](https://github.com) from the underlying infrastructure.

Initial experiments are promising.  I [built an image](https://github.com/gorsuch/dockerfile-examples/blob/master/rbenv/Dockerfile) containing a minimal `rbenv` configuration and was able to get a trivial app up and running - all while watching an episode of _[Sons of Anarchy](http://en.wikipedia.org/wiki/Sons_of_Anarchy)_.  I even opened up an [amazing pull request](https://github.com/dotcloud/docker/pull/1400) when the action slowed.

I am most excited about the notion of the `Dockerfile`.  By bundling these with an application, I can specify the preferred execution environment rather than have to fight with whatever is already in play.  My deployment can be bundled with most of the information needed for a successful execution.  The `Procfile` describes the processes that can be run, the `Dockerfile` describes the runtime.  Mix in some environment variables and you might be in business.  There are plenty of details to work out, but this has so much potential.
