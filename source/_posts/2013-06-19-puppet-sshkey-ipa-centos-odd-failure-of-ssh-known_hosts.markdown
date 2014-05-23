---
author: mhbarr
comments: true
date: 2013-06-19 20:48:05+00:00
layout: post
slug: puppet-sshkey-ipa-centos-odd-failure-of-ssh-known_hosts
title: Puppet sshkey + IPA + Centos = odd failure of ssh known_hosts
wordpress_id: 15
tags:
- puppet
- sysadmin
---

Recently saw this at work:

_Server with IPA (freeipa.org), running puppet 3.X (3.2.1, but not really relevant.)  Centos 6.4._


## Normally


Puppet's [sshkey type](http://docs.puppetlabs.com/references/latest/type.html#sshkey)  creates /etc/ssh/ssh_known_hosts by default.

SSH defaults to checking /etc/ssh/ssh_known_hosts & /etc/ssh/ssh_known_hosts2, if GlobalKnownHostsFile isn't set inside [ssh_config](http://www.openbsd.org/cgi-bin/man.cgi?query=ssh_config).   RHEL / Centos doesn't have it in the default config file, so it's probably using the default for you.

All very good.

BTW- they could fix a long time sshkey bug: https://projects.puppetlabs.com/issues/4145 - it defaults to mode 600, which really needs to be 644 to be useful.


## With IPA


If you're using IPA, though, with SSSD, it adds this to your /etc/ssh/ssh_config:

    
    GlobalKnownHostsFile /var/lib/sss/pubconf/known_hosts


This in turn disables the defaults, and it **only** checks /etc/ssh/ssh_known_hosts2. **Not** /etc/ssh/ssh_known_hosts.

Fix is to use a target parameter, for the sshkey, to make sure you write to /etc/ssh/ssh_known_hosts2.

Not sure how I'm going to get a bug opened on this. It's not a puppet bug at all. It's probably a bug for OpenSSH, but I don't have a binary that they build, so I can't submit a bug :(



BTW: for those that are interested: `strace ssh HOSTNAME` was how I found the details.  Since it stops and waits for the human to agree to add the host key to known_hosts, it's actually fairly easy to strace.
