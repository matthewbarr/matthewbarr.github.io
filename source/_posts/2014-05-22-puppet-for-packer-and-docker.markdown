---
layout: post
title: "Puppet for Packer &amp; Docker"
date: 2014-05-22 15:22:20 -0400
comments: true
categories: 
---


In [Moving to Librarian-puppet](/blog/2014/05/22/moving-to-librarian-puppet/) I mentioned that we needed to keep 3 different sets of puppet stacks around.  I thought I'd expand on it.

There will likely be a part 2, & maybe 3, as I describe what we have inside our puppet provisioner code for packer, and then later for docker...


We're using 2 different sets of puppet stacks, similar, but not quite the same, and testing a third:

1. one to build AMI's, installing software, but typically not starting it.
2. the full build
3. testing docker

These are different because we install and start different parts based on which portion of the build is involved.

#### Packer
For Packer, we want to install all the pieces, especially packages with large dependency chains.  We *don't* want to start services, but putting config files in place is helpful.  We also currently put a copy of the code git checkout in place, so that VCSrepo updates can happen very quickly, only needing to download delta's from upstream.


#### Full Build
In the full build, we need to make sure that everything is installed, and running.  We also add some runtime cache cleanup code that wouldn't be helpful in the packer AMI build, and also leave out some scripts that update mcollective caches and drop extra facts into it's cache. [Extra Yaml in mco](https://github.com/puppetlabs/puppetlabs-mcollective/pull/109)

#### Docker
Docker is new target for us.  We're hoping to move to move to Docker for building containers with our code + virtualenv & running either celery or uwsgi.  We hope to be able to use the same artifacts from dev-> prod, and that it will make life easier to update running versions of code.

However, we will still build AMI's for the host systems, running mcollective & puppet, sensu for monitoring, and probably the nginx webserver/proxy for the uwsgi containers.  We'll also include in the packer built AMI's the docker images, so these systems can be deployed via autoscaling.  

This will mean we'll have a different set of requirements for puppet in the docker systems.  We'll be getting rid of much of the code deployment related functions, but still keeping most of the other parts.   And we'll have to add docker related functionality & mgmt.