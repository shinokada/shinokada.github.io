---
layout: post
title: "Using Terminal"
date: 2013-06-09 22:00
categories: Terminal
---

## hello world in terminal

	echo 'hello world!'
	echo -n 'hello! world'
	
-n surpress the new line

ctrl+A go to the begginning of the line

ctrl+E go to the end of line

up/down key shows history of command

~, tilda is the variable for home directory

tab completion

clear command to clean the screen

man echo

to quit man page is to type q

### Navigation
- Page Down = Spacebar or the ‘Page Down’ Key
- Page Up = b or the ‘Page Up’ Key
- Line Down = j or the ‘Down Arrow’ Key
- Line Up = k or the ‘Up Arrow’ Key
- Top of Document = g
- Bottom of Document = G
- Quit = q
- Search = / to search forward [example /keyfile ] |or| ? to search backward [ ?keyfile ]
- Repeat Search = n to repeat the search forward and N to repeat the search in the opposite direction
- Help = h | Will give you the full summary of Less commands

## Navigating the File System

	ls
	pwd
	ls Documents //ls directory
	ls Documents Music // ls directories
	ls /Users/js/Documents //using absolute path
	ls Documents // same command as above
	ls ~/Documents //same command as above
Case sensitive on Linux but not in Mac

For space use \

	ls Some Directory // this will pick up two direcories
	ls Some\ Direcotry
	ls "Some Directory"
	ls 'Some Directory'
	ls -l // details list
	ls -a //all the files including hidden files
	ls -t //sort by time last modified
	ls -t -l -a
	ls -tla //or ls -alt etc
	cd .. //one level up the directory

 . is current directory

Mac finder integration in Mac. Drag a file on the teminal will show the absolute path of the file.

This will open the current directory in the Finder in Mac

	open . 
	
This will open a file with the default text editor.
	
	open finename.txt 	

This will open a file with nameofApp.

	open -a nameofApp nameofFile.txt
	
-a stand for application
	
-R reveal the file in the Finder

	open -R filename.txt	
	
Open a website in a default browser

	open http://google.com

Displaying absolute path on the Finder

	defaults write com.apple.finder _FXShowPosixPathInTitle -bool YES
	
	killall Finder

## Files, Links, and CRUD
### Copying
	touch finename
	nano afile
	mkdir dirname
	touch adir/anotherfile
	ls adir
	cp afile afile.bak
	
If there is a superimportantfile, this will overwrite, so be careful.
	
	cp afile superimportantfile //
	cp adir seconddir // this will give an error
	cp -r adir seconddir
	
### Moving
	mv afile adir // or mv afile ~/adir/
	
The following will move a file and rename it as afile-backup
	
	mv afile.bak ~/adir/afile-backup 
	//renaming by mv command
	mv afile secondfile
	
	cd ~
	touch file1 file2 file3 filejesse filejohn
	
Moving all files with 'file' name 
	
	mv file* adir/
### Delete

rm command deletes files, be super careful

	rm afile
In order to delete a directory you need to use -r
	
	mkdir folderr
	touch folderr/afile
	ls folderr
	rmdir folderr // error
	rmdir -r folderr

### Symbolic link/Soft link and Hard link

Go to the dir where you want to add a symlink and use ln -s <destination> <linkname> to create the link.
	
	cd /dir/where/youwant/toaddsymlink/ //link will be created in this dir
	ln -s <destination folder/file name> <linkname>

In the video

	touch afile 
	ln -s afile symlinforafile
	ls
	nano afile
	// add hey there~
	cat afile // to show the content hey there~
	cat symlinkforafile // this will show hey there~ as well

But if you move a file, the link will break.

	mv afile afile2
	cat symlinkforafile // this is broken
	
A hard link won't break.

	rm symlinkforafile
	ls
	ln afile2 hardlinkforafile2
	cat afile2 // shows hey there~
	mv afile2 afile3 //move the original
	ls
	cat hardlinforafile2 // this is not broken and shows hey there~

## Finding Files

	mkdir searchHere
	cd searchHere
	touch CaSeFiLe afile.doc athird newestfile aDir anotherfile bigfile smallfile afile anotherfile.doc fourth.txt
	cd aDir
	touch file1.txt file2.txt file3.txt
	cd ..

### Find by type

	find . -type f //find name of dir, . means current dir and will find in aDir as well
	
### Find by name

	find . -name "newestfile" //this will find ./newestfile
	find . -name "*.txt" //find recursively
	//case incensitive -iname
	find . -iname "*text"

### Find by size

	find . -size +2048 //more than 2048, 512bite which is more than 1MB
	find . -size -2048 //find less than 1MB size
	
### find by time of modification, creation or last accessed date
	find . -mtime -1 //by modification time less than one day old
	find . -mtime +1 // older than one day
	find . -atime +1  // last accessed time more than one day
	find . -atime -1 // accessed during the last day
	find . -ctime -1 // created within one day

### Combining them
	find . -iname "*.txt" -or -iname "*.doc" // will find .txt or .doc
	find . -iname "*.txt" -or -iname "*.doc" -and -mtime -1 // will find .txt or .doc and modified within one day.
	
### 	Exclude a dir from search 
	find . -iname "*.txt"
	find . -iname "*.txt" -or -iname "aDir" -prune // -prune means do not include in the seach but this will print aDir dir
	find . -iname "*.txt" -print -or -iname "aDir" -prune// this won't print aDir
	
	
### Search file for content

	grep "Hello" afile// search word, file name
	grep -i "Hello" afile //case incensitive match
	grep -il "Hello" afile //-l will return the file name
	grep -il "Hello" * //return all the file names
	grep -ilr "Hello" afile //-r recursively search the current dir

Searching recursively  and type of files won't work. You need to use 'find'.

	grep -ilr "Hello" *.txt // this won't work
	find . -name "*.txt" -exec grep -il "Hello" {} \;

-exec is passing -name "*.txt" to grep. {} is placehoder to put the outputs. and escape \;
	

## Managing File Permissions

	cd Documents
	touch afile.txt
	ls -l
	
-rw-r--r--

The first - is a regular file. d:directory, l:symbolic link

user(u), group(g), anyone(o) else. The above one, the user can read and write but not able to execute. Group and anyone else can only read. 

	chmod g+w afile.txt //giving write to group
	ls -l
	chmod o+w afile.txt //giving write to others
	chmod a+x afile.txt //'a' stands for user, group and others. Giving executable to all.
	
Remove permissions by - instead of +. go is the same as og means group and others.

	chmod og-x afile.txt
	ls -l

Giving execution and taking writing permission to group.

	chmod g+x-w afile.txt
	ls -l

Giving group read, write and execution permission by using =
	
	chmod g=rwx afile.txt
	ls -l
	
Taking out all permissions

	chmod g= afile.txt
	
Giving different permissions to different group.

	chmod u=rw, g=,o=x afile.txt
	ls -l
	
In absolute form using numbers. 4:read, 2:write, 1:execute

	chmod 444 afile.txt
	ls -l
	chmod 744 afile.txt
	chmod 700 afile.txt
	
	chmod 644 afile.txt





