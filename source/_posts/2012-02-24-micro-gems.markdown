---
layout: post
title: "Micro Gems"
date: 2012-02-24 21:01
comments: true
published: true
categories: [ruby]
---

After reading Jeff Kreeftmeijer's blog post on Micro Gems I decided to give it a go.

Presenting Hide the Micro gem to hide Ruby code, a gloryifed comment outterer.

All in all it was quite fun.

I'm not sure adding a method to Kernal is a particully good idea, but thats the only way I could think of to get the functionality everywhere with out having to create a new object and have a syntax such as Hide.this { really anything }.

Here is the spec and implimentation, see the full Gist for README and gemspec:

1
2
3
4
5
6
7
module Kernel
  def noop(&block)
    # do nothing
  end
 
  alias :hide :noop
end
view rawhide.rbThis Gist brought to you by GitHub.
1
2
3
4
5
6
7
require File.expand_path('hide')
 
describe Kernel do
  it 'provides a noop for a block' do
    lambda { hide { raise 'Bad code' } }.should_not raise_error
  end
end
view rawhide_spec.rbThis Gist brought to you by GitHub.
