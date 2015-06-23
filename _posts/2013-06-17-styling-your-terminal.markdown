---
layout: post
title: "Styling your Terminal"
date: 2013-06-10 21:31
categories: Terminal
---


[oh my zsh](http://github.com/robbyrussell/oh-my-zsh)


Type in terminal

	curl -L https://github.com/robbyrussell/oh-my-zsh/raw/master/tools/install.sh | sh
	
Restart the terminal

Now create a dir

	mkdir test
	cd test
	git init
	cd ..
	ls
	cd test
	// it will give the git repo 
	git add . //ever there is no file to add, just for example
	// it will give all the option list
	git commit -(click tab to show all the options)
	
[Themes](https://github.com/robbyrussell/oh-my-zsh/wiki/themes)



To use: Set ZSH_THEME in ~/.zshrc to blinks, and set up Solarized.

	vim ~/.zshrc
	ZSH_THEME="blinks"
	
[Set up Solarized](http://ethanschoonover.com/solarized)


Clone github or Download ZIP and open it in Finder and click Solarized Dark xterm-256color.terminal.

osx-terminal.app-colors-solarized > xterm-256color > Solarized Dark xterm-256color.terminal

Go to terminal preferences > settings > Solarized Dark change 
	font size 13
	
Then change back ZSH_THEME="blinks" to ZSH_THEME="robbyrussell"

Go to the terminal preferences setting to make Solarized Dark the default setting.  

In order to use transparency in full scree, you need to use iTerm. oh-my-zsh is already installed in iTerm. Go to the downloaded solarized 

solarized / iterm2-colors-solarized / and click Solarized Dark.itermcolors

After successful installation of theme, go to iTerm preferences Profile to set up a new profile.

- Click + to add a new profile. Shin's Profile.  
- Go to Keys and add ctrl+cmd+t to Hotkey. Tick Show/hide iTerms 2 with a system-wide hotkey.
- Then to go color and load Presets to select Solarized Dark.
- Under Text tab to change the size of font.
- Under Window tab, change transparency 
- Go to General in the main menu and uncheck Use Lion-style Fullcreen windows
- Go back to profile and under Other Actions, set as Default
- Close iTerm and reopen it.

cmd+enter will change to full screen and hotkey to show/hide. ctrl+cmd+t.






	
	

