---
layout: post
title: "Summer Course 2013: Day 1 Make Your Own Website"
date: 2013-06-17 22:19
comments: true
categories: [2013 summer courses, HTML, CSS]
---

<!-- more -->

{:.no_toc}

* Will be replaced with the ToC, excluding the "Contents" header
{:toc}

## Tools
[&#8629; TOP](#markdown-toc)

### Text editer
Download and install sublime text 2 installation

[sublime text 2](http://www.sublimetext.com/2)



### Installing sublime text 2 package controller

[How to install Package Control](http://wbond.net/sublime_packages/package_control/installation)

### Installing Emmet

cmd+shift+p type install

Type emmet to install it.


### Alfred
[Link](http://www.alfredapp.com/)

### Chrome developer tools
short-cut opt+comm+i

Explore dev tools

### [site44](http://www.site44.com/)

[my website at site 44](http://shinokada.site44.com/)



## Your first web page
[&#8629; TOP](#markdown-toc)

### [day1/index.html](index.html)
Use dropbox pages

~~~ html
{% gist 5796266 %}
~~~

##### [lorem ipsum generator](https://chrome.google.com/webstore/detail/lorem-ipsum-generator/dmpfoncmmihgkooacnplecaopcefceam/related?hl=en)

* title, doctype, html, head, body, charset etc
* h1, h2,h3,h4,h5,h6
* p
* em
* b
* strong
* br 
* Brower default padding for h1, p etc.


	

### Lists
#### Unordered Lists

* indentation, child and parent
* Browser will make up your bad coding. It depends on browsers.


Example

	<ul>
		<li>
		<h1>Coffee</h1>
		<p>I don't like it</p>
		</li>
		<li>Milk</li>
	</ul>

#### Ordered Lists

	<ol>
		<li>Coffee</li>
		<li>Milk</li>
	</ol>

#### Definition Lists

	<dl>
		<dt>Coffee</dt>
		<dd>- black hot drink</dd>
		<dt>Milk</dt>
		<dd>- white cold drink</dd>
	</dl>


### Parent and child relation
Siblings, ancester, decendent, grand child etc

html->body->hi->span

	<h1>
		Things I must do.
		<span>Today: bla</span>
	</h1>
	

# CSS and style sheet

## Playing around with Chrome Developer Tool
[&#8629; TOP](#markdown-toc)

- Delete all css
- Add some css
- Edit HTML


## Inline Style Sheet
[&#8629; TOP](#markdown-toc)

Use dropbox about.html


	<p><a href="about.html" style="color: green;">Learn more about me.</a></p>
	
	<p style="color:sienna;margin-left:20px">This is a paragraph.</p>
	
## Internal Style Sheet	
[&#8629; TOP](#markdown-toc)

Use dropbox about.html


	<!doctype html>
    <head>
       <meta charset="utf-8">
       <meta charset=utf-8>
       <title>My First Website</title>
    <style>
    hr {color:sienna;}
    p {margin-left:20px;}
    body {background-image:url("images/back40.gif");}
    </style>
    </head>
    <body>
        <div id="content">
        	<p>My content.</p>
        </div>
        <div id="footer">
        	<p>My footer.</p>
        </div>
    </body>


## External Style Sheet
[&#8629; TOP](#markdown-toc)


    //For the same directory
    <link rel="stylesheet" href="mystyle.css">

    // For one level up
	<link rel="stylesheet" href="../mystyle.css">
    
    // in other directory
    <link rel="stylesheet" href="../other/mystyle.css">







