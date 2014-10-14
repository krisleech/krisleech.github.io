---
layout: post
title: "Installing and publishing events to Routemaster"
date: 2014-10-10 09:36
comments: true
published: true
categories: [ruby, rails, soa]
---

In this post I want to cover installing and publishing events to the excellent [RouteMaster](https://github.com/HouseTrip/routemaster) event bus.

> Routemaster is an opinionated event bus over HTTP, supporting event-driven / representational state notification architectures.

RouteMaster uses [RabbitMQ](https://www.rabbitmq.com) under the hood.

<!--more-->

##  Routermaster

First of all install and start [RouteMaster](https://github.com/HouseTrip/routemaster):

```
hub clone HouseTrip/routemaster

cd routermaster

bundle install 

foreman start
```

## RabbitMQ

Install and start [RabbitMQ](https://www.rabbitmq.com):

```
brew install rabbitmq

rabbitmq-server
```

If `rabbitmq-server` is not found make sure `/usr/local/sbin` is in your
`$PATH`.

### Create a Virtual Host

* Visit [http://localhost:15672/#/](http://localhost:15672/#/)
* Login with guest/guest (the default user for RabbitMQ)
* Go to Admin => Virtual hosts => add a new virtual host called `routemaster.development`
* The virtual host can be changed in `.env`, see `ROUTEMASTER_AMQP_URL`.

### Add a user to the Virtual Host

Add the `admin` user with full permissions to the newly created virtual host.

## Setup port and SSL proxy

Since Routemaster only excepts SSL connections we need to jump through a few hoops.

We want this proxying:

> https://localhost:443 -> http://localhost:80 -> http://routemaster.dev:15672

`15672` is the default port which RouteMaster binds to and can be changed in `.env`.

* Forward port 443 (HTTPS) to 80 (HTTP)

```
gem install tunnels
rbenv rehash
sudo tunnels 127.0.0.1:443 127.0.0.1:80
```

* Forward `localhost:80` to `routemaster.dev:15672` using [Port proxying](http://pow.cx/manual#section_2.1.4) in [Pow](https://github.com/basecamp/pow)

```
brew install pow
# follow the post install instructions...

echo 17890 > ~/.pow/routemaster

pow
```

## Publish an Event

Now the fun starts...

```
gem install routemaster-client
irb
```

In our `irb` session we are going to try and connect to RouteMaster:

```ruby
require 'routemaster/client'

client = Routemaster::Client.new(url: 'https://routemaster.dev', uuid: ‘demo’)
```

The UUID `demo` is already known to RouteMaster see `ROUTEMASTER_CLIENTS` in
`.env`.

### Troubleshooting

If you get a `Timeout` error most likely the port proxying is not setup correctly. Ensure `pow` is running.

A `cannot connect to bus` error is most likely an authentication issue between RouteMaster and RabbitMQ. 
Check the output of `foreman` to confirm, you might see something like `Bunny::PossibleAuthenticationFailureError`. 
If so make sure the virtual host is setup and the `guest` user is assigned with full permissions:

```
rabbitmqctl list_users
rabbitmqctl list_user_permissions guest

# routemaster.development .* .* .*
```

### Sending a create event

RouteMaster only supports a small set of event types: `create`, `update`, `delete` and `noop`.

If the client connected try publishing an event:

```ruby
client.created('orders', 'https://app.example.com/orders/1') # => nil
```

The first argument is the topic, which is automatically created if it does not exist. There should be one topic per domain concept. The URL is where the subscribers can fetch a representation of the created entity.

You should see output in foreman since RouteMaster logs to STDOUT by default, e.g. `POST /topics/widgets HTTP/1.1" 200`.
