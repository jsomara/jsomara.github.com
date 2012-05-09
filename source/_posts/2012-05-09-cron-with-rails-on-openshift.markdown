---
layout: post
title: "Cron with Rails on Openshift"
date: 2012-05-09 11:38
comments: true
categories: [openshift, rails, ruby]
---
Openshift has a cron cartridge that's very easy to install and configure. You
simply install the cron cartridge:

```
rhc-ctl-app -a $appname -e add-cron-1.4
```

And populate the directory tree in your app's git repository:

```
openshift
├── cron
│   ├── daily
│   │   └── my_script.rb
│   ├── hourly
│   ├── minutely
│   ├── monthly
│   ├── README.cron
│   └── weekly
│       ├── chrono.dat
│       ├── chronograph
│       ├── jobs.allow
│       ├── jobs.deny
│       └── README

```

The trick is that unlike Heroku, the Openshift cron system is basically just
like regular linux cron. They do not execute (by default) in your application's
environment. Thankfully, it's very easy to set up your scripts to run this way.

``` ruby ruby cron example
#!/usr/bin/env ruby
# this script will default to DEVELOPMENT if you dont force it
ENV['RAILS_ENV'] ||= 'production'

require File.expand_path("./config/environment", ENV['OPENSHIFT_REPO_DIR']) 

Rails.logger.debug 'daily cron job!'
MyHelper.do_something_daily('internet')

```
It should be pretty obvious where to put your script files based on when you want
them to run!
