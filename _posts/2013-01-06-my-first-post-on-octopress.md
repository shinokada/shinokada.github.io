---
layout: post
title: "Personal note to create and deploy githubpages"
date: 2013-01-06 20:35
comments: true
categories: [Octopress, github, github pages]
---

<!-- more -->

{:.no_toc}

* Will be replaced with the ToC, excluding the "Contents" header
{:toc}

My note to create and deploy next posts on github pages. Useful [website](http://learnaholic.me/2012/10/10/deploying-octopress-to-github-pages-and-setting-custom-subdomain-cname/) for octopress and github pages.

After installation, to create a new post.

	
~~~ text
rake new_post['My another post title here']
	
# Then open it and adding categories and enter an article. Once you are ready, then

rake generate   # Generates posts and pages into the public directory
rake watch      # Watches source/ and sass/ for changes and regenerates
rake preview    # Watches, and mounts a webserver at http://localhost:4000
~~~


To see a preview locally,

	
~~~ text
rake preview
//Go to http://localhost:4000/
	
# Then deploy your post by,

rake generate
rake deploy
~~~


And check your githubpage.

## Commit and push your sorce
[&#8629; TOP](#markdown-toc)
	
~~~ text
git add .
git commit -m 'your message'
git push origin source
~~~


[Blogging Basics on Octopress](http://octopress.org/docs/blogging/)



## How to install Octopress theme
[&#8629; TOP](#markdown-toc)
	
~~~ text
$ git clone git://github.com/lucaslew/whitespace.git .themes/whitespace
$ rake install['whitespace']
$ rake generate
$ rake update_source            # update the template's source
$ rake update_style 
$ rake deploy
$ git add .
$ git commit -m 'your message'
$ git push origin source
~~~

And some tweaks.

	
~~~ css
/* sass/partials/diebar/_base.scss */
@media only screen and (min-width: 768px) {
  .toggle-sidebar {
    outline: none;
    position: absolute; right: -10px; top: 0; bottom: 0;
    /*display: inline-block;*/
    ...}
    
~~~

sass/custom/_styles.scss

	
~~~ text 
// This File is imported last, and will override other styles in the cascade
// Add styles here to make changes without digging in too much

$white: #FFFFFF;

html {
	background: $white;
}

body {
	font-family: $sans !important;
	font-size: 1em;
	max-width: 850px;
	padding-left: 0.5em;
	padding-right: 0.5em;

	> header {
		background: $white;
		text-align: center;
		padding-left: 0px;
		padding-right: 0px;	

		h1 {
			a, a:visited, a:hover {
				color: #8C8C8C;
				font-family: $heading-font-family;
			}
		}
	}

	> nav {
		background: $white;
		border-bottom: 1px solid #F2F2F2;
		padding-left: 0px;
		padding-right: 0px;

		form .search {
			border-radius: 0.2em 0.2em 0.2em 0.2em;
			box-shadow: none;
			border: 0px;
			padding-top: 0.3em;
			padding-bottom: 0.3em;
			padding-left: 0.5em;
			padding-right: 0.5em;
		}

		a {
			font-family: $sans;
			font-size: 0.9em;
			padding-top: 0.3em;
			line-height: 1.5em;
		}

		li + li {
			border-left: 0px;

			a {
				border-left: 0px;
			}
		}
	}

	> div {
		background: $white;
		border-bottom: 0px;

		> div {
			background: $white;
			border-right-width: 0px;
		}
	}

	> footer {
		background: $white;
		text-shadow: none;
		color: #AAAAAA;
	  	padding-left: 0;
	  	padding-right: 0;
		border-top: 0px;
		padding-left: 0px;
		padding-right: 0px;
	}

	#content {
		> article {
			padding-left: 0px;
			padding-right: 0px;
		}

		> div {
			> article, > section {
				padding-left: 0px;
				padding-right: 0px;
			}	
		}
	}
}

article {
	padding-top: 2em;

	.entry-content {
		h3 {
			font-style: italic
		}
	}

	ul, ol {
		margin-left: 2em;
	}

	a, a:visited {
		color: #1863A1;
	}

	header {
		h1.entry-title {
			font-family: $heading-font-family;
			font-weight: 400;
		}
	}
}

#content {
	div.pagination {
		background: none repeat scroll 0 0 transparent;
		margin: 0 10px;  
		font-size: 0.95em;
		padding-bottom: 1.5em;
		margin-top: 4em;
		position: relative;
		text-align: center;		
		border-top: dotted 1px #D1D1D1;
		border-bottom: dotted 1px #D1D1D1;
	}

	.blog-index {
		article {
			padding-top: 4em;

			header {
				padding-left: 0;
  				padding-right: 0;
			}
		}

		h1 {
			a {
				font-family: $heading-font-family;
				font-weight: 400;
			}

			a:hover {
				text-decoration: none;
			}
		}
	}

	.hentry {
		h1.entry-title {
			font-family: $heading-font-family;
			font-weight: 400;
			font-size: 2.2em;
		}
	}
	
}

figure.code {
	.highlight {
		background: #212C3B !important;

		.gutter {
			display: none;
		}
	}
}

.pre-code, html .gist .gist-file .gist-syntax .highlight pre, .highlight code {
	background: #212C3B !important;
}

aside {
	display: none;
}

#markdown-toc:before {
  content: "Table of Contents";
  font-weight: bold;
}
ul#markdown-toc {
  list-style: none;
  float: right;
  @include shadow-box;
  background-color: white;
  margin: 10px 0 20px 10px;
}

.gist, .bogus-wrapper{
	clear: both;
	margin-top:10px;
}

#content {
	margin-right: 0;
}

/*
http://blog.codebykat.com/2013/05/23/gorgeous-octopress-codeblocks-with-coderay/
 */
/* overrides of the lousy styles from _syntax.scss */
.CodeRay {
	clear: both;
}
.CodeRay pre {
  background: none;
  color: #000;
}

/* fixes issue where the whole line wouldn't get colored in a diff */
.CodeRay span.insert, .CodeRay span.change, .CodeRay span.delete {
        width: auto;
}

.toggle-sidebar{
  display: none;
}

article h2:first-of-type{background-image:none;}

@import "coderay-github"
~~~


## Using CodeRay
[&#8629; TOP](#markdown-toc)

[Codeblocks with CodeRay](http://blog.codebykat.com/2013/05/23/gorgeous-octopress-codeblocks-with-coderay/)
After installing CodeRay use the folloings.

## Some examples
[&#8629; TOP](#markdown-toc)

{% codeblock coderay lang:ruby %}
~~~
def what?
  42
end
~~~
{:.language-ruby}
{% endcodeblock %}

The above code will produce the following.

~~~
def what?
  42
end
~~~
{:.language-ruby}

Another way


{% codeblock coderay lang:ruby %}
~~~ ruby
def what?
  42
end
~~~
{% endcodeblock %}

will produce,

~~~ ruby
def what?
  42
end
~~~



### Inline code
[&#8629; TOP](#markdown-toc)

{% codeblock coderay lang:ruby %}
`fox jumps over`

A quick brown `fox jumps over` lazy dogs.
{% endcodeblock %}

will produce,

`fox jumps over`

A quick brown `fox jumps over` lazy dogs.

### Block code
[&#8629; TOP](#markdown-toc)

{% codeblock coderay lang:ruby %}

~~~
foo foo
bar bar
baz baz
~~~
{% endcodeblock %}

will produce,

~~~
foo foo
bar bar
baz baz
~~~

