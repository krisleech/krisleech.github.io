---
layout: post
title: "Getting RVM to work on Mac"
date: 2010-05-14 10:00
comments: true
published: true
categories: [mac ruby]
---

I've been trying on and off for the last six months to get RVM (Ruby Version Manager) working on my Mac. I had no problems on linux. RVM allows you switch between different Ruby interpreters and their respective versions. For example I can test my applications in Ruby 1.9, JRuby and the new YARV bytecode interpreter.

<!--more-->

Today I realised my mistake. When I first got the Mac I had problems with locally installed gems not working so I changed the permissions on ~/.gem to prevent mistakenly installing a gem without sudo. This is the folder RVM uses to install gems for each Ruby install. No wonder I was having problems!

To allow RVM to install to this folder I simply opened the permissions back up:

```
sudo chmod -R 744 ~/.gem
```

RVM is a great project and I recently listened to an interview with the author on the CodePath podcast which I highly recommend.

UPDATE: After posting my finding to the RVM Google Group Wayne informed me that the latest HEAD version of RVM no longer uses the .gem directory but instead uses .rvm. I had put off updating to HEAD as I assumed it would be unstable, but I guess as this is recommended practice that only green code is commited.

```
rvm update --head
```
