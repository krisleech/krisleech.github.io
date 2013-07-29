---
layout: post
title: "Teaching Ruby to children"
date: 2013-07-28 21:01
comments: true
published: true
categories: [ruby]
---

Keeping an audit is slow, so disable it for all specs except those which need it.

1
2
3
4
5
6
7
8
9
10
11
# spec/support/paper_trail.rb
 
RSpec.configure do |config|
  config.before(:each) do
    PaperTrail.enabled = false
  end
 
  config.before(:each, :versioning => true) do
    PaperTrail.enabled = true
  end
end
view rawpaper_trail.rbThis Gist brought to you by GitHub.
1
2
3
4
5
6
7
8
9
10
11
12
13
14
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
view rawstudy_audit_spec.rbThis Gist brought to you by GitHub.
Now simply 'tag' specs at either the it or describe scope with :versioning => true to enable paper_trail for that scope only.