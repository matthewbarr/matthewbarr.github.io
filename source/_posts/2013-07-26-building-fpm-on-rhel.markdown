---
author: mhbarr
comments: true
date: 2013-07-26 15:57:28+00:00
layout: post
slug: building-fpm-on-rhel
title: Building FPM on RHEL
wordpress_id: 32
---

(If you have a version of FPM installed, you may want to remove & install the newest version before building...)

Building FPM:

```bash
gem install --install-dir /tmp/gems fpm
cd /tmp/gems/cache/

fpm -s gem -t rpm -d rpm-build -d rubygems -d ruby -d ruby-devel -d kernel-devel -d gcc -d gcc-c++ -d make -e fpm*.gem
```
Building other gems:

```bash
fpm -s gem -t rpm -d rubygems -d ruby *.gem
```