---
layout: post
title: "Summer Course 2013: Day 6 Make Your Own Website"
date: 2013-06-24 20:35
categories: 2013 summer courses, HTML, CSS
---

## Review


Measureit, form, spliting windows, placeholder, assignment 2, background image, repeat, background-position, background-size, image replacement, negative indent, typography, letter-spacing, text-shadow.

[Summer Course 2013: Day 5 Make Your Own Website](http://shinokada.github.io/blog/2013/06/21/day-5-make-your-own-website/)



## Starting Your Own Project


You learned a lot during the last week. There are more to learn, but you have enough knowledge and skills to start working on your website. If you can, use your logo, images and text for your project. You need to work this project at home. 



## Solution to Assignment 2


[Assignment 2](http://sokada.site44.com/assignment2.html)



## More Typography


### font-variant
This sets to a small-caps font.

	h1{
		font-variant: small-cpas;
	}

### Google font

[Google fonts](http://www.google.com/fonts/)

	<link href='http://fonts.googleapis.com/css?family=Lily+Script+One' rel='stylesheet' type='text/css'>
	<link rel="stylesheet" href="css/typography.css">


	body{
	 	font-family: 'Lily Script One', cursive;
	}



## CSS clear


[Demo](http://sokada.site44.com/clear.html)

	clear: both|left|right;


{% highlight text %}
{% gist 5829613 %}
{% endhighlight %}


## Sublime Text 2 


### Instant File Changing

	cmd+p (goto anything), 

Fuzzy searching and this will load the file automatically.

### How to set syntax,

	shift+cmd+p 

Type syntax and select a syntax you want to use.

### Adding comment

	cmd / 
	opt+cmd+/

The first one will give a comment. The second one will give a block comment.



## Relative and Absolute Positioning


position.html

	<div class="box"></div>

position.css
	
	.box{
		width:400px;
		height: 400px;
		background: red;
		margin: auto;	
	}

Take out the browser margin.

	body{margin: 0;}

### position: relative

A relative positioned element is positioned relative to its normal position.

	.box{
		width:400px;
		height: 400px;
		background: red;
		position: relative;
		/*margin:auto; */
		/*top: 200px; */
		/*left: 200px; */
		/*right: 200px;*/
		/*botom : 200px;*/
	
	}



### position: absolute

This positions to relative to the parent. Compare relative and absolute.

HTML

		<div class="box1"></div>

CSS

	.box1{
		width:400px;
		height: 400px;
		background: green;
		/*position: relative;*/
		position: absolute;
		left:20px;
		top: 30px;
	}

**Relatively positioned elements are often used as container blocks for absolutely positioned elements.**

HTML

	<div class="wrap">
	<div class="box"></div>
	</div>

CSS

	.wrap{
		width: 800px;
		height: 1000px;
		margin: auto;
		background: #e3e3e3;
		/* You need to add the following*/
		posistion:relative;
	}
	
	.box{
		width: 400px;
		height: 400px;
		background: green;
		position: absolute;
		left:20px;
		top: 30px;
	}

You need to add posistion:relative to the parent. Without it, the nearest parent is the window.



### position: fixed

An element with fixed position is positioned relative to the browser window.

	p.pos_fixed
	{
		position:fixed;
		top:30px;
		right:5px;
	}


## Assignment 3



![Assignment 3 Image](http://sokada.site44.com/img/assignment3.png)	

[Assignment 3](http://sokada.site44.com/img/assignment3.png)

HTML: assignment3.html

{% highlight html %}
{% gist 5822001 %}
{% endhighlight %}

CSS Skelton: assignment3.css

{% highlight css %}
{% gist 5822008 %}
{% endhighlight %}


### Solution will be posted in the next lesson


