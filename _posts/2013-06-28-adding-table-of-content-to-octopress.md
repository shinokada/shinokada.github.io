---
layout: post
title: "Adding Table of Content to Octopress"
date: 2013-06-29 16:46
categories: Octopress
---


## _config.yml

Open _config.yml and change markdown.
	
{% highlight ruby %}
markdown: kramdown
{% endhighlight %}




## Adding TOC code to your post
Use the following to your content.
	
{% highlight text %}
{:.no_toc}

* Will be replaced with the ToC, excluding the "Contents" header
{:toc}
{% endhighlight %}


The following is Sublime Text 2 snippet for this toc.

	
{% highlight text %}
<snippet>
	<content><![CDATA[
{:.no_toc}

* Will be replaced with the ToC, excluding the "Contents" header
{:toc}
]]></content>
	<!-- Optional: Set a tabTrigger to define how to trigger the snippet -->
	<tabTrigger>toc</tabTrigger>
	<!-- Optional: Set a scope to limit where the snippet will trigger -->
	<scope>text.html.markdown</scope>
</snippet>
{% endhighlight %}


## Adding ToTop code to your post


I use the following for a internal link to top.

	
{% highlight text %}

{% endhighlight %}


The following is the Sublime Text 2 snippet.
	
{% highlight text %}
<snippet>
	<content><![CDATA[

]]></content>
	<!-- Optional: Set a tabTrigger to define how to trigger the snippet -->
	<tabTrigger>top</tabTrigger>
	<!-- Optional: Set a scope to limit where the snippet will trigger -->
	<scope>text.html.markdown</scope>
</snippet>
{% endhighlight %}


## Adding CSS


Open octopress/sass/custom/_style.scss and add the followings.

	
{% highlight text %}
#markdown-toc:before {
  content: "Table of Contents";
  font-weight: bold;
}
ul#markdown-toc {
  list-style: none;
  float: right;
  @include shadow-box;
  background-color: white;
  margin: 10px 0 20px 10px;
}

.gist, .bogus-wrapper{
	clear: both;
	margin-top:10px;
}
{% endhighlight %}


