---
layout: post
title: Brief thoughts on the Dockerfile
---

I've been playing with [Docker](http://www.docker.io/) recently, digging in to see if we use it to decouple our ever growing number of applications at [GitHub](https://github.com) from the underlying infrastructure.

Initial experiments are promising.  I [built an image](https://github.com/gorsuch/dockerfile-examples) containing a minimal `rbenv` configuration and was able to get an app up and running.  I even opened up my [first pull request](https://github.com/dotcloud/docker/pull/1400) last night.

I am most excited about the notion of the `Dockerfile`.  By bundling these with my applications, I can specify the preferred execution environment for my app rather than have to fight with whatever is already in play.  My app can potentially be deployed and comes bundled with most of the information needed for a successful execution.  The `Procfile` describes the processes that can be run, the `Dockerfile` describes the runtime.  Mix in some environment variables and you might be in business.  There are plenty of details to work out, but this has so much potential.
