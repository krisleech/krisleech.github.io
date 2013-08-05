---
layout: post
title: "Archive and Compress files"
date: 2013-07-28 21:01
comments: true
published: true
categories: [shell, linux, gnu]
---

## Create (-c)

tar -cf public.tar /path

## Compress (-z, -j)

tar -czf public.tgz /path
tar -cjf public.bz2 /path

## Extract (-x)

tar -xvjf file.tar.bz2
tar -xvzf file.tgz

## Options

x Extract
c Create
z Compress (gzip)
j Compress (bzip2)
v Verbose

To get LZMA compression you can use --lzma.
