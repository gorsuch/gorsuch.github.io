---
layout: post
title: TIL considered harmful
categories:
  - antifragile
---

A short note: if a server is named lb1, it should do load balancing.  And that is it.  Please do not also cram a VPN endpoint on the box or whatever else comes to mind.  Doing so violates the [Principle of Least Astonishment](http://www.catb.org/~esr/writings/taoup/html/ch01s06.html#id2878339) and only leads to confusion down the road.  Yes, that information can be contained in your Puppet manifest or your Chef cookbook, but the name itself implies a single role.

This is similar to the [single responsibility principle](http://en.wikipedia.org/wiki/Single_responsibility_principle) in software development.
