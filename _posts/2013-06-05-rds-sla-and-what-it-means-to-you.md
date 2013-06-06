---
layout: post
title: What the new RDS SLA means to you
categories:
  - aws
  - rds
  - antifragile
---

Amazon has just [announced that RDS has gone GA](http://aws.amazon.com/about-aws/whats-new/2013/06/05/amazon-rds-ga-sla/) and even went so far as to include an SLA with it.

That SLA states that if you deploy with a Multi-AZ setup (meaning, two instances in a given region, each in its own availability zone), they will guarantee 99.95% uptime.  The only  responsible way to read this is "I should _expect_ my database to be down _at least_ 22 minutes per month."

This is in no way meant as a slight to the RDS team - it is a fantastic product and extremely valuable.  However, you must understand that it will fail and be prepared to deal with it.

All the Best,

Michael
