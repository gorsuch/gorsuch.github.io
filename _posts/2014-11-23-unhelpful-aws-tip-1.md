---
layout: post
title: Unhelpful AWS Tip 1 - Compute Instances, Not Computers
---

*I've been hacking around on AWS for five years now, and am starting to compile a list of tips and tricks to help make the most of things.  I'm hoping to use this series of posts to help me clarify my own thinking, and if you find any of this helpful, even better.*

When working in a bare metal environment, I'd likely provision a new box and hook it up to a Chef Server or Puppet Master.  I'd spend most of my time thinking about my on-instance configuration management, as that matters quite a bit since these boxes are going to hang around for a long time.

This isn't the right way to approach EC2.  In the AWS model, you want your EC2 instances to be stateless and thrown away often.  You should be pushing state onto dedicated services such as Heroku Postgres or Amazon RDS / DynamoDB.  Your computer instances should be as thin as possible, focusing as much of their compute power on the task at hand.

It's useful to think of EC2 instances as nothing more than dynamic compute containers awaiting instruction.  For example, take a look at this small ruby script:

```rb
#!/usr/bin/env ruby

require 'aws-sdk'

metadata = {
  image_id:      'ami-9aaa1cf2',
  instance_type: 't2.micro',
  subnet:        'subnet-a55587fc',
  key_name:      'ops',
  slug_url:      'https://s3-us-west-1.amazonaws.com/example-slugs/demo.tar.gz',
  cmd:           'bin/web'
}

script=<<END
#!/bin/bash

set -exo pipefail

apt-get update
apt-get install -y runit ruby

# our hero
useradd -r app
mkdir -p /srv/app
chown app:app -R /srv/app

# fetch the app
su - app -c 'curl -s #{metadata[:slug_url]} | tar xvz -C /srv/app'

# write our runit script
mkdir -p /etc/sv/app
cat >> /etc/sv/app/run <<EOF
#!/bin/bash

cd /srv/app
exec chpst -u app #{metadata[:cmd]}
EOF
chmod +x /etc/sv/app/run

# write our runit logger script
mkdir -p /etc/sv/app/log
mkdir -p /var/log/app
cat >> /etc/sv/app/log/run <<EOF
#!/bin/bash
exec svlogd -t /var/log/app
EOF
chmod +x /etc/sv/app/log/run

# symlink to turn our app on
ln -s /etc/sv/app /etc/service/app
END

ec2 = AWS::EC2.new
i = ec2.instances.create({
  image_id:                             metadata[:image_id],
  instance_type:                        metadata[:instance_type],
  subnet:                               metadata[:subnet],
  associate_public_ip_address:          true,
  key_name:                             metadata[:key_name],
  instance_initiated_shutdown_behavior: 'terminate',
  user_data:                            script
})

puts "launched #{i.id}..."
```

Let's walk through this:

* we define some metadata for our instance that not only describes the necessary AWS elements but also a URL for the app we want to run as well as what command we need to use to invoke it
* we render a shell script based on those paraemters.  It sets up a process supervisor (runit in this case), fetches our app and starts it up
* we launch an instance with these inputs

And that's about it.  If you were to visit port 5000 with a browswer, you'd find a 'Hello World' web app running.

## The Good Parts

* things are data-driven and fairly functional - data in, app instance out
* we just launched our app on a compute instance with minimal code
* it is repeatable - you could run N of these if you so pleased
* at this point, we're not _required_ to use more complex configuration management - a shell script is just fine
* we stand a fair chance at being able to run any app we want - just give us a tarball, and we're set if the right libs are installed on the host

## Room for Improvement

Things that immediately jump to mind:

* what if we want to configure this app - what then?
* how do we know that it booted okay?
* how do I maintain a fleet of these with minimal overhead?
* what if I need a load balancer?
* what if I want to run a lot of different apps, with minimal overhead?
* I don't like installing packages at boot time
* how do we reliably support apps that are not ruby?

## In Summary

The more you treat EC2 instances like bare metal computers, the more you'll hate your life.  Treat them like single-task compute instances, and you'll find yourself working with something more like legos.  Your call.

Future posts will likely dig into improvements and higher level concerns.  I'll probably also go all hipster on you and demonstrate how docker could be applied here.
