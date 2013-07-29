---
layout: post
title: "Teaching Ruby to children"
date: 2013-07-28 21:01
comments: true
published: true
categories: [ruby]
---

Torquebox and Sunspot
30/03/2012

Threadsafe
Turn threadsafe on in production/staging, but not development. In development environment I got loads of 'random' errors, my guess would be code reloading is not threadsafe.

Avoiding java.lang.OutOfmemoryError
Normally setting the heap size is something like jruby -XXX or export $JAVA_OPTS='-XX'. We can't pass options to jruby because we will be using the torquexbox run wrapper to start JBoss and we can't use export because JBoss exports its own JAVA_OPTS which we would overwrite.

The solution is to merge our JAVA_OPTS with the JBoss ones.

torquebox run --jvm-options=-Xmx1024m

https://github.com/torquebox/torquebox/commit/e9a7ece9d5413e87263df3b6ba92f565ddc2f3d9

Firewall
It is critical you install a firewall and open ports for HTTP, SSH and HTTPS. The regular Ruby/Rails app servers only open a single HTTP port, where as JBoss and Solr will also open ports for administration which you do not want publically exposed. Of course a firewall is always recommended even on an Apache/Mongrel type setup but it is essential when using Torquebox and Solr.

Nginx
You can set the number of maximum threads at the top of ngnix.conf.

Database pool
In database.yml set the pool to a high number, say 100.

Facets Lmit
My client noticed that some categories where not showing up for a search when they should be. I could easily reproduce the problem locally and after a whole day of trying everything I could think of to isolate the cause and getting nowhere I emailed the Sunspot Google Group. Thanks to Mauro Asprea he pointed out I was getting exactly 100 facets returned and there was some limit in Solr. This is one of those situations which did not show up in my tests since they use such a small dataset.

UnlimitedFacets = -1

facet :category_ids, :limit => UnlimitedFacets