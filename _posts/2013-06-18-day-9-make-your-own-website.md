---
layout: post
title: "Summer Course 2013: Day 9 Make Your Own Website"
date: 2013-06-27 20:53
categories: 2013 summer courses, HTML, CSS
---



## Making Own Snippet


[Sample snippets](https://github.com/joshnh/CSS-Snippets)

Tools > New Snippet

- Add snippet
- Add tabTrigger
- Add scope or comment it out.

Example 1

    
{% highlight text %}
<snippet>
    <content><![CDATA[
Hello, ${1:this} is a ${2:snippet}.
]]></content>
    <!-- Optional: Set a tabTrigger to define how to trigger the snippet -->
    <tabTrigger>hello</tabTrigger>
    <!-- Optional: Set a scope to limit where the snippet will trigger -->
    <scope>source.css</scope>
</snippet>
{% endhighlight %}

Example 2

    
{% highlight text %}
<snippet>
    <content><![CDATA[
margin: ${1:0} ${2:0};
]]></content>
    <!-- Optional: Set a tabTrigger to define how to trigger the snippet -->
    <tabTrigger>m2</tabTrigger>
    <!-- Optional: Set a scope to limit where the snippet will trigger -->
    <scope>source.css</scope>
</snippet>
{% endhighlight %}


### Assignment 5
Create CSS snippet.

- [text-shadow 1](http://line25.com/articles/using-css-text-shadow-to-create-cool-text-effects)
- [text-shadow 2](http://designshack.net/articles/css/12-fun-css-text-shadows-you-can-copy-and-paste/)
- margin: 0 auto; etc
- clear: both; etc



## CSS3 Border-radius


HTML
    
{% highlight html %}
<div class="box">Hello World!</div>
{% endhighlight %}
  
  
CSS
    
{% highlight css %}
body{width: 200px; margin:100px auto 0;}
.box{
    background: red;
    width: 200px;
    height: 200px;
    border-radisu: 10px;/*20px, 100px, etc.*/
    line-height: 200px;/*to center vertically*/
    text-align: center;
} 
{% endhighlight %}


   
For webkit and firefox.
    
{% highlight css %}
-webkit-border-radius: 4px;
-moz-border-radius: 4px;
border-radius:4px;
{% endhighlight %}


## Prefixr


[prefixr](http://prefixr.com/)

Add the following to prefixr.com.

prefixr.css

    
{% highlight css %}
.box{
    background: red;
    width: 200px;
    height: 200px;
    line-height: 200px;
    text-align: center;
    border-radius: 5px;
    box-shadow:
        0 0 0 1px yellow,
        0 0 0 2px green,
        0 0 0 3px orange;
}
{% endhighlight %}


### Prefixr in Sublime Text 2

shift+cmd+p, type install and type prefixr in another dropdown.

In prefixr.css, move your cursor to where you want to change. Then press 'ctrl+super+x'. (You can check your shortcut through shift+cmd+p, Preferences: Prefixr Key Binding - Default)

Mine is like this. 

    
{% highlight text %}
[
    { "keys": ["ctrl+super+x"], "command": "prefixr" }
]
{% endhighlight %}


## CSS3 Box-Shadow 


Same markup as CSS3 Border-radius

[Demo](http://sokada.site44.com/gradient.html)

HTML
    
{% highlight html %}
<div class="box">Hello World!</div>
{% endhighlight %}
    
  
CSS

    
{% highlight css %}
body{width: 200px; margin:100px auto 0;}
.box{
    background: red;
    width: 200px;
    height: 200px;
    line-height: 200px;
    text-align: center;
    /* x-offset y-offset blur color*/
    box-shadow: 10px 10px 10px black;
    /*box-shadow: -10px -10px 50px black;*/
    /*box-shadow: 1px 1px 5px rgba(0,0,0,.5);*/
    /* rgba alpha*/
    /* x-offset y-offset blur spread color*/
    /*box-shadow: 1px 1px 5px 10px rgba(0,0,0,.5);*/
    /*stacking box-shadow*/
    box-shadow: 
        /*1px 1px 5px rgba(0,0,0,.5),
        2px 2px 5px green;*/
        /* with 0 spread*/
        /*1px 1px 0 yellow,
        2px 2px 0 green;*/
        /*box-shadow all around for border effect*/
        0 0 0 1px yellow,
        0 0 0 2px green, /* spread should be larger than one before*/
        0 0 0 3px orange;
}
{% endhighlight %} 



## CSS3 Gradients


[linear-gradient](http://www.css3files.com/gradient/)

[Demo](http://sokada.site44.com/gradient.html)
    
{% highlight html %}
<!-- (.box$+br)*6 (Thanks for Kaiki) -->

<div class="box1 box"><p>Box 1</p></div>
<br>
<div class="box2 box"><p>Box 2</p></div>
<br>
<div class="box3 box"><p>Box 3</p></div>
<br>
<div class="box4 box"><p>Box 4</p></div>
<br>
<div class="box5 box"><p>Box 5</p></div>  
<br>
<div class="box6 box"><p>Box 6</p></div>
{% endhighlight %}

gradient.css
    
{% highlight css %}
.box p{
    text-align: center;
    line-height: 500px;
    font-size: 90px;
    color: #fff;
    margin: 0;
    padding: 0;
}

.box1{
    margin: auto;
    width: 400px;
    height: 500px;
    background: black;
    background: linear-gradient(top, black, white);
    color: white;
}
{% endhighlight %}
    

background: linear-gradient(1 left, 2 red, 3 blue 4 30%, 5 green)

1. (optional) Starting point, can be angle 90deg, -45deg
2. Starting color
3. (optional) A color stop. Enhancement in the middle
4. (optional) Stop position
5. End color 

Check it in a browser. But it does not show gradients.

Hit a tab at the end of background: linear-gradient(top, black, white);. It will initiate the prefixr.

    
{% highlight css %}
.box2{
    margin: auto;
    width: 400px;
    height: 500px;
    background: black;
    background: -webkit-gradient(linear, 0 0, 0 100%, from(black), to(white));/* old webkit */
    background: -webkit-linear-gradient(black, white);
    background: -moz-linear-gradient(black, white);
    background: -o-linear-gradient(black, white);
    background: linear-gradient(black, white);/* place the official version at the end!*/
    color: white;
}
{% endhighlight %}
   

Then check it in Chrome and Firefox.

Try a color stop.

    
{% highlight css %}
background: linear-gradient(top, black, red, green);
{% endhighlight %}

Creating an edge with linear-gradient. 50% means #e3e3e3 will be upto 50% and transition starts from 50%.
    
{% highlight css %}
body{background: #666;}
.box3{
    margin: auto;
    width: 400px;
    height: 500px;
    background: linear-gradient(top, #e3e3e3 50%, white 50%);
}
{% endhighlight %}

    
Left to right 
    
{% highlight css %}
.box4{
    margin: auto;
    width: 400px;
    height: 500px;
    background: linear-gradient(left, red 50%, green);
}


.box5{
    margin: auto;
    width: 400px;
    height: 500px;
    background: linear-gradient(left, red 50%, #e70303 50%, #22db22 51%, #12c512 52%);
}


.box6 {
    border: 5px solid #fff;
    margin: auto;
    width: 400px;
    height: 500px;
    /*Official Syntax*/
    background-image: linear-gradient(top, #ff5a00, #FFAE00);
}
{% endhighlight %}

Adding gradient to body tag.

    
{% highlight css %}
body{
    background: linear-gradient(top, #e3e3e3, white); /* hit tab for prefixr */
}
{% endhighlight %}


But only one box, this will create a problem. Try it.

How to fix it. Add min-height: 100% to body, html
    
{% highlight css %}
body, html{
    min-height: 100%;
}
{% endhighlight %}

{% gist 5856737 %}

Try the followings. [CSS3 Linear Gradients](http://demo.hongkiat.com/css3-linear-gradient/index.html)



### Resource

[Speed Up with CSS3 Gradients](http://css-tricks.com/css3-gradients/)


### Gradient generator

[Ultimate CSS Gradient Generator](http://www.colorzilla.com/gradient-editor/)



## Image Caption With CSS3


[Demo](http://sokada.site44.com/imagecaption.html)

[6 Cool Image Captions With CSS3](http://www.hongkiat.com/blog/css3-image-captions/)
Create a new dir with the name of "imagecap" in your desktop. Within this dir, create images dir, index.html and style.css. 

Create 6 images, (1.jpg, 2.jpg,.. 6.jpg) with 200px x 200px and put it in images dir.

### HTML

{% gist 5857970 %}

### CSS

Eric Meyer's reset.css, then go through from [Basic CSS](http://www.hongkiat.com/blog/css3-image-captions/) 

{% gist 5858023 %}

