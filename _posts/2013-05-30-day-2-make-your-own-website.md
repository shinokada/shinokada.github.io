---
layout: post
title: "Summer Course 2013: Day 2 Make Your Own Website"
date: 2013-06-18 19:38
categories: 2013 summer courses, HTML, CSS
---

Day 2 of Make YOur Own Website covering review, snippet, external style sheet and CSS properties.

## Review

Tools/sublime text, emmet, alfred, chrome, spectacle

html, head, body, h1 - h6, css, parent-child, ordered, unordered definition list, em, b, br, p, strong, external stylesheet, inline and internal stylesheet, directory structure


## Show or hide file extensions on a Mac?

Method 1

Finder > Preferences > Advanced and tick Show all filename extensions

Method 2

Use terminal

	defaults write com.apple.finder AppleShowAllFiles TRUE
	killall Finder
	
	

## Spectacle shortcuts
[&#8629; TOP](#markdown-toc)

Spectacle > Preferences

## Sublime text 2
[&#8629; TOP](#markdown-toc)

Skin

Preferences->color scheme



## Stackoverflow
[&#8629; TOP](#markdown-toc)

[stackoverflow](http://stackoverflow.com/) 

## html5 snippet
[&#8629; TOP](#markdown-toc)

### How to edit snippets
You can find your snippet in 

	Sublime Text 2 > Preferences > Browse Packages > User

	Or
	
	~/library/Appication Support/Sublime Text 2/Packages/User/
	

### Creating and saving snippets

new folder, html, html5.sublime-snippet. you need sublime-snippet at the end. Since you saved a snippet in a javascript folder, it will appear when you are using syntax html.

Copy/paste from [Gist](https://gist.github.com/shinokadagist/5794866)

{% gist 5794866 %}


## External Style Sheet
[&#8629; TOP](#markdown-toc)

Use dropbox about.html in the root folder.

### div

html
	
	<div>
	<h1>My website</h1>
	<p>Lorem ipsum doloe minima ipsa illo ipsum iste alias!</p>	
	<img src="img/builder.jpg" alt="">
	</div>

css 

	div {
		width:500px;
		background:red;
	}
	

#### div id and class

{% gist 5796223 %}


In your mystyle.css

{% gist 5796232 %}

    
#### Using li:nth-child()

{% gist 5796258 %}

	
#### Using first-of-type

{% gist 5796285 %}


    
## CSS properties
[&#8629; TOP](#markdown-toc)


- background-color
- width
- height



Example

    .container{
		background-color: white;
		width: 300px;
		height: 600px;
		border: 10px solid #ccc;
		text-align: center;
    }
    
    
- font-family

Example
        
	p.serif{font-family:"Times New Roman",Times,serif;}
	p.sansserif{font-family:Arial,Helvetica,sans-serif;}

	<h1>CSS font-family</h1>
	<p class="serif">This is a paragraph, shown in the Times New Roman font.</p>
	<p class="sansserif">This is a paragraph, shown in the Arial font.</p>


- padding: Padding will add to width and height. 

Example

	width: 300px;
	padding: 20px;
	// total width will be 340px

		
Or


	padding: 10px 5px 10px 15px;
	
Generally don't apply padding to a parent tag. Apply padding to a child tag. This will keep the container with the original width


	.container{
		background-color: white;
		width: 300px;
		height: 600px;
    }

    .container p{
    	padding: 20px;
    }

.container p will aim for p-tag in class container, but not other p-tag.

    <p>this is other p-tag outside container.</p>


- margin: Each browser has the default setting for some properties.

margin for p-tag

	.container p{
		padding: 10px;
		margin: 0;
    }
    
marging for .container

	.container {
	    margin: 10px 5px 10px 5px;
    }	
    
margin-right, margin-left, margin-top, margin-bottom

	.container {
	    margin-right: 100px;
    }
    
   	.container {
	    margin-left: 100px;
    }

