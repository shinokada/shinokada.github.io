---
layout: post
title: "Rspec notes"
date: 2013-10-21 21:01
categories: Ruby
---


Video from Efficient Rails Test-Driven Development by Walfram Amold

## RSpec verifications

{% highlight ruby %}
should respond_to
# works with any ? method so-called predicates
should be_nil
should be_valid
should_not be_nil; should_not be_valid
lambda{...}.should change(), {}, .from().to(), .by()
should eql, ==, equal

{% endhighlight %}

## RSpec structure
	
{% highlight ruby %}
before, before(:each), before(:all)
after, after(:each), after(:all)
describe do...end, nested
it do...end
{% endhighlight %}

## Practical

{% highlight text %}
$ rails address_book
$ cd address_book
$ script/generate rspec
$ script/generate rspec model Person first_name:string last_name:string
{% endhighlight %}

## Specify/it and context/describe

"context" and "specify" are the aliases of "describe" and 
"it".

## &:to_s

{% highlight ruby %}
v.map(&:to_s)
# Is the same as:
v.map { |i| i.to_s }
{% endhighlight %}

## Enumerable classes

This code will display all enumerable classes.

{% highlight ruby %}
ObjectSpace.each_object(Class) {|cl| puts cl if cl < Enumerable}
{% endhighlight %}


