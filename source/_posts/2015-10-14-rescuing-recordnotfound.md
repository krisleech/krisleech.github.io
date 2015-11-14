---
layout: post
title: "Rescuing RecordNotFound is too broad"
date: 2015-10-14 09:36
comments: true
published: true
categories: [ruby]
---

Because rescuing `ActiveRecord::RecordNotFound` rescues missing records from all models it can produce unexpected behaviour which is hard to trace.

I recommend doing yourself a favour and logging any exceptions which are rescued by not re-raised and being explicit about the model from which you want to rescue `RecordNotFound` using the [NotFound](https://github.com/krisleech/not_found) gem.

<!-- more -->

We had a fairly regular controller, here is an extract.

```ruby
rescue_from ActiveRecord::RecordNotFound do
  render action: 'not_found'
end

def show
  @study = Study.find(study_id)
end

def destroy
  command = DeleteStudy.new
  command.on(:success) { flash[:notice] = "The study was successfully deleted" }
  command.on(:failed)  { flash[:notice] = "The study was NOT deleted" }
  command.execute(study_id)
  redirect_to studies_path
end
```

Looks okay, right? The problem was that the destroy action was not redirecting to the index action. Instead we where getting the not found page being rendered. At first I thought it was redirecting to the show action and then rendering the not found because the model had been deleted. I didn't write some of the code in the controller and that which I did I hadn't looked at in years.

It took some time to figure out the problem.

It turns out that we made a change to an engine which was subscribing to a [Wisper](https://github.com/krisleech/wisper) event broadcast from the `DeleteStudy` command.

Due to a bug in the listener a `ActiveRecord::RecordNotFound` exception was being raised on a different model. But this exception was being rescued by `rescue_from` and the not found page was being rendered. Because the exception was being silently swallowed it was not apparent what was going on.

I decided two things, one if you rescue an exception and don't re-raise the same exception it should be logged and second we shouldn't rescue such a broad exception as `ActiveRecord::RecordNotFound`. I wrote a [tiny gem](https://github.com/krisleech/not_found) which raises a model specific `RecordNotFound` exception, so we can be explicit about the model from which we want to rescue`ReecordNotFound`. The new code looked something like this:

```ruby
rescue_from Study::RecordNotFound do |e|
  log_swallowed_exception(e)
  render action: 'not_found'
end

def destroy
  command = DeleteStudy.new
  command.on(:success) { flash[:notice] = "The study was successfully deleted" }
  command.on(:failed)  { flash[:notice] = "The study was NOT deleted" }
  command.execute(study_id)
  redirect_to studies_path
end

private

# in ApplicationController
def log_swallowed_exception(e)
  logger.debug("#{e.class.name} #{e.message}\n #{e.backtrace.take(10).join("\n"))
end
```

We log the exception and rescue `Study::RecordNotFound` instead of
`ActiveRecord::RecordNotFound`.
