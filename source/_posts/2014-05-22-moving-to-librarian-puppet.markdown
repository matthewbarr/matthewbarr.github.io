---
layout: post
title: "Moving to librarian-puppet"
date: 2014-05-22 10:47:01 -0400
comments: true
categories: puppet
---
So... The office hit a point where I had way too many modules installed from the forge & git, and it was past the point of being a problem.  

(Beyond the maintenance problem, we also hit an issue for using [Packer & Docker](/blog/2014/05/22/puppet-for-packer-and-docker/) )

We've implemented a hybrid roles & providers model, so we end up w/ quite a few custom modules for our needs.

We had 45 modules.  It was starting to get difficult to find our library modules, from our custom ones.

I've seen people talking about [Librarian Puppet](https://github.com/rodjek/librarian-puppet), and about [R10K](https://github.com/adrienthebo/r10k).

(FYI: http://garylarizza.com/blog/2014/02/18/puppet-workflow-part-3/ is a very explanation of why you might use R10K, and how..)

However, I *really* didn't want to have to move all my code to a new repo, and especially didn't want to move each module to it's *own* repo.  This is all being maintained by one person right now, and the dev's have enough trouble w/ it being in it's own repo, vs the main code repo.

Eventually I realized that I could move the custom modules to their own module directory, and use the main "modules" directory for librarian-puppet.  

In order for this to work, I had to modify puppet.conf to have 

    modulepath =  $confdir/modules-local:$confdir/modules


On my server w/ (older, pre 3.5) dynamic environments, I used:

        modulepath =  $confdir/environments/$environment/modules-local:$confdir/environments/$environment/modules


Once the Puppetfile was created, I just had to run:

    librarian-puppet install

Here's the Puppetfile we used, and what it produces:

```plain Puppetfile
forge "http://forge.puppetlabs.com"

mod "rodjek/logrotate"

mod "elasticsearch/logstash"
mod "elasticsearch/elasticsearch"
mod "sensu/sensu"
mod "garethr/graphite"

mod "puppetlabs/ntp"
mod "puppetlabs/mcollective",
  :git => 'https://github.com/matthewbarr/puppetlabs-mcollective.git',
  :ref => 'kensho'

mod "puppetlabs/vcsrepo"
mod "puppetlabs/git"


mod "jfryman/nginx"
mod "puppetlabs/inifile"


```
Generates:

```
modules
├── elasticsearch-elasticsearch (v0.3.2)
├── elasticsearch-logstash (v0.5.1)
├── footballradar-python (v0.1.0)
├── garethr-graphite (v0.3.0)
├── ispavailability-file_concat (v0.1.0)
├── jfryman-nginx (v0.0.9)
├── mcollective (???)
├── nanliu-staging (v0.4.0)
├── puppetlabs-activemq (v0.2.0)
├── puppetlabs-apache (v0.6.0)
├── puppetlabs-apt (v1.4.2)
├── puppetlabs-concat (v1.1.0)
├── puppetlabs-firewall (v1.1.1)
├── puppetlabs-git (v0.0.3)
├── puppetlabs-inifile (v1.0.3)
├── puppetlabs-java (v1.1.1)
├── puppetlabs-java_ks (v1.2.3)
├── puppetlabs-ntp (v3.0.3)
├── puppetlabs-rabbitmq (v4.0.0)
├── puppetlabs-stdlib (v4.2.1)
├── puppetlabs-vcsrepo (v0.2.0)
├── richardc-datacat (v0.4.3)
├── rodjek-logrotate (v1.0.2)
└── sensu-sensu (v1.0.0)
```

Which is 26/45 of my original modules.  I was also able to remove a few dependencies, and upgraded a few of the modules.  (Which why you only see 24 modules listed above..)

It also meant that I had to extract my changes to a few upstream modules, and send them up as PR's.  In the end, the only critical one was a few modifications to the puppetlabs-mcollective module, which is why I'm currently using a git fork.  PR's have been raised for most of the changes, but they aren't live yet.

* [Extra Yaml in mco](https://github.com/puppetlabs/puppetlabs-mcollective/pull/109) Needs more work.
* [Remove Erlang dependency](https://github.com/puppetlabs/puppetlabs-mcollective/pull/145)


