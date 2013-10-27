---
layout: post
title: "linux on HP Pavilion g series Laptop"
date: 2013-07-19 21:01
comments: true
published: true
categories: [linux]
---

Each g series has a specific model, mine is g6-1394sa, the model is found under the battery.

I installed the CrunchBang distro which first heard about in an article on Ruby Source.

CrunchBang is Debian based. Therefore I would assume that any Debian distro (e.g Ubuntu) will run on any of the HP Pavilion g series laptops. If you can confirm this please leave a comment.

I didn't have to do anything special to use wireless or sound. I've not yet tried the built-in webcam.

The only thing I found not to work is waking after suspend/hibernation. After shutting the laptop it would wake, but the screen remained blank. This is kind of annoying and I am trying to figure out how to fix it. I think it is something to do with the graphics card. Any tips welcome! UPDATE: I fixed this, see bottom of article.

CrunchBang is an excellent disto, normally I would install one of the light Ubuntu distro's, but CrunchBang is, for me, an even better option. It uses the OpenBox window manager and has little but the basics pre-installed. However it does have links to install common software which would normally come installed as part of the distro and of course there is the usual aptitude / apt-get. Because of the no-frills minimal nature of CrunchBang it is very quick, booting in a few seconds (on a SSD).

Fixing suspend

Install graphics driver

```
sudo apt-get install fglrx-driver
sudo aticonfig --initial
```

Disable kernel mode setting (KMS):

vi /etc/default/grub

and add:

```
GRUB_CMDLINE_LINUX="nomodeset"
```
 
reboot and close the lapptop lid or from terminal sudo pm-suspend.
If this does not work you can change "shutting lid of laptop" to do nothing in System > Power Management.

References for fixing suspend/hibernation:

http://h10025.www1.hp.com/ewfrf/wc/document?docname=c03382804&tmp_task=prodinfoCategory&cc=uk&dlc=en&lc=en&product=5278396
http://ubuntuforums.org/showthread.php?t=1475009
http://www2.ati.com/relnotes/catalyst_linux_installer.pdf
http://wiki.debian.org/Suspend
https://wiki.archlinux.org/index.php/AMD_Catalyst#Disable_kernel_mode_setting
http://ubuntuforums.org/showthread.php?t=2003532
