---
layout: post
title: "Enforcing Price Plans"
date: 2010-03-13 21:01
comments: true
categories: [ruby]
---

In my my controllers I am restricting access to features based on the users assigned price plan, which is an integer, mappable to a label such as 'Gold', 'Silver', 'Bronze'.

<!--more-->

As well as providing autherisation at the controller layer, where it belongs, I also wish to enforce this at the model layer. This gives me defense in depth and will also alert me to holes in the autherisation at the controller level where testing dare not tread.

I have a YAML file which has a list of methods (the key) and minimum price plan (the value). At the top of my model I specify which methods I want to protect. At the bottom of model I then alias each method and check the YAML file to make sure the price plan is sufficient, if not I raise an exception, otherwise I call the original method.

```ruby
class Account

  RestrictedMethods = %w(enable_logging backup)
  
  def enable_logging
  # do something
  end
  
  def backup
  # do something
  end

  def allow?(action)
    YAML.load(File.join(RAILS_ROOT, 'config', 'price_plan.yml'))[action.to_s].to_i >= price_plan
  end
  

  RestrictedMethods.each do |method_name| 
      str_id = "__#{method_name}__hooked__"
      unless private_instance_methods.include?(str_id)
        alias_method str_id, method_name       
        private str_id                          
        define_method method_name do |*args|    
          raise PricePlanRestrictedException unless allow? method_name
          __send__ str_id, *args  # Invoke backup
        end
      end
    end
end    
```

The YAML file, price_plan.yml, looks like this:

```yaml
backup: 5
enable_logging: 10            
```

This was largely inspired by the following post: http://cheind.blogspot.com/2008/12/method-hooks-in-ruby.html
