---
layout: post
title: "Summer Course 2013: Day 3 Make Your Own Website"
date: 2013-06-19 10:12
comments: true
categories: [2013 summer courses, HTML, CSS]
---

<!-- more -->

{:.no_toc}

* Will be replaced with the ToC, excluding the "Contents" header
{:toc}

## Review
[&#8629; TOP](#markdown-toc)

showing hidden files and folders, spectacle shortcuts, sublime text 2 skin, stackoverflow, html5 snippet, div, class, id, first-type-of, height, margin, padding.


## Installing sublime text 2, SideBarEnhancements
[&#8629; TOP](#markdown-toc)

cmd+shift+p, install, Sidebarenhancements to install.

Open Sublime Text 2 > Preferences > Browse Packages > User > SideBarEnhancements > Open With > Side Bar.sublime-menu

{% endhighlight %} json
{% gist 5796310 %}
{% endhighlight %}

Right click on a file in sublime text 2 and select Open in Browser.


## Comments in HTML and CSS
[&#8629; TOP](#markdown-toc)

HTML

	
{% highlight html %}
<!-- here you add your comments -->
{% endhighlight %}


CSS

	
{% highlight css %}
/* here you add your comments */
{% endhighlight %}


## CSS properties
[&#8629; TOP](#markdown-toc)

- border none, dotted, dashed, solid, double, groove, ridge, inset, outset
- text-align, center, left, right


## margin auto
[&#8629; TOP](#markdown-toc)

margin with auto

	
{% highlight css %}
.container{
	background-color: white;
	width: 300px;
	height: 600px;
	margin: 0 auto;
	border: 10px solid #ccc;
}
{% endhighlight %}


## blockquote, pre, code
[&#8629; TOP](#markdown-toc)

	
{% highlight html %}
<blockquote>
	<p>
	blockquote tag
	somethng here.
	something here.
	</p>
</blockquote>

<pre>
	pre tag
	First line
	second line
	third line
</pre>

<code>
	code tag
	First line
	second line
	third line
</code>
{% endhighlight %}
		

		
### Styling blockquote

#### Example 1

	
{% highlight css %}
blockquote {
	margin: 1em 3em;
	color: #999;
	border-left: 2px solid #999;
	padding-left: 1em; 
}
{% endhighlight %}

#### Example 2
	
{% highlight css %}
blockquote 
{
	margin: 1em 3em;
	padding: .5em;
	background-color: #f6ebc1; 
}

blockquote p {
	margin: 0; 
}
{% endhighlight %}
		
		
#### More examples

[CSS trick](http://css-tricks.com/examples/Blockquotes/)

[codrops](http://tympanus.net/codrops/2012/07/25/modern-block-quote-styles/)


## Anchor
[&#8629; TOP](#markdown-toc)

### target
	
{% highlight html %}
<a href="http://www.google.com/" target="_blank">Visit Google</a>

<a href="http://www.google.com/" target="_self">Visit Google</a>
{% endhighlight %}


### Same level anchor, One level up, sibling folder

	
{% highlight html %}
// folder1/anchor.html
<ul>
	<li><a href="index.html">Folder Index.html</a></li>
	<li><a href="../index.html">One level up index.html</a></li>
	<li><a href="index.html" target="_blank">Same level Folder1/index.html with a new tab</a></li>
	<li><a href="../folder2/index.html">Folder2/index.html</a></li>
</ul>
{% endhighlight %}



### Internal links

[internal links](http://sokada.site44.com/folder1/anchor.html)

	
{% highlight html %}
<ul >
    <li><a href="#link1">
     Internal link 1</a></li>
    <li><a href="#link2">
     Internal link 2</a></li>
     <li><a href="#link3">
     Internal link 3</a></li>
     <li><a href="#link4">
     Internal link 4</a></li>
</ul>

<div id="link1">
    <h1>Link1</h1>
    <p>Lorem ipsum dolor sit amet, consectetur adipisicing elit. Unde voluptas suscipit velit minima quisquam. 
    Temporibus doloremque amet nihil nobis in quod facere cum voluptates eaque nulla sequi consequuntur rerum sunt.
    Repellat nam tempore officiis numquam recusandae animi mollitia velit quia.</p>
    <h3><a href="#top">Top</a></h3>
</div>
{% endhighlight %} 


## Project Structures
[&#8629; TOP](#markdown-toc)
	
{% highlight text %}
- index.html
|- css 
|- img
|-js

{% endhighlight %}


Update all css links.

	
{% highlight html %}
<link rel="stylesheet" href="css/mystyle.css">
// Or
<link rel="stylesheet" href="../css/mystyle.css">
{% endhighlight %}


    
## Images
[&#8629; TOP](#markdown-toc)    

In index.html page type the following including alternate text. alt means "Specifies an alternate text for an image".
	
{% highlight html %}
<img src="img/builder.jpg" alt="builder">
{% endhighlight %}

				
You can specify the dimention of the image.
	
{% highlight html %}
<img src="img/builder.jpg" alt="builder" height="42" width="42">
{% endhighlight %}



Don't do hotlinkig. Finding a link by right-click an image and Copy image URL.

- You are using other's bandwidth. 
- You have no control

Example
	
{% highlight html %}
<img src="http://cdn.tutsplus.com/net.tutsplus.com/uploads/2013/06/laravel-plus-backbone-200.jpg" alt="builder">
{% endhighlight %}



You must use an image which you are allowed to use it. Your own images or [creative common](http://www.flickr.com/creativecommons/) images.


## Emmet Doc
[&#8629; TOP](#markdown-toc)

[Emmet doc](http://docs.emmet.io/abbreviations/syntax/)

[7 Awesome Emmet HTML Time-Saving Tips](http://designshack.net/articles/css/7-awesome-emmet-html-time-saving-tips/)

