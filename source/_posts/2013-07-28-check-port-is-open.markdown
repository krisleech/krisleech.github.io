---
layout: post
title: "Check port is open in shell script"
date: 2013-07-28 21:01
comments: true
published: true
categories: [ruby]
---

`nc` is used to test for an open port, It returns 1 (non-zero) on failure and 0 (zero) on success.

Here are a couple of examples:

```ruby
#!/bin/sh                                                                       
                                                                                 
nc -z localhost 8983 < /dev/null                                                
 
if [ $? == 0 ]; then                                                            
  bundle exec rake sunspot:solr:reindex                                         
else                                                                            
  echo ''                                                                       
  echo 'Solr is not running, start Solr with "foreman start" and then run "rake sunspot:solr:reindex to update the index"'
  echo ''                                                                       
fi      
```

```bash
#!/bin/sh
echo "Restarting Unicorn on port 9000"
fuser -k 9000/tcp
while nc -vz localhost 9000; do sleep 1; done
bundle exec unicorn_rails -p 9000 -E production -D
while ! nc -vz localhost 9000; do sleep 1; done
curl http://localhost:9000
```
