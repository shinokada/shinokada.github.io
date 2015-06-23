---
layout: post
title: "Sublime Text 2 Enhancements"
date: 2013-06-13 21:11
comments: true
categories: [2013 summer courses]
---

<!-- more -->

{:.no_toc}

* Will be replaced with the ToC, excluding the "Contents" header
{:toc}

##  Use subl in the terminal
[&#8629; TOP](#markdown-toc)

Add the following 

	sudo ln -s "/Applications/Sublime Text 2.app/Contents/SharedSupport/bin/subl" /bin/subl
	
	OR 
	
	ln -s /Applications/Sublime\ Text\ 2.app/Contents/SharedSupport/bin/subl /usr/local/bin/subl

Then go to the dir and type subl . . Don’t forget the the full stop at the end to open all.


###  Mac 
Open Automator and select ‘service’.

find shell script from Search and drag it to the right stage.

/bin/zsh should be selected from the dropdown. And type this.

	/Applications/Sublime\  Text\  2.app/Contents/ShareddSupport/bin/subl -n $@

Files or folders should be selected under the The service receives selected.

Save it (command + s) as Open in sublime text 2.

Then you can right click on a file or folder and select this service to open it in sublime text.


## Multiple Cursors and Incremental Search
[&#8629; TOP](#markdown-toc)

### Selecting words,

Select all: cntl+cmd+G

column view : press option and drag the cursor.

Select lines: cmd+L(cntl+cmd+L) to edit

Incrementalfind, cmd+i

## The command palette
[&#8629; TOP](#markdown-toc)

shift+cmd+p
syntax javasc (it is fuzzy searching) or just javascript then enter

### Sidebar 

	cmd+k, cmd+b

### Save all 

	opt+cmd+s or use palette to search save. 
	
It will find save all, then enter. 

(Command key) - On some Apple keyboards, this key also has an Apple logo 

^(Control key) 

(Option key) - "Alt" may also appear on this key. It looks like a step.

(Shift key) It looks like an upwards-arrow key.

(Caps Lock) - Toggles Caps Lock on or off

Fn (Function key)

## Instant File Changing
[&#8629; TOP](#markdown-toc)

find files, 

    cmd+p (goto anything), 
fuzzy searching, like viewsindex. This will load the file automatically.

## Finding method/function or Symbols
[&#8629; TOP](#markdown-toc)

Find method/function in a file, 

	cmd+r or cmd+p and @. 

This can be used in CSS file as well.

To go to a certain place in css, 

	cmd+p style@field. This means going to field part of style.css. style@body 


## Keyboard binding
[&#8629; TOP](#markdown-toc)

don’t change preferences>key bindings default, use Key-binding>users and add

	{ "keys": ["ctrl+s"], "command": "toggle_side_bar" } 
or

	{ "keys": ["super+shift+5"], "command": "toggle_side_bar" }

This will toggle side bar with ctrl+s. don’f forget to take out the , comma at the end since it is the last one.

## Installing Plugins Without Package Control
[&#8629; TOP](#markdown-toc)

adding alias to mac through terminal for sublime text packages

	alias p="open ~/Library/Application\ Support/Sublime\ Text\ 2/Packages"

then just type p and enter.

when you want to clone github go to the dir ~/Library/Application\ Support/Sublime\ Text\ 2/Packages and git clone blablabla

## Package control
[&#8629; TOP](#markdown-toc)

copy from wbond.net /sublime_packages/package_control
ctrl ` to open console and paste it 

shift+cmd+p and type ‘install’ to bring up package control install, then type a name what you want to install, e.g. coffeescript or laravel blade etc.


## First snippet
[&#8629; TOP](#markdown-toc)

to open snippet, tool > snippets

By changing language at the bottom-right, snippets will change according to the language
shift+cmd+p and type snippets will bring all the snippets.

Adding a new snippet, tool>new snippet

### How to change syntax,

	shift+cmd+p and type syntax and select a syntax you want to use.

cmd / will give a comment or opt+cmd+/ will give a block comment.

### How to edit snippets
You can find your snippet in 

	~/library/Appication Support/Sublime Text 2/Packages/User/
	

### saving snippets

new folder, javascript, hello.sublime-snippet. you need sublime-snippet at the end. Since you saved a snippet in a javascript folder, it will appear when you are using syntax javascript.

a)
${1:this} bla bla ${2:snippet}
The first stop point and after : is the default value.

b)
Save as siaf.sublime-snippet, self invoking anonymous function under javascript folder,

	(function () {
	${1}
	})();
	<tabTrigger>siaf</tabTrigger>
	// indent works as well.

c) 
Save as model-extend.sublime-snippet under javascript>backbone, and trigger name is bm

	var {1:Task} = Backbone.Model.extend({
	${1}
	});
	<tabTrigger>bm</tabTrigger>

## Adding snippets through package control
[&#8629; TOP](#markdown-toc)

shift+cmd+p
install
select package to install

## Easier testing with snippets
[&#8629; TOP](#markdown-toc)

installing packages from github 

	cd ~/Library/Application\ Support/Sublime\ Text\ 2/Packages/User
	git clone https://github.com/florianeckerstorfer/sublime-phpunit.git PHPUnit


assertContails,
assertRegExp

Create php unit function snippet>tool>new snippet

	<snippet>
    <content><![CDATA[
	public function test${1}()
	{
   	 ${2}
	}
	]]></content>
	
    <!-- Optional: Set a tabTrigger to define how to trigger the snippet -->
    <tabTrigger>test</tabTrigger>
    <!-- Optional: Set a scope to limit where the snippet will trigger -->
    <scope>source.php</scope>
	</snippet>

Save it under PHPUnit directory as test.sublime-snippet
test, tab

## Zen coding / Deprecated, install Emmet
[&#8629; TOP](#markdown-toc)

use tab to change to html, no space between

	ul>li*4
	.container>header+.main+footer
	+ is siblings
	> is child

for ID use # like ul#someId
header and footer are html5
cursor need to be at the end of sequence
* is mutiple
for attribute use [ ]
	
	a.button[href=#] will be anchor with class button and href=”#”
	a[href=http://google.com] and tab

	li{my text} will be <li>my text</li>
	(ul>li*4>a[href=#]{some link})+div

Use ( ) to bundle together

	.container>(.header>header>h1{My webiste})+.main+.footer+footer

## Emmet
[&#8629; TOP](#markdown-toc)

This will replace zencoding in future.
[emmet](http://docs.emmet.io/)

	cd ~/Library/Application\ Support/Sublime\ Text\ 2/Packages
	git clone https://github.com/sergeche/emmet-sublime.git Emmet
Restart sublime
Use tab to convert to CSS

	.box{
	p20 tab will be padding: 20px;
	p50
	m10 will be margin: 10px:
	w50 will be width
	w80p will be width with 80%
	m10-20 will be margin: 10px 20px;
	m0-auto will be margin: 0 auto;

CSS3
transition: all 1s; and select cntr+cmd+x will use prefier to change 
Or
-transition and tab will add all the prefixes.

Don’t use prefix to all. Check it at caniuse.com
Or first add prefix and select all and cntr+cmd+x to prefixr will delete prefixed which you don’t need.

In html

	lorem20 will give 20 fake words.
	^ will go up one step in html
	.container>h1^.main(but it is not working yet)
	ul>.item*3 should (in future) give three li with class item under ul.
	em>.hello will give em and span with class hello since em only has span.


## Cross-Browser CSS with prefixr, prefixr.com
[&#8629; TOP](#markdown-toc)

Install it from package contrl.

Select css and edit>prefixr or ^+cmd+x


## Fetch files with ease
[&#8629; TOP](#markdown-toc)

pulling latest jquery etc
install from package control, shift+cmd+p and nettus+fetch

	shift+cmd+p and fetch manage to see samples.

Create a jquery.js and shift+command+p and fetch and file and select jquery to write jquery in the file.
shift+command+p and fetch and select package and type the location to download the package.

Adding normarize.css to Fetch

	https://github.com/necolas/normalize.css/

And click normalize.css and Raw. Find the webaddress.
In the above fetch manage file(Fetch.sublime-settings), add the following after jquery ...,(comma)

	"jquery": "http://code.jquery.com/jquery.min.js",
	"normalize":"https://raw.github.com/necolas/normalize.css/master/normalize.css"

It is ready. Create a file called normailze.css. Shift+cmd+p, fetch, file, and normalize.

Adding Zip files to Fetch.
e.g. Wordpress
Goto wp download and copy the download zip link with right-click copy link location.

Goto fetch.sublime-settings, and add the followings to packages after html5_boilerplate.

"wordpress": "http://wordpress.org/latest.zip"

Now shift+cmd+p, fetch, package, wordpress, add the folder at the bottom, enter.

## Lighting fast folder and file creation
[&#8629; TOP](#markdown-toc)

Install AdvancedNewFile

See details Sublime>Preferences>Browse Packages>Readme.md drop to sublime to read

    cmd+option(alt)+n>

then type directory/filename

## sidebar enhancement
[&#8629; TOP](#markdown-toc)

Create more options with right-click
install>sidebarenhancement
Open readme file to see the details.

contl+t will create a new file

change the command to open in a browser in the default(OSX).sublime-keymap 

	"keys": ["ctrl+o"],
		"command": "side_bar_open_in_browser" ,
now ctrl+o will open a page in a browser.

If you forget the shortcut, go to shift+cmd+p and type browser and select open in a browser.

Open a page in a server.

Save it as a project. 

Command palette (shift+cmd+p)>project>Save as
Then either right-click the project root on the left sidebar>Project>Edit Project

Right click any file on sidebar and select: "Project -> Edit Projects Preview URLs"

Edit this file, and add your paths and URLs with the following structure:

	{
       "/Applications/MAMP/htdocs/hwtrackerNov12":
        {
       "url_testing":"http://localhost:8888/hwtrackerNov12/"
        }
    }

And ctrl+o to open it in the local server

## Sublime Linter
[&#8629; TOP](#markdown-toc)

This will give you warning of coding mistakes. 

To disable it program palette, type lint and find disable lint.

## Code snippet management with Gists
[&#8629; TOP](#markdown-toc)

[Watch video](https://tutsplus.com/lesson/sexy-code-snippet-management-with-gists/)

Make an account at github gist, shinokadagist, for maintaining code snippets

Install Gists
Open Gist.sublime-settings to enter username and pw.

Write codes and select codes>Palette> create (public gist)> enter description(optional but enter name), like HTML: blabla, or Javascript: blabla>(optional)
 
Palette>open (gist)>select name> enter, this will open a new file which is connected to github gist. Copy and paste to your file.

If you edit the gist file >select the codes>Palette>gist(update)
Palette> gist open (in browser)>enter
creating Paul Irish’s pub/sub, .ir image replacement, jquery pub/sub
Or fork other’s gist and edit name like JavaScript: Detect IE. These will be displayed in your gist, and sublime as well.

Palette>open gist> select your file

## docBlockr
[&#8629; TOP](#markdown-toc)

Install package
Create a class and function,
	
	class MyClass{
	public function run($person, $howfar)
	{
		return “$person needs to run $howfar miles.”;
	}
	}

Then above the funciton type 
	
	/** and tab or enter. It will create a comment block.
	
	/**
	 * [run description]
	 * @param  [type] $person [description]
	 * @param  [type] $howfar [description]
	 * @return [type]
	 */
Tab to go to the next to type.
If you add ($person==”John doe”,...
It will detect the string and add it as string. If you add (Array $people, ..)
It will add Array $peple etc.

If the function name has ‘is’ or ‘has’ like, isRunning or hasRunningit will expect to return boolean. @return boolean in the comment.

Read more doc 


## Pretty Task Management
[&#8629; TOP](#markdown-toc)

Install PlainTasks
Palette>task
Type a heading like Things to do today, new line and type ‘go to the store’, cmd+enter.
You can use cmd+i to add another task.
Add -- and tab(or enter??) to add --------------------- line.
Create more headings and to do list.
Add tags like, @work, @personal etc
To add finished, go to the cmd+d for done. You can toggle with it.
to archive them shift+cmd+a
to move an item, cmd+ctrl+arrow up or down
save as this-week.TODO


## HTTP Requests within sublime
[&#8629; TOP](#markdown-toc)

Install HTTP Requester

At the moment only ‘get’ works.
Add //http://localhost:8888/hw_development/index.php/welcome/ in front of method and right click and select http requests

##  LiveReload
[&#8629; TOP](#markdown-toc)

Install it from Palette and also install browser extension as well.
For Chrome, Go to chrome extension to tick, Allow access to file URLs.
Open HTML or CSS. Turn on the LiveReload by clicking the icon. Open the sublime and browser next to next. Edit html or css and save it. You see the update when you save the file.


##  Regular expressions in sublime
[&#8629; TOP](#markdown-toc)

Edit>Convert Case>Title case will change to ‘this is title’ to ‘This Is Title”.
Goto Palette, and type title to select Title Case.
Turn on regexp at the left bottom or cmd+opt+r
cmd+i to incremental search, type to search, e.g. 
	
	<h2>.+</h2>. ‘.’ 

full stop means any character and + means one or more characters. Now option+enter will select all the occurring of 

	<h2>blabla</h2>. 
	
But this will select h2 tags as well. And h2 will be capitalize if you are doing Title case.
http://gskinner.com/RegExr/

	(?<=<h2>).+(?=</h2>) This will select between <h2> and </h2>
	(?<=ABC) means Positive lookbehind. Matches a group before your main expression without including it in the result.
	(?=ABC) means Positive lookahead. Matches a group after your main expression without including it in the result.
So cmd+optn+f and select regex at the left-bottom.

	cmd+K, cmd+u : upper case
	cmd+k, cmd+l: lower case
  
##  Vintage Mode
[&#8629; TOP](#markdown-toc)

This can use VIM command. 
Add     "ignored_packages": [] to Preferences Settings User. Not to ignore

	5j go 5 down.
viw, change to vidual, insert and select a word.

	ci’ //c: change, i:inner. This means change everything between/inner sigle quote,’. You can use anywhere in the line.
	ci{ wiil select all between { and }.

In a text, and command mode, c,w is change word and it will delete a word and change to insert mode.

	2 c w (or c 2 w) will delete 2 words and ready to change.
	v 5 w wiil select 5 words. visula 5 words.
	v t , will select til comma,. (visually select til comma).
	v f , will do the same.
	//capital A is move to the end of the line.
	//capital I is move to the beginning of the line.
	//capital V is to select the line. And jjjj to move down to select all.
	y for yank and p for paste
	d for delete, or d d for delete the line.
	y y p will copy and paste a line.
	. full stop will repeat the last command.


##  Quicker stylesheet references
[&#8629; TOP](#markdown-toc)

In Palette, copy (path from project)
and in index.html, link, tab and paste it.
Or copy (as Tag style) and paste it.
for js, copy as Tag script.

##  Joining lines
[&#8629; TOP](#markdown-toc)

Edit > lines > join lines
or cmd+j

cmd+left to go to the left, cmd+shift+right to select all the line.
cmd+j at the end of the sentence to join the next line and repeat it rather than back space it.

## Sublime and Markdown with marked
[&#8629; TOP](#markdown-toc)

[This is better.](http://theablefew.com/blog/sublime-text-2-as-a-markdown-editor)

Next, open up your Sublime Text 2 User Packages directory.

	/Users/username/Library/Application Support/Sublime Text 2/Packages/User

Make yourself a file called “Appname.sublime-build”, for example, if you use Mou it would be “Mou.sublime-build”. This is more for reference than anything, you could name it ButtSniff.sublime-build and it’ll work…
After that, open up Appname.sublime-build in your favorite text editor, which should be Sublime Text 2 at this time.
Inside, we’ll be using a very small piece of JSON.

	{
    "osx": {
        "cmd": ["open", "-a", "Appname", "$file"]
    },
    "selector": "text.html.markdown"
	}

Again, Appname would be changed to the name of the application you are using, so if you are using Mou, your code would look like: "cmd": ["open", "-a", "Mou", "$file"]
Once you have made this edit, save the file, and open up your markdown or rdoc file (may require a restart). Once you have the file open, just hit CMD+B  or tool>Build.

## All about Project
[&#8629; TOP](#markdown-toc)

In myapp.sublime-project file, You can add more than one 
	
	{
	"path": "/Applications/MAMP/htdocs/hw_development/appication"
	},
	{
	"path": "/Applications/MAMP/htdocs/hw_development/assets"
	}

Don’t forget , comma between array.
ctrl+cmd+p to select another project to switch. Or Project > switch Project in window

In order to exclude files/folders, add file_exclude_patterns or older_exclude_patterns

	{
	"path": "/Applications/MAMP/htdocs/hw_development/appication",
	“file_exclude_patterns”:[“*.css”,”*.js”],
	“folder_exclude_patterns”:[“js”,”css”]
	},
	…

You can add settings here as well.


	{
	“folders”;
	[…

	],
	“settings”:
	{
	“tab_size”:4

	}
	}

View>sidebar> show open files

## Configuring and mastering split windows
[&#8629; TOP](#markdown-toc)

cmd+shipt+[ or ] to move to next file.
crt+1 or crt+2 etc to move to next file.

Add the followings to Preferences>Key binding>User

	{
		"keys": ["super+alt+right"],
		"command": "set_layout",
		"args":
		{
			"cols": [0.0, 0.33, 1.0],
			"rows": [0.0, 1.0],
			"cells": [[0, 0, 1, 1], [1, 0, 2, 1]]
		}
	},

	{
		"keys": ["super+alt+left"],
		"command": "set_layout",
		"args":
		{
			"cols": [0.0, 0.66, 1.0],
			"rows": [0.0, 1.0],
			"cells": [[0, 0, 1, 1], [1, 0, 2, 1]]
		}
	},

	{ "keys": ["alt+1"], "command": "move_to_group", "args": { "group": 0 } },
	{ "keys": ["alt+2"], "command": "move_to_group", "args": { "group": 1 } },
	{ "keys": ["alt+3"], "command": "move_to_group", "args": { "group": 2 } },
	{ "keys": ["alt+4"], "command": "move_to_group", "args": { "group": 3 } }



The first one and two will separate the windows ⅓ and ⅔ .
The move_to_group will move the file to different group.

## Custom builds
[&#8629; TOP](#markdown-toc)


	function sayHi($name){
	return “Hi there, $name”;
	}
	echo sayHi(‘John’);


Then you can run in a terminal like, php funcs.php

Build script
Tools>build system>build new system

In a terminal, php -help will give, e.g. -l           Syntax check only (lint)

[Sublime Doc](http://docs.sublimetext.info/en/latest/reference/build_systems.html#build-system-variables)

Save the new system as PHP-Lint.sublime-build

	{
		"cmd": ["php","-l","$file"]
	}

Save it under 

	/Library/Application Support/Sublime Text 2/Packages/User/newfolder/PHP-Lint.sublime-build

To select it through, Tools>Bilde System>PHP-Lint
To run, Tools>build OR cmd+b

This will show Lint.

In order to display output, Save the following as PHP.sublime-build

	{
		"cmd": ["php","$file"]
	}
Select it under Tools>Build System>PHP

Then run cmd+b

Using coffee script

create a file script.coffee with following code.

	sayHell = (name) -> "Hi there, #{name}."
in a terminal 

	coffee -c script.coffee
This will create a file called script.js in the same folder with followings.
	// Generated by CoffeeScript 1.4.0
	
	(function() {
  		var sayHell;

  		sayHell = function(name) {
    	return "Hi there, " + name + ".";
  	};

	}).call(this);

Creating coffee-script compiler through Tools>Build System>New Build System
	
	{
	// coffee -c script.coffee
	"cmd": ["coffee","-c", "$file"]
	}

Save it under build folder which I made it before, as my-coffeescript.sublime-build
Select my-coffeescript from Tools>Build System, and run by cmd+b.
Change -c to -wc to watch and compile.
Save it and build it and see.


## My notes:
[&#8629; TOP](#markdown-toc)

Spelling check
Open Preferences>Settings>User and add,
 "spell_check": true,
"dictionary": "Packages/Language - English/en_US.dic"

And add the following to Preferences>Key Binding>User

	{ "keys": ["super+\\"], "command": "toggle_setting", "args": {"setting": "spell_check"} },
	{ "keys": ["super+ctrl+\\"], "command": "next_misspelling" },
	{ "keys": ["super+alt+\\"], "command": "prev_misspelling" }



Additional notes
cmd+opt+n to create folder and file, css/style.css and save it. This is from package advancedNewFile



