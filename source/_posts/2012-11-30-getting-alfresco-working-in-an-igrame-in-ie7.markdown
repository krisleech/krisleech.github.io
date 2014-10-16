---
layout: post
title: "Getting Alfresco Share working in an iframe in IE7"
date: 2012-11-30 21:01
comments: true
published: true
categories: [apache, alfresco]
---

Problem: "Cookie disabled" pop-up in IE7 when using Alfresco share in an iframe.

Summery: You need to add a P3P header to all responses from Share. The P3P header must link to an XML file which tells IE to allow the iframe to have its own cookies.

<!--more-->

The easiest way to do this is to proxy Alfresco share through Apache and use mod_headers to add the header to every request.

CONFIGURE APACHE

Create a subdomain and point it at the server, this will be the domain sourced in the iframe.

/etc/apache2/apache2.conf

```
<VirtualHost *:80>
  ServerName share.mydomain.com
  DocumentRoot /home/ubuntu/share
 
  ProxyRequests off
  ProxyPass /w3c/ !
  ProxyPass / http://127.0.0.1:8080/
  ProxyPassReverse / http://127.0.0.1:8080/
  ProxyPreserveHost on
 
  Header add p3p 'P3P:policyref="/w3c/p3p.xml", CP="NID DSP ALL COR"'
</VirtualHost>
```

http://share.mydomain.com/w3c/p3p.xml must respond with a P3P XML file which tells the iframe to allow cookies.

The easiest way to do this is to add the file to the DocumentRoot of the share VirtualHost, i.e /home/ubuntu/share/w3c/p3p.xml

Note /home/ubuntu/share is an arbitrary empty directory and not related to the Share/Alfresco install.

```xml
<META xmlns="http://www.w3.org/2002/01/P3Pv1">
 
  <POLICY-REFERENCES>
 
    <POLICY-REF about="#policy1">
 
      <INCLUDE>/</INCLUDE>
 
      <COOKIE-INCLUDE/>
 
    </POLICY-REF>
 
  </POLICY-REFERENCES>
 
  <POLICIES>
 
    <POLICY discuri="" name="policy1">
 
      <ENTITY>
 
        <DATA-GROUP>
 
          <DATA ref="#business.name"></DATA> 
 
          <DATA ref="#business.contact-info.online.email"></DATA> 
 
        </DATA-GROUP>
 
      </ENTITY>
 
      <ACCESS>
 
        <nonident/>
 
      </ACCESS>
 
      <!-- if the site has a dispute resolution procedure that it follows, a DISPUTES-GROUP should be included here -->
 
      <STATEMENT>
 
        <PURPOSE>
 
          <current/>
 
          <admin/>
 
          <develop/>
 
        </PURPOSE>
 
        <RECIPIENT>
 
          <ours/>
 
        </RECIPIENT>
 
        <RETENTION>
 
          <indefinitely/>
 
        </RETENTION>
 
        <DATA-GROUP>
 
          <DATA ref="#dynamic.clickstream"/>
 
          <DATA ref="#dynamic.http"/>
 
        </DATA-GROUP>
 
      </STATEMENT>
 
    </POLICY>
 
  </POLICIES>
 
</META>
```

UPDATE HOSTS FILE

Update hosts file so requests from server to share.mydomain.com are not routed out to the internet back, but just forwarded to 127.0.0.1.

/etc/hosts

127.0.0.1 share.mydomain.com

Check it works: http://www.w3.org/P3P/validator.html
