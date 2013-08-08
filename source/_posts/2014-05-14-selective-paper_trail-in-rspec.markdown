---
layout: post
title: "Selective paper_trail in rspec"
date: 2013-07-28 21:01
comments: true
published: true
categories: [ruby]
---

Keeping an audit is slow, so disable it for all specs except those which need it.

```ruby
# spec/support/paper_trail.rb
 
RSpec.configure do |config|
  config.before(:each) do
    PaperTrail.enabled = false
  end
 
  config.before(:each, :versioning => true) do
    PaperTrail.enabled = true
  end
end
```

```ruby
# spec/models/study_audit.rb
 
require 'spec_helper'
 
describe StudyAudit, :versioning => true do
  it 'starts when a new study is created' do
    study = Factory.create(:study)
    audit = StudyAudit.new(study)
    audit.versions.size.should == 1
    audit.versions[0].event.should == 'create'
  end
 
  # ...
end
```

Now simply 'tag' specs at either the it or describe scope with 
`:versioning => true` to enable paper_trail for that scope only.
