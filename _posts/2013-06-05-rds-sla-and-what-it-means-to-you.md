---
layout: post
title: What the new RDS SLA means for your service
categories:
  - aws
  - rds
  - antifragile
---

Amazon has [announced that RDS has gone GA](http://aws.amazon.com/about-aws/whats-new/2013/06/05/amazon-rds-ga-sla/) and even went so far as to include an SLA with it.

That agreement states that if you deploy a [Multi-AZ setup](http://aws.amazon.com/rds/multi-az/), they will guarantee 99.95% uptime.  The only responsible way approach this is "I should _expect_ my database to be down _at least_ 22 minutes per month."

This is in no way meant as a slight to the RDS team - this is a fantastic product and extremely valuable to me.  However, you must understand that it will fail and be ready to respond.  Are you prepared for this?

All the best,

Michael Gorsuch
