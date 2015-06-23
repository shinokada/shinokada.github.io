---
layout: post
title: "Rspec notes"
date: 2013-10-21 21:01
comments: true
categories: Ruby
---

<!-- more -->

{:.no_toc}

* Will be replaced with the ToC, excluding the "Contents" header
{:toc}

Video from Efficient Rails Test-Driven Development by Walfram Amold

## RSpec verifications
[&#8629; TOP](#markdown-toc)

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
[&#8629; TOP](#markdown-toc)
	
{% highlight ruby %}
before, before(:each), before(:all)
after, after(:each), after(:all)
describe do...end, nested
it do...end
{% endhighlight %}

## Practical
[&#8629; TOP](#markdown-toc)

{% highlight text %}
$ rails address_book
$ cd address_book
$ script/generate rspec
$ script/generate rspec model Person first_name:string last_name:string
{% endhighlight %}

## Specify/it and context/describe
[&#8629; TOP](#markdown-toc)

"context" and "specify" are the aliases of "describe" and 
"it".

## &:to_s
[&#8629; TOP](#markdown-toc)

{% highlight ruby %}
v.map(&:to_s)
# Is the same as:
v.map { |i| i.to_s }
{% endhighlight %}

## Enumerable classes
[&#8629; TOP](#markdown-toc)

This code will display all enumerable classes.

{% highlight ruby %}
ObjectSpace.each_object(Class) {|cl| puts cl if cl < Enumerable}
{% endhighlight %}


