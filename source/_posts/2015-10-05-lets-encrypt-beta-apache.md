---
layout: post
title: "LetsEncrypt Beta for Apache"
date: 2015-10-05 09:36
comments: true
published: true
categories: [apache ssl]
---

I received an email today (4/Nov/2015) saying I had been accepted on the [LetEncrypt](https://letsencrypt.org/) Beta programme.

The instructions in the email where not clear, here is what I did to generate my certificate in to Apache.

<!--more-->

On the web server:

```
$ mkdir sources
$ cd sources
$ git clone git@github.com:letsencrypt/letsencrypt.git
$ cd letsencrypt
$ ./letsencrypt-auto --agree-dev-preview --server https://acme-v01.api.letsencrypt.org/directory certonly
```

Provided the above completes okay you will have a new vhost file in `/etc/apache2/sites-avavible`, named `$DOMAIN-le-ssl.conf`.

Edit this file and add the following lines, replacing $DOMAIN with your domain.

```
SSLCertificateFile /etc/letsencrypt/live/$DOMAIN/cert.pem
SSLCertificateKeyFile /etc/letsencrypt/live/$DOMAIN/privkey.pem
SSLCertificateChainFile /etc/letsencrypt/live/$DOMAIN/chain.pem
```

Enable the vhost and restart Apache:

```
$ sudo a2ensite interkonect.com-le-ssl.conf
$ sudo service apache2 reload
```

Your site should now have SSL enabled, you will have to renew your certificate in 90 days.

## Optional

You might want to copy/paste the new vhost file in to an existing vhost for the
domain and enable redirect from HTTP to HTTPS.
