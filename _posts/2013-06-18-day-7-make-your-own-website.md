---
layout: post
title: "Summer Course 2013: Day 7 Make Your Own Website"
date: 2013-06-25 20:53
categories: 2013 summer courses, HTML, CSS
---


## Livereload with Chrome/Sublime Text 2 


- shift+cmd+p and install livereload
- Install [Chrome extension Livereload](https://chrome.google.com/webstore/detail/livereload/jnihajbhpnppcggbcgedagnkighmdlei?hl=en)
- Go to chrome [extension](chrome://extensions/) and tick Allow access to file URLs.
- Load your website in Chrome and enable livereload. Save html or css to see the website without refresh it.


## Assignment 3 Solution
 

HTML Solution: assignemt3.html

{% gist 5822180 %}

CSS Solution: assignment3.css

{% gist 5822074 %}

## Table
 

### Table basics

table.html
	
{% highlight html %}
table[border=1]>(tr>td*2)*2

<table border="1">
<tr>
<td>row 1, cell 1</td>
<td>row 1, cell 2</td>
</tr>
<tr>
<td>row 2, cell 1</td>
<td>row 2, cell 2</td>
</tr>
</table>
{% endhighlight %}


### Attributes

width
	
{% highlight html %}
<table border="1" width="30%">

<td align="left|center|right" valign="top|middle|bottom|baseline">Cell 1</td>
{% endhighlight %}



align="left|center|right" in HTML is the same as td{text-align:right;} But CSS has more effect over HTML.

valign="top|middle|bottom|baseline" in HTML is the same as td{vertical-align: top;}

	
{% highlight html %}
<td align="right" valign="top">Cell 1</td>
{% endhighlight %}

	


### colspan

Example 1
	
{% highlight html %}
<table border="1">
  <tr>
	<td colspan="3">Cell 1</td>
  </tr>
  <tr>
	<td>Cell 2</td>
	<td>Cell 3</td>
	<td>Cell 4</td>
  </tr>
</table>
{% endhighlight %}



Example 2
	
{% highlight html %}
<table border="1">
  <tr>
	<td colspan="2">Cell 1</td>
	<td>Cell 2</td>
  </tr>
  <tr>
	<td>Cell 3</td>
	<td>Cell 4</td>
	<td>Cell 5</td>
  </tr>
</table>
{% endhighlight %}


### rowspan
	
{% highlight html %}
<table border="1">
  <tr>
	<td rowspan="3">Cell 1</td>
	<td>Cell 2</td>
  </tr>
  <tr>
	<td>Cell 3</td>
  </tr>
  <tr>
	<td>Cell 4</td>
  </tr>
</table>
{% endhighlight %}




### Pretty Table Examples

[Table examples](http://sokada.site44.com/table.html)

[Read more about Table Design](http://designshack.net/articles/css/15-tips-for-designing-terrific-tables/)

[table generator](http://www.quackit.com/html/html_table_generator.cfm)



## Validation
 

Validate your HTML at [Markup Validation Service](http://validator.w3.org/)

Or you can validate your CSS at [CSS Validation Service](http://jigsaw.w3.org/css-validator/). Usually your text-editor will tell your mistakes.

Don't blindly follow it! Validation is to find out your mistakes. You don't need to be 100% right. It is a tool not a rule.

You can use File-upload or Direct input tab.

 ## Resets and Normalizing
Popular Reset Style Sheet

[HTML5 Reset Stylesheet](http://html5doctor.com/html-5-reset-stylesheet/)

[Eric Meyer](http://meyerweb.com/eric/thoughts/2011/01/03/reset-revisited/)

[HTML5 Reset](http://html5reset.org/)

[HTML5 ★ BOILERPLATE](http://html5boilerplate.com/)

[2012’s most popular CSS Reset scripts, all in one place](http://www.cssreset.com/)

HTML
	
{% highlight html %}
.wrap>header>h1{My Wesite}+nav>ul>li*3>a[href=#]

<!-- after header -->
.main>p{The body of my webiste.}
{% endhighlight %}



Copy/paste a reset.css from [Eric Meyer](http://meyerweb.com/eric/thoughts/2011/01/03/reset-revisited/). 
Make a link to css/reset.css and css/mystyle.css. 

{% gist 5829107 %}

CSS

{% gist 5829073 %}

### normalize CSS
Check out the demo.

[normalize css Demo](http://necolas.github.io/normalize.css/latest/test.html)

[Download](http://necolas.github.io/normalize.css/)

[Layouts](http://layouts.ironmyers.com/)



## CSS Layouts
 

[Tutorial: Learn CSS Layout](http://learnlayout.com/)

[Resource: CSS layouts](http://www.maxdesign.com.au/articles/css-layouts/)



## JQuery slideshow


[jQuery Slideshow Plugins](http://vandelaydesign.com/blog/web-development/jquery-slideshow/)

- [Download Nivo Slider](http://dev7studios.com/nivo-slider/)

- Move downloaded file to Desktop.

- Create four place holder images with 600px x 300px. Move all images to nivo-slider/demo/images/

[Get the websites from Day 5](http://shinokada.github.io/blog/2013/06/21/day-5-make-your-own-website/)

-  Change img to your images. 

- Set .slider-wrapper to your image size. Nivo slider is set to flexbile by jQuery.

Example 
	
{% highlight css %}
{width: 700px;}
{% endhighlight %}


- Delete the link 

Example
	
{% highlight html %}
<a href="http://dev7studios.com" id="dev7link" title="Go to dev7studios">dev7studios</a>
{% endhighlight %}



- You can add the followings to data-transition

Examples 

	sliceDown
	sliceDownLeft
	sliceUp
	sliceUpLeft
	sliceUpDown
	sliceUpDownLeft
	fold
	fade
	random
	slideInRight
	slideInLeft
	boxRandom
	boxRain
	boxRainReverse
	boxRainGrow
	boxRainGrowReverse


- Adding captions

You can add captions in titles or add id. See the demo.



## Another SlideShow/Lightbox


[Coin Slider](http://workshop.rs/2010/04/coin-slider-image-slider-with-unique-effects/)

[30 Efficient jQuery Lightbox Plugins](http://www.designyourway.net/blog/resources/30-efficient-jquery-lightbox-plugins/)


### Extra practice
[More practice](http://sokada.site44.com/img/basic90.png)


![More practice](http://sokada.site44.com/img/basic90.png)




