---
layout: post
title: "Using xz with ruby"
date: 2013-02-04 15:28
comments: true
categories: ruby, xz, compression 
---
[LZMA2 / xz](http://en.wikipedia.org/wiki/Xz) are the hottest new compression
algorithms on the block. When I found out [Katello](http://katello.org) would
have to decrypt certain data payloads compressed with xz, I was excited to work
on it.

I found the excellent [ruby-xz package](https://github.com/Quintus/ruby-xz) and
got to work. My little bit of code (not quite ready for a pull request yet,
otherwise I'd link it) would simply have to check a file on a remote server,
figure out if the file was newer than the last check, and if so decrypt it and
process it.

This gem makes it so simple! You need to require 'open-uri' to open URLs as
file streams first.

The data I'm parsing is JSON, so I can't load the data in chunks. Random chunks
of decompressed data would not be valid JSON.

```
require 'open-uri'
require 'ruby-xz'

...

open(@url) do |f|
  JSON.load(XZ.decompress_stream(f))
end
```

Fast & easy!
