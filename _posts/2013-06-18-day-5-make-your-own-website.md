---
layout: post
title: "Summer Course 2013: Day 5 Make Your Own Website"
date: 2013-06-21 20:34
categories: 2013 summer courses, HTML, CSS
---

## Chrome


How to see the source and CSS.

### Measureit extension

[measureit](https://chrome.google.com/webstore/detail/measureit/pokhcahijjfkdccinalifdifljglhclm)



## Forms


{% gist 5812295 %}

{% gist 5812296 %}

## Sublime Text 2 Spliting Windows


Configuring and mastering split windows

cmd+shipt+[ or ] to move to next file. 

Add the followings to Preferences>Key binding>User

{% gist 5812472 %}

Read more details in 
[Configuring and mastering split windows in Sublime Text 2 Enhancements](http://shinokada.github.io/blog/2013/06/13/sublime-text-2-enhancements/)

{% highlight text %}
cmd+1,cmd+2,cmd+3 etc to move to other 

alt+cmd+1 to one window
alt+cmd+2 to two vertically split windows
alt+cmd+3 to three vertically split windows

shift+alt+cmd+2 to window
shift+alt+cmd+2 to two vertically split windows
shift+alt+cmd+3 to three vertically split windows

ctl+1 to focus to group 1
ctl+2 to focus to group 2
{% endhighlight %}

## Place holder


[Placehold.it](http://placehold.it/)

[Placeholder Image](http://lorempixel.com/)

[Dynamic dummy image generator](http://dummyimage.com/)



## Individual work 2


### Advanced Assignment 
![assignemt](http://sokada.site44.com/img/assignment2.png)

[Assignment image](http://sokada.site44.com/img/assignment2.png)
 
### Solution
[Advanced Assignment](http://sokada.site44.com/assignment2.html)



## Background Image


#### background-color
	
{% highlight css %}
body
{
	background-color:yellow;
}
h1
{
	background-color:#00ff00;
}
p
{
	background-color:rgb(255,0,255);
}
{% endhighlight %}

#### background-image

{% gist 5812985 %}

{% gist 5812985 %}

#### background-repeat

{% highlight css %}
background-repeat:repeat-y;
/*
repeat
repeat-x
no-repeat
*/
{% endhighlight %}

#### background-size

{% highlight css %}
background-size: 80px 60px;
{% endhighlight %}

#### background-position

{% highlight css %}
background-position:center; 
/* left top
left center
left bottom
right top
right center
right bottom
center top
center center
center bottom
x% y%
*/
{% endhighlight %}

### Advanced background image

[Demo](http://css-tricks.com/examples/FullPageBackgroundImage/css-1.php)

[Perfect Full Page Background Image](http://css-tricks.com/perfect-full-page-background-image/)


## Image Replacement


imagereplacement.html

{% highlight html %}
<h1>My Great Website Name</h1>
{% endhighlight %}

imagereplacement.css

{% highlight css %}
h1{
	background: url(img/mylogo.jpg) no-repeat;
	text-indent: -9999px;
	width: 400px;
	height: 150px;
}
{% endhighlight %}

## Typography


HTML

{% highlight html %}
<h1>My Website Name</h1>
{% endhighlight %}

CSS

{% highlight css %}
h1{
	font-size: 120px;
	text-align: center;
	margin-top: 50px;
	font-family: san-serif; 
	/* helvetica, arial, sans-serif, cursive
	But the user may not have the same font as you have.
	*/
}
{% endhighlight %}	


### letter-spacing
{% highlight css %}
h1{
	letter-spacing: -5px; /* 10px */
	color:#666; /* same as #666666 */
	text-transform: uppercase; /* lowercase */
}

p{
	letter-spacing: -5px; /* looks aweful */ 
}
{% endhighlight %}

### text-shadow
{% highlight css %}
h1{
	text-shadow: 5px 10px 10px #292929;/* 0 1px 0 black very subtle */
	/* x-offset(position, negative is ok) y-offset blur(distance) color*/
}

{% endhighlight %} 
Example 1
{% highlight css %}
body{
	background: #666;
}
h1{
	color:#666;/* or black */
	text-shadow: 0 1px 0 white;/* for glow up use 0 -1px 0 white */
}
{% endhighlight %}

Example 2 Multiple shadow
{% highlight css %}
text-shadow: 
	5px -1px 0 white
	6px -2px 0 red;/* 1px more than the previous*/
{% endhighlight %}
[Other example 1](http://designshack.net/articles/css/12-fun-css-text-shadows-you-can-copy-and-paste/)

[Other example 2](http://line25.com/articles/using-css-text-shadow-to-create-cool-text-effects)

[Demo](http://line25.com/wp-content/uploads/2010/text-shadow/demo/index.html#vintage)

[textshadow generator](http://css3gen.com/text-shadow/)


