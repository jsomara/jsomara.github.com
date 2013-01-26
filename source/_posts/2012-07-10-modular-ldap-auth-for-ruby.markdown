---
layout: post
title: "Modular LDAP auth for Ruby"
date: 2012-07-10 11:08
comments: true
categories: [ruby, rails, ldap, active_directory, freeipa, auth]
---
ldap fluff provides an easy way for your ruby apps to query regular LDAP,
FreeIPA and Active Directory systems for authentication and authorization.

It's currently being used in the [Katello Project](https://github.com/Katello/katello)
to provide the auth system with authentication and group membership info

You can easily add ldap fluff to your system, especially if you're already using devise
or something similar, to add additional auth backend support. Here's how to get started:

```
gem install ldap_fluff

```

You need /etc/ldap_fluff.yml to configure your LDAP connection. An example is provided in
the [source repo](https://github.com/jsomara/ldap_fluff/blob/master/etc/ldap_fluff.yml)

Here's an example to connect to my FreeIPA server, named AMERICA

``` yaml config example
host:  america 
port: 389 
encryption: start_tls
base_dn: dc=jomara,dc=redhat,dc=com 
group_base: dc=jomara,dc=redhat,dc=com 
server_type: free_ipa 
service_user: admin
service_pass: reallylookingforwardtotheweekend 

```

The service_user doesn't have to be admin (and probably shouldn't be - but I'm
no LDAP admin), but just any user that has R/O access to user & group data

Next, fire up irb or your rails console

``` ruby irb fluff example 
irb(main):001:0> require 'ldap_fluff'
irb(main):002:0> ldap = LdapFluff.new
=> #<LdapFluff:0x7f7e9ca37358>...
irb(main):003:0> ldap.group_list('rstantz')
=> ["ipausers", "ghostbusters", "broskies", "eighties"]
irb(main):005:0> ldap.is_in_groups?('rstantz',['ghostbusters'])
=> true
irb(main):006:0> ldap.is_in_groups?('rstantz',['transformers','ghostbusters'])
=> false
irb(main):007:0> ldap.is_in_groups?('rstantz',['transformers'])
=> false
irb(main):009:0> ldap.authenticate?('rstantz','bustin')
=> false
irb(main):010:0> ldap.authenticate?('rstantz','ghostbustin')
=> true

```

Note that the "is in groups?"  method is an AND operation. These methods should
provide all you need to support read only authorization and authentication
based on LDAP and LDAP groups. 
