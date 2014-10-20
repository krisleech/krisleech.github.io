---
layout: post
title: "Incremental scaling of Domain Driven Design"
date: 2014-10-22 09:36
comments: true
published: true
categories: [ruby, rails]
---

This is the first post in a short series about scaling a Domain Driven Design style architecture. Starting at the MVP stage through to profitable business.

<!--more-->

The use case I will look at to begin with is sending email when a bid is placed in a fictitious auction website.

## Entity

```ruby
module Bids
  class Bid < ActiveRecord::Base
    belongs_to :buyer, class_name: Customer
  
    def self.from_form(form)
      new(form.attributes)
    end
  end
end
```

Our model has no validations. This is so we don't restrict ourself with regards to what is considered a valid entity. It is pretty much a simple data object. The `from_form` method is used later to initialize a new entity from a form object.

## Form

```ruby
module Bids
  class Create::Form
    include ActiveModel::Model
    include Virtus.model
  
    attribute :buyer_id,    Integer
    attribute :auction_id,  Integer
    attribute :maximum_bid, Money
  
    validates :buyer_id,    presence: true
    validates :auction_id,  presence: true
    validates :maximum_bid, presence: true
  end
end
```

We create a "form" with which to capture, validate and sanitise user input. This will be used to create a new `Bid` entity.

The [Virtus](https://github.com/solnic/virtus) gem provides us with an `attribute` macro which coerces values to the correct type and an initializer which given a hash will set the attributes.

## Service object

```ruby
module Bids
  class Create
    include Wisper::Publisher
  
    def call(form)
      if form.valid?
        bid = Bid.from_form(form)
        bid.save!
      
        broadcast(:create_bid_successful, bid.id)
      else
        broadcast(:create_bid_failed, bid.id)
      end
    end
  end
end
```

The service will take the `form` and use it to create a new `Bid` entity. It will broadcast an event, using [Wisper](https://github.com/krisleech/wisper), to signal the outcome.

While not shown here I use the `initialize` method for injecting dependencies to make unit testing easier.

## Controller

```ruby
module Bids
  class CreateController < ApplicationController
	def new
	  @form = new_form
	end

	def create
	  @form = new_form
	  create_bid = Bids::Create.new(@form)
	  
	  create_bid.on(:create_bid_successful) { |bid_id| redirect_to bid_path(bid_id) }
	  create_bid.on(:create_bid_failed)     { |bid_id| render action: :new }
	  
	  create_bid.call
	end

	private

	def new_form
	  Create::Form.new(params[:form])
	end
  end
end
```

This is the context which brings each part together. I've not included the view the user sees but you can imagine the use of `form_for` to show a HTML form which the user can interact with.

Now imagine we want to send an email to the seller to notify them of the new bid.

We could put the mailing code directly in the service object. However I see email as a UI and not part of the core business logic, its on the outside of the [Hexagon](http://alistair.cockburn.us/Hexagonal+architecture). So instead, in the controller, we can subscribe a listener to our service object which will react to the `create_bid_successsful` event.

```ruby
create_bid.subscribe(Orders::Notifier.new, prefix: 'on')
```

## Listener

```ruby
module Bids
  class Notifier
    def on_create_bid_successful(bid_id)
      bid = Bid.find(bid_id)
    
      Mailer.bid_created(bid).deliver
    end
  end
end
```

### and Mailer

```ruby
module Bids
  class Notifier::Mailer < ActionMailer
    # the usual...
  end
end
```

It would be nice if we didn't have to have a separate mailer class, but `ActionMailer` makes this tricky. Typically I'd have the listener and mailer in the same file.

## Summary

With this code we have a clear separation of concerns, the boundary and responsibility of each object is clear. The simplicity of each object means that they can be used in different contexts. In this case a web application, but equally a native app or REST API.

Objects either tell other objects what to do or tell them that something happened. They don't ask for bits of state and act on them as this would create an unnecessary dependency on the internals of the other object.

I've nested everything related to the concept of 'bids' in a `Bids` module, this is something like a [bounded context](http://martinfowler.com/bliki/BoundedContext.html) in DDD and provides a neat way of later pulling out a particular concept in to its own service.
