---
author: mhbarr
comments: true
date: 2013-07-26 16:13:42+00:00
layout: post
slug: change-ipa-password-expiration-for-a-bunch-of-users
title: Change IPA password expiration for a bunch of users
wordpress_id: 35
---

Run this on a ipa server.  It'll work there..

1. Find users with passwords that expire before DATE below

        ldapsearch -Y GSSAPI -h localhost -b "cn=users,cn=accounts,dc=ayisnap,dc=com" '(krbPasswordExpiration<=20140101000000Z)'>lsearch2`


 
2. Change the actual expiration date for those users to date :1/1/2015.

```bash
grep dn lsearch2  > dn
IFS=$'\n'
for i in  `cat dn`;
  do echo $i; 
  echo "changetype: modify";
  echo "replace: krbpasswordexpiration";
  echo "krbpasswordexpiration: 20150101000000Z";
  echo ;
done > test
ldapmodify -D "cn=Directory Manager" -W -h localhost -f test
```

