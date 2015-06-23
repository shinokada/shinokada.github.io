---
layout: post
title: "Summer Course 2013: Day 10 Make Your Own Website"
date: 2013-06-28 20:53
categories: 2013 summer courses, HTML, CSS
---

## Feedback


Moodle

## Code Editor


### PHPStorm

[PhpStorm](http://www.jetbrains.com/phpstorm/)

I will give you an academic license code in class.

### Brackets

[Brakets](http://brackets.io/)

### Codenvy

[Codenvy](https://codenvy.com/)

## CSS3 Columns


HTML

	
{% highlight html %}
<!-- p.threecol>lorem100 -->
<p class="threecol">Pellentesque habitant morbi tristique senectus et netus et malesuada...</p>
{% endhighlight %}



CSS

	
{% highlight css %}
/* ctl+cmd+x 
column-count: 3;
column-gap:20px;
 */
p.threecol {
    -webkit-column-count: 3;
	-moz-column-count: 3;
	column-count: 3;
	-webkit-column-gap:20px;
	-moz-column-gap:20px;
	column-gap:20px;		
}
{% endhighlight %}


## CSS3 Transitions


[Demo](http://sokada.site44.com/transition.html)

HTML

{% highlight html %}
{% gist 5865758 %}
{% endhighlight %}

CSS
After typing the following, ctl+cmd+x to use prefixr.
	
{% highlight css %}
#timings_demo {
	position:relative;
	width:530px;
	height:530px;
	margin:0 auto 10px;
	border:1px #aaa solid;
	padding:10px;
}
.test_box {
	font-size:12px;
	position:relative;
	width:60px;
	height:60px;
	margin-bottom:10px;
	background-color:#eee;
}
.test_box p {
	text-align:center;
	padding-top:4px;
}
#ease.test_box {
	transition: all 4s ease;    
	border:1px #f00 solid;
}
#ease-in.test_box {
	transition: all 4s ease-in;    
	border:1px #0f0 solid;
}
#ease-out.test_box {
	transition: all 4s ease-out;    
	border:1px #00f solid;
}
#ease-in-out.test_box {
	transition: all 4s ease-in-out;    
	border:1px #ff0 solid;
}
#linear.test_box {
	transition: all 4s linear;    
	border:1px #f0f solid;
}
#custom.test_box {
	transition: all 4s cubic-bezier(1.000, 0.835, 0.000, 0.945);    
	border:1px #0ff solid;
}
#negative.test_box {
	transition: all 4s cubic-bezier(1.000, -0.530, 0.405, 1.425);    
	border:1px #000 dotted;
}
#timings_demo:hover .test_box, #timings_demo.hover_effect .test_box {
	border-radius:30px;
	transform: rotate(720deg);    
	margin-left:420px;
	background-color:#fff;
}
{% endhighlight %}



Final CSS

{% highlight css %}
{% gist 5865762 %}
{% endhighlight %}

[More demo](http://css3.bradshawenterprises.com/transitions/)



## CSS3 Transform 


[Demo 1](http://sokada.site44.com/transform.html)
Images

Prepare four of 450 x 281px.

HTML

{% highlight html %}
{% gist 5865884 %}
{% endhighlight %}

CSS
	
{% highlight css %}
#controls, #transparency {
	text-align:center;
}
#controls span {
	padding-right:2em;
	cursor:pointer;
}

#cubeCarousel {
	perspective: 800;
	perspective-origin: 50% 100px;
	margin:100px auto 20px auto;
	width:450px;
	height:400px;
}

#cubeCarousel #cubeSpinner {
	position: relative;
	margin: 0 auto;
	height: 281px;
	width: 450px;
	transform-style: preserve-3d;
	transform-origin: 50% 100px 0;
	transition:all 1.0s ease-in-out;
}

#cubeCarousel #Ycube,#cubeCarousel #Zcube {
	transform-style: preserve-3d;
}

#cubeCarousel .face {
	position: absolute;
	height: 281px;
	width: 450px;
	padding: 0px;
}

#cubeSpinner .one {
	transform: translateZ(225px);
}

#cubeSpinner .two {
	transform: rotateY(90deg) translateZ(225px);
}

#cubeSpinner .three {
	transform: rotateY(180deg) translateZ(225px);
}

#cubeSpinner .four {
	transform: rotateY(-90deg) translateZ(225px);
}
.trans {
	opacity:0.7;
}
{% endhighlight %}

	  

Then use prefixr to get the following.

{% highlight css %}
{% gist 5865886 %}
{% endhighlight %}

[Demo 2](http://sokada.site44.com/transform2.html)

Images

Prepare two of 320 x 420px.

HTML

{% highlight html %}
{% gist 5874627 %}
{% endhighlight %}

CSS
	
{% highlight css %}
/* simple */
.flip-container {
	/* prefixr not working for this */
	-webkit-perspective: 1000;
	-moz-perspective: 1000;
	perspective: 1000;
	border: 1px solid #ccc;
}

	
.flip-container:hover .flipper, .flip-container.hover .flipper, #flip-toggle.flip .flipper {
	transform: rotateY(180deg);
}

.flip-container, .front, .back {
	width: 320px;
	height: 420px;
}

