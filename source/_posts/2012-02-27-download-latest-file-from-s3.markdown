---
layout: post
title: "Download the latest file from S3"
date: 2012-02-27 21:01
comments: true
published: true
categories: [shell, s3]
---

I was writing a Ruby script which runs shell commands on a remote server as part of a disaster recoveray. I needed to fetch the latest backup for S3. My solution was to use Fog to query the S3 API and use Ruby to figure out which was the most recent filename, then build a command to run on the server. In other words I figure out the command locally and then run it to the server.

But Guy had a better solution which ran server side:

```
s3cmd get `s3cmd ls  s3://my_bucket/back* | tail -1 | awk '{print $4}'`
```

or

```
BACKUP_FILENAME=`s3cmd ls  s3://my_bucket/back* | tail -1 | awk '{print $4}'`
echo "The backup file name is $BACKUP_FILENAME"
s3cmd get $BACKUP_FILENAME
```
