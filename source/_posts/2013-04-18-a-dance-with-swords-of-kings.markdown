---
layout: post
title: "A Dance Game of Crows with Swords of Kings"
date: 2013-04-18 14:56
comments: true
categories: epub, copyright, game of thrones, george rr martin 
---

A friend and I thought it would be fun to read A SONG OF ICE & FIRE in order by
character instead of the chapter order presented in the books. However, with
regular physical books this would be an enormous pain in the ass. It would be a
little easier with Kindle copies and a good chapter listing, but not good
enough.

I didn't know much about the various open book standards (epub, mobi) but I
thought it might be worth taking a look at them to see how I could hack them up
for this project. Thankfully epub is just essentially a zip archive and a
collection of html files with a table of contents and some brief metadata. I
went about writing [a script](https://github.com/jsomara/crows) to automate the
process.

Obviously, it requires a collection of the books in epub format to work.
Additionally, each epub publisher categorizes the chapters differently - you'll
have to inspect the html and edit the chapter parsing part of crows.rb for your
files.

Right now, an [online epub validator](http://validator.idpf.org/) says my epub
isn't valid, but hopefully with a little more hacking I'll have a book that
works on my devices.

The real question is whether or not it will make sense to read the series like this!
