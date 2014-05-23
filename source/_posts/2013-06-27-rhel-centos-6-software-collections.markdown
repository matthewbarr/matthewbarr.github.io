---
author: mhbarr
comments: true
date: 2013-06-27 21:14:14+00:00
layout: post
slug: rhel-centos-6-software-collections
title: RHEL / Centos 6 Software Collections
wordpress_id: 23
---

So, there's a new way to use multiple versions of languages, such as Ruby, on RHEL, without breaking the system.  I've got this down, so I thought I should share it.

In this case, it's replacing the need for RVM, and moving to a much more standardized system.  I <del>don't love</del> can't stand having GCC installed on production servers, so this is much better.

SCL - Software Collections :)

The goal of this was to get a recent version of Fog working on RHEL 6 / Centos 6.

First:  get the SCL repo:

```bash
cd /etc/yum.repos.d`
wget http://people.redhat.com/bkabrda/scl_ruby193.repo
```

That gives you the basics, but we also need to get some pieces for Fog, compiled for SCL. Thanks to the Katello Project, which is packaging Fog for use w/ Foreman, we can add their repo. You'll have to make a repo for this, though:

[http://fedorapeople.org/groups/katello/releases/yum/foreman-nightly/RHEL/6Server/x86_64/](http://fedorapeople.org/groups/katello/releases/yum/foreman-nightly/RHEL/6Server/x86_64/)

--You could probably use that as your repo for ruby_193, even without the above scl repo.

This is what the repo file would look like:

```plain
[foreman-nightly]
name=Foreman Nightly
baseurl=http://fedorapeople.org/groups/katello/releases/yum/foreman-nightly/RHEL/6Server/x86_64/
enabled=1
gpgcheck=0
```

### Now, install ruby_193:


    yum install ruby193-ruby ruby193-rubygem-fog   ruby193-rubygem-rbvmomi

Now, you can use it:

    scl enable ruby193 bash
    which ruby

---

One thing, if you want to use ruby_193 inside scripts, you can use a stub for it:

    /usr/bin/ruby193-ruby
