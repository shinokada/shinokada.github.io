---
layout: post
title: "Summer Course 2013: Day 4 Make Your Own Website"
date: 2013-06-20 14:33
categories: 2013 summer courses, HTML, CSS
---

## Individual work


### Assignment 1

![assignemt](http://sokada.site44.com/img/assignment.png)

[Assignment image](http://sokada.site44.com/img/assignment.png)



### Solutions
Check HTML/CSS with Chrome's Developer tool. alt+cmd+i

[Assignment 1](http://sokada.site44.com/assignment.html)



## Float 


Emmet shorthand

    .wrap>h1{My Second Website}+.content>h2{My Content Heading}+p{This is my blog.}^aside>h2{Sidebar}+ul>li*3>a[href="#"]{Home}
    // and hit tab

{% gist 5802981 %}

{% gist 5803276 %}

#### Problem 

Since you have floated all, the wrap div is separated.

#### Solution 1: Add footer to html and add css
  
    
	// html
	<div class="wrap">
		<h1>My Second Webiste</h1>
		<div class="content">
			<h2>My Content Heading</h2>
			<p>This is my blog.</p>
		</div>
    
		<aside>
			<h2>Sidebar</h2>
			<ul>
				<li><a href="#">Home</a></li>
				<li><a href="#">About</a></li>
				<li><a href="#">Contact</a></li>
			</ul>
		</aside>
    
		<footer>
    
		<h2>My footer</h2>
		</footer>   
	</div>     
	
	// css
	footer{
	    clear: both;
    }
	
#### Solution 2: Add overflow: hidden to .warp

    .wrap{
		min-width: 600px;
		width: 80%;
		margin: auto;
		background: green;
		overflow: hidden; /* add this to clear float*/
    }
	



## Navigation List 


    ul>li*4>a[href="#"]{Home}
    
This will produce the following.
    
menu.html

    <ul>
    	<li><a href="#">Home</a></li>
    	<li><a href="#">Home</a></li>
    	<li><a href="#">Home</a></li>
    	<li><a href="#">Home</a></li>
    </ul>
    
    
Change to     
    
    <ul>
    	<li><a href="#">Home</a></li>
    	<li><a href="#">About</a></li>
    	<li><a href="#">Portfolio</a></li>
    	<li><a href="#">Contact</a></li>
    </ul>
        
CSS Example 1

menu.css

	li{
		display: inline;
	}
	
	// check the browser
	
	ul a{
		text-decoration: none;
	}
	
	// check the browser
	
	
	body {
		font-family: sans-serif;
	}
	
	// check the browser
	
	li{
		display: inline;
		padding-right: 10px;
	}
	
	// check the browser
	// adding a pseudo class
	ul a:hover{
		text-decoration: underline;
		// other example
		font-size: 30px;
		// or change the color
		color: blue;
	}
	


