---
author: mhbarr
comments: true
date: 2013-07-26 16:19:37+00:00
layout: post
slug: ipa-server-client-commands
title: IPA server & client commands
wordpress_id: 43
tags:
- IPA
- RHEL
- sysadmin
---

Not that this isn't available elsewhere, but... just keeping it nicely stored somewhere for myself.

on ipa server:

    ipa host-add host.example.com --password=MEEEEEP --force

on client:

    ipa-client-install --hostname=host.example.com --domain=example.com --password=MEEEEEP --realm=EXAMPLE.COM --server=ipa.example.com --unattended --mkhomedir
 
 
----
```bash Hostadd.sh
#!/bin/bash
echo
echo "ipa host-add $1.example.com --password=MEEEEEP --force"
echo
echo
echo "ipa-client-install --hostname=$1.example.com --domain=example.com  --password=MEEEEEP --realm=EXAMPLE.COM --server=ipa.example.com --unattended --mkhomedir"
 
echo
```
 
----
 
Uninstall IPA

```bash Uninstall IPA
[root@solr05 ~]# ipa-client-install --uninstall -U
Unenrolling client from IPA server
Removing Kerberos service principals from /etc/krb5.keytab
Disabling client Kerberos and LDAP configurations
Restoring client configuration files
```
