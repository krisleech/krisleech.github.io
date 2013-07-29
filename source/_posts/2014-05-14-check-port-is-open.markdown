---
layout: post
title: "Teaching Ruby to children"
date: 2013-07-28 21:01
comments: true
published: true
categories: [ruby]
---

Check port is open in shell script
nc is used to test for an open port, it returns 1 on failure and 0 on success.

Here is a couple of examples:

1
2
3
4
5
6
7
8
9
10
11
12
13
14
15
#!/bin/sh                                                                       
                                                                                 
bundle install                                                                  
bundle exec rake db:migrate                                                     
bundle exec rake db:test:prepare
 
nc -z localhost 8983 < /dev/null                                                
 
if [ $? == 0 ]; then                                                            
  bundle exec rake sunspot:solr:reindex                                         
else                                                                            
  echo ''                                                                       
  echo 'Solr is not running, start Solr with "foreman start" and then run "rake sunspot:solr:reindex to update the index"'
  echo ''                                                                       
fi      
view rawupdate.shThis Gist brought to you by GitHub.
1
2
3
4
5
6
7
#!/bin/sh
echo "Restarting Unicorn on port 9000"
fuser -k 9000/tcp
while nc -vz localhost 9000; do sleep 1; done
bundle exec unicorn_rails -p 9000 -E production -D
while ! nc -vz localhost 9000; do sleep 1; done
curl http://localhost:9000
