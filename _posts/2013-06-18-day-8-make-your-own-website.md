---
layout: post
title: "Summer Course 2013: Day 8 Make Your Own Website"
date: 2013-06-26 20:53
categories: 2013 summer courses, HTML, CSS
---

## HTML5 Tag list

[HTML5 element list](https://developer.mozilla.org/en-US/docs/Web/Guide/HTML/HTML5/HTML5_element_list)



## Sublime Text 2 directory/file creation

Install AdvancedNewFile.

See details Sublime>Preferences>Browse Packages>AdvancedNewFile>Readme.md drop to sublime to read
    
{% highlight text %}
cmd+option(alt)+n
{% endhighlight %}

	

Then type directory/filename.
    
{% highlight text %}
ctl+opt+enter
{% endhighlight %}


This will display a space at the bottom to enter your abbreviation and real code in the main panel.

### Sublime Text 2 Emmet abbriviation list
[Emmet Cheat sheet](http://docs.emmet.io/cheat-sheet/)

[Emmet CSS Snippets for Sublime Text 2](http://peters-playground.com/Emmet-Css-Snippets-for-Sublime-Text-2/)


## Solution Assignment from Day  7

[Solution for More Pracitce in Day 7](http://sokada.site44.com/demo/basic-90/)


## CSS Frameworks

[960 Grid system Demo](http://960.gs/demo.html)

Download from 960 Grid system. Copy reset.css, 960_12_col.css, text.css

    
{% highlight html %}
.wrap>header>h1{My Wesite}+nav>ul>li*3>a[href=#]
    
<!-- after header -->
.main>.primary>p{My content goes here.}^aside

<!-- Within aside -->
ul>li*6>a[href=#]{Item}
{% endhighlight %}

{% gist 5829383 %}

960_12_col.css 

{% highlight css %}
.container_12 {
  margin-left: auto;
  margin-right: auto;
  width: 960px;
}
{% endhighlight %}


Add .container_12 to .wrap. You can add multiple class but not to id.
    
{% highlight html %}
<div class="wrap container_12">
{% endhighlight %}

	

Overwrite 960grid in css/style.css
    
{% highlight css %}
.container_12{
    background: red;
}
{% endhighlight %}


Add .grid_4 to header>h1 and .grid_8 to header>nav. All have to add up to 12 in 960_12_col.css. Also notice that all .grid is floated. So it will collapse.

Edit style.css
    
{% highlight css %}
nav li{display: inline;}
{% endhighlight %}


Add .grid_8 to .primary and .grid_4 to aside. And add bg to see clearly.

    
{% highlight css %}
aside{background: green;}
.primary{background: #e3e3e3;}
header{
    background: yellow;
}
{% endhighlight %}



Check it in your browser. aside class="grid_4" should be on the right. But it is not. Because it is not clearing. 

To solve it, you can add clear=both to .primary but it is partially fix the problem. It does not show yellow bg color. Add overflow:hidden; to header{} or add grid960's class="clearfix" to header.

    
{% highlight css %}
/*css*/
header{
    background: yellow;
    overflow:hidden;    
}
{% endhighlight %}

Or HTML
    
{% highlight html %}
<header class="clearfix">
{% endhighlight %}


960 will add clearfix to after that class.

    
{% highlight css %}
/* http://www.yuiblog.com/blog/2010/09/27/clearfix-reloaded-overflowhidden-demystified */
.clearfix:before,
.clearfix:after,
.container_12:before,
.container_12:after {
  content: '.';
  display: block;
  overflow: hidden;
  visibility: hidden;
  font-size: 0;
  line-height: 0;
  width: 0;
  height: 0;
}
{% endhighlight %}


More CSS
    
{% highlight css %}
.main{margin: 30px 0;}
{% endhighlight %}

	

But again you need to add .clearfix to .main since all the divs under .main are floated.
    
{% highlight html %}
<div class="main clearfix">
...
{% endhighlight %}

	

In order to align top menu and right column menu, change h1 class to grid_8 and nav to grid_4.
    
{% highlight html %}
<h1 class="grid_8">My Wesite</h1>
<nav class="grid_4"> 
{% endhighlight %}

Add a color to nav.
    
{% highlight css %}
/*css*/
nav{
    background: orange;
}
{% endhighlight %}


Lines are aligned but not list items due to margin-left.
    
{% highlight css %}
nav li{display: inline; margin-left:0; margin-right:30px;}
{% endhighlight %}


#### Adding contents

We add four columns. Remove 
    
{% highlight html %}
<p>My content goes here.</p>
{% endhighlight %}

    

And type (section.grid_2>p>lorem20)*4.
    
{% highlight html %}
<div class="main clearfix">
<div class="primary grid_8">
    (section.grid_2>p>lorem20)*4      
    <p>My content goes here.</p>
</div>
{% endhighlight %}


This will produce the followings.

    
{% highlight html %}
<section class="grid_2">
    <p>Lorem ipsum dolor sit amet, consectetur adipisicing elit. Accusantium repellendus quas dolor quo pariatur explicabo obcaecati fugit voluptatem rem vel.</p>
</section>
<section class="grid_2">
    <p>Ea veritatis mollitia dolorum explicabo atque in hic doloribus ipsa fuga asperiores. Architecto deleniti nihil dignissimos sed ducimus dolores error.</p>
</section>
<section class="grid_2">
    <p>Quibusdam nisi saepe vitae illum laboriosam et eaque adipisci nesciunt iure voluptates reiciendis veritatis dolores nemo facilis ullam dolorem voluptas?</p>
</section>
<section class="grid_2">
    <p>Sapiente placeat quibusdam cumque enim autem magni rerum culpa earum voluptate quo. Sint voluptatibus reiciendis deserunt illum architecto nemo accusamus.</p>
</section>
{% endhighlight %}


Check it on your browser. It creates only three columns because of margin-left on the first column and margin-right on the last column. Take it out on your web deveoper tools(element style).

Grid960 provides alpha and omega class for this situation.
    
{% highlight css %}
.alpha {
  margin-left: 0;
}

.omega {
  margin-right: 0;
}
{% endhighlight %}


Add alpha to the first column and omega to the last column.
    
{% highlight html %}
<!-- first column -->
<section class="grid_2 alpha">
    ...
<!-- last column -->
<section class="grid_2 omega">
    ...
{% endhighlight %}



Copy all the sections and paste them. Since all are foated to left, it align nicely.

Now let's add headings. Select p-tag 
    
{% highlight html %}
<p>
{% endhighlight %}


and hit ctl+cmd+g to select all of p-tag. then add h4 above p-tag.
    
{% highlight html %}
//h4 tab  
<h4>My heading</h4>
{% endhighlight %}


#### Box model

Adding padding will break the code.

[Box model](https://developer.mozilla.org/en-US/docs/Web/CSS/box_model?redirectlocale=en-US&redirectslug=CSS%2Fbox_model)

Total width, Total height = width + margin + padding + border

![box model](http://sokada.site44.com/img/boxmodel.png)

IE vs FF,Chrome

Solution 1: Adding div under each section.
    
{% highlight html %}
<section class="grid_2 alpha">
    <div>
        <p>Lorem ipsum dolor sit ame rem vel.</p>
    </div>
</section>
<section class="grid_2 alpha">
    <div>
        <p>Lorem ipsum dolor sit ame rem vel.</p>
    </div>
</section>
<section class="grid_2 alpha">
    <div>
        <p>Lorem ipsum dolor sit ame rem vel.</p>
    </div>
</section>
<section class="grid_2 omega">
    <div>
        <p>Lorem ipsum dolor sit ame rem vel.</p>
    </div>
</section>
{% endhighlight %}


And add padding to section div.
    
{% highlight css %}
section div{
    padding: 15px;
}
{% endhighlight %}


Solution 2: Using a new technology [box-sizing](https://developer.mozilla.org/en-US/docs/Web/CSS/box-sizing) for adding paddings inside width.
    
{% highlight html %}
section {
    box-sizing: border-box;
    padding:15px;
}
{% endhighlight %}


### Other CSS Frameworks

[Twitter Bootstrap](http://twitter.github.io/bootstrap/index.html)

[Blueprint](http://blueprintcss.org/)

[20 Exceptional CSS Boilerplates and Frameworks](http://mashable.com/2013/04/26/css-boilerplates-frameworks/)



## How to Slice a PSD

Open Photoshop and tick Auto-Select: Layer. When you click an image, it will select a layer in LAYERS panel. alt+cmd and click the image in the LAYERS panel. (cmd+d to deselect) Copy(cmd+c) it and paste it in a new file.　After pasting untick the background. File > Save as Web and Devices and use JPEG for photos and PNG-24 for transparent images.

### Ruler in Photoshop
Eyedropper > Ruler.

![Auto select and layer](http://sokada.site44.com/img/photoshop1.png)



## Asignment 4 PSD to HTML/CSS

[Download Simple Website Template PSD](http://www.92pixels.com/psd-of-the-day-simple-website-template/)

### Extra practice
PSD2HTML

[Simple Design](http://sundox.deviantart.com/art/Template-Light-Blog-147294562)

