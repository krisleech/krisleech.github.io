---
layout: post
title: "Multi-host Vagrant and private networks"
date: 2014-11-24 09:36
comments: true
published: true
categories: [vagrant]
---

Problems accessing HTTP applications using the IP of your Vagrant boxes?

<!--more-->

I have three Vagrant boxes all provisioned from a single Vagrantfile. The HTTP
apps where starting up fine and if I ssh in to the machine I could `curl localhost:3000`
and get the expected response.

However while the guest machines where up and pingable from the host I got no
response from the HTTP port.

The problem it turns out is Ubuntu has the ufw firewall installed which blocks all incoming connections
by default.

When provisioning the machine either disable the firewall or punch a hole in it for your ports:

```
sudo ufw disable
```

```
sudo ufw allow 3000/tcp
```

Here is a snippet from my Vagrantfile:

```ruby
Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  config.vm.box = "ubuntu/trusty64"

  config.vm.define "jukebox" do |jb|
    jb.vm.network "private_network", ip: "192.168.1.1"
    jb.vm.hostname = "jukebox.dev"

    $script = <<-SCRIPT

    sudo ufw allow 3000/tcp

    # ...

    SCRIPT

    jb.vm.provision "shell", inline: $script
  end
end
```