.flipper {
	transition: 0.6s;
	transform-style: preserve-3d;
	position: relative;
}

.front, .back {
	/* prefixr not working for this */
	-webkit-backface-visibility: hidden;
	-moz-backface-visibility: hidden;
	backface-visibility: hidden;
	position: absolute;
	top: 0;
	left: 0;
}

.front {
	background: lightgreen;
	z-index: 2;
}

.back {
	background: lightblue;
	transform: rotateY(180deg);
}

/* vertical */
.vertical.flip-container {
	position: relative;
}

.vertical .back {
	transform: rotateX(180deg);
}

.vertical.flip-container .flipper {
	transform-origin: 100% 213.5px;
}

.vertical.flip-container:hover .flipper {
	transform: rotateX(-180deg);
}
{% endhighlight %}


Watch out that there are two properties does not work with Prefixr. You have to write the code. 
Then use prefixr to get the following.

{% gist 5874629 %}



## jQuery Accordion


Download source files from [Theming JQuery UI Accordion](http://www.hongkiat.com/blog/theming-jquery-ui-accordion/)



## JqueryMobile


[jQueryMobile](http://jquerymobile.com/)

- Copy-and-Paste Snippet for CDN-hosted files (recommended):

Example

	
{% highlight html %}
<link rel="stylesheet" href="http://code.jquery.com/mobile/1.3.1/jquery.mobile-1.3.1.min.css" />
<script src="http://code.jquery.com/jquery-1.9.1.min.js"></script>
<script src="http://code.jquery.com/mobile/1.3.1/jquery.mobile-1.3.1.min.js"></script>
{% endhighlight %}


- See [Demo](http://view.jquerymobile.com/1.3.1/dist/demos/)
Copy and paste some codes. Accordion, Buttons, etc.



## Markdown


[Markdown doc](http://daringfireball.net/projects/markdown/basics)

[Mou App](http://mouapp.com/)



## [Different Stylesheets for Differently 


Sized Browser Windows](http://css-tricks.com/resolution-specific-stylesheets/)

[Demo 1](http://css-tricks.com/examples/ResolutionDependantLayout/example-one.php)




## Jquery Menu


[25 Free jQuery Menu and Tutorials](http://www.codefear.com/scripts/25-free-jquery-menu-navigation/)




## Quiz


[CSS Quiz](http://net.tutsplus.com/articles/quizzes/nettuts-quiz-1-beginner-css/)



## Resources


### Learn HTML/CSS
[Free online course: 30 Days to Learn HTML and CSS](https://tutsplus.com/course/30-days-to-learn-html-and-css/)

### HTML5/CSS3
[Demo](http://demo.hongkiat.com/)

### HTML5
[html5rocks](http://www.html5rocks.com/en/tutorials/)

[MDN HTML5](https://developer.mozilla.org/en-US/docs/Web/Guide/HTML/HTML5)

[caniuse](http://caniuse.com/)

### CSS
[CSS-Tricks](http://css-tricks.com/)

[MDN Getting started with CSS](https://developer.mozilla.org/en-US/docs/Web/Guide/CSS/Getting_started?redirectlocale=en-US&redirectslug=CSS%2FGetting_Started)

[CSS Reference](https://developer.mozilla.org/en-US/docs/Web/CSS/Reference?redirectlocale=en-US&redirectslug=CSS%2FCSS_Reference)

### CSS3
[css3please](http://css3please.com/)

[css3test](http://css3test.com/)

[MDN css3](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS3)

### CSS3 Transitions, Transforms, Animation, Filters

[CSS3 Transitions, Transforms, Animation, Filters](http://css3.bradshawenterprises.com/)

### Learn Sublime Text 2
[Free online course: Perfect Workflow in Sublime Text 2](https://tutsplus.com/course/improve-workflow-in-sublime-text-2/)

### Learn jQuery
[Free online course: 30 Days to Learn jQuery](https://tutsplus.com/course/30-days-to-learn-jquery/)

### Tutorials
[Codrops](http://tympanus.net/codrops/)

[tutorialzine](http://tutorialzine.com/)

### Web Development
[nettuts+](http://net.tutsplus.com/)

### Web Design
[Smashingmagazine](http://www.smashingmagazine.com/)

### Community
[Stackoverflow](http://stackoverflow.com/) 

[Github](https://github.com/)

### CSS compressor/minifier

[CSS Compressor](http://www.csscompressor.com/)

[CSS Compressor & Minifier](http://www.minifycss.com/css-compressor/)

[CSS Drive](http://www.cssdrive.com/index.php/main/csscompressor/)

[Online YUI Compressor](http://refresh-sf.com/yui/)


### Templates
[Free HTML5 Template](http://www.os-templates.com/free-basic-html5-templates)

### Blog
You need [Feedly](http://cloud.feedly.com/#welcome) to follow blogs.

[100 Best Web Design Blogs](http://designm.ag/designer-showcase/top-100-highest-quality-web-design-blogs/)

### Parallax Effect
[15 Best Parallax Scrolling Tutorials](http://inspiretrends.com/parallax-scrolling-tutorials/)



## Extending CSS with LESSjs
[CSS preprocessors](http://www.vanseodesign.com/css/css-preprocessors/)

## Others

[Google Custom Search](http://www.google.com/cse/)




