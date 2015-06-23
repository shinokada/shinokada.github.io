---
layout: post
title: "Learning Pry Personal notes on Pry"
date: 2013-11-10 10:27
comments: true
categories: pry
---

This is my personal notes on Pry, Part 1.


Notes from the following sites.

- [pry wiki](https://github.com/pry/pry/wiki)
- [Introductory screencast](http://pryrepl.org/screencasts.html)
- [Pry~IRB (Japanese)](http://www.school.ctc-g.co.jp/columns/masuidrive/masuidrive03.html)


Install it.

    $ gem install pry
    # version
    $ pry -v
    # enter to pry
    $ pry
    # help
    pry(main)> help
    
    # To customize change ~/.pryrc file
    # get help on command
    pry(main)> show-doc -h
    
    # Showing rubydoc 
    pry(main)> s ="pry"
    pry(main)> show-doc s.each_line
    
    # or
    pry(main)> show-doc String#each_line
    
    # to show method
    pry(main)> show-method s.each_line
    
    # or
    pry(main)> show-source s.each_line
    
    # adding line number
    pry(main)> show-source s.each_line -l
    
    # creating gist
    # "gem install gist" and afer pry console enter "install-command gist"
    pry(main)> gist s.each_line
    Gist created at URL https://gist.github.com/acfc1489eaa0dbbc3f0a, which is now in the clipboard.


## Using command shell in pry
Using . dot.

    # Showing methods in IRB is very messy but pry shows them line by line.
    pry(main)> self.methods
    
    # Using shell command with .(dot)
    pry(main)> .ls -al

Or use shell-mode.
	
    pry(main)> shell-mode
    pry main:/Users/myname/rubytest $ .cd rubytest
    pry main:/Users/myname/rubytest $ .ls
    addtemps.rb	list.txt	temps.txt
    exception1.rb	tempavg.rb	tkexample.rb
    pry main:/Users/myname/rubytest $ .vim test6.rb
    pry main:/Users/myname/rubytest $ load "test6.rb"
    => true
    pry main:/Users/myname/rubytest $ hello_world
    hello world!
    => nil

Getting out of shell-mode.
	
    # just type shell-mode again
    $ pry
    [1] pry(main)> shell-mode
    pry main:/Users/myname/first_app $ shell-mode
    [3] pry(main)>



## List methods of defined context


`ls` to list methods of defined context.

    pry(main)> s="pry"
    pry(main)> ls s
    
    # Use cd to change to the obect. Then use ls to show all methods 
    # You can create an object and cd 
    pry(main)> require 'net/http'
    => true
    pry(main)> url = URI.parse('http://www.zenet-web.co.jp/index.html')
    => #<URI::HTTP:0x007faf6247d560 URL:http://www.zenet-web.co.jp/index.html>
    pry(main)> req = Net::HTTP::Get.new(url.path)
    => #<Net::HTTP::Get GET>
    pry(main)> res = Net::HTTP::start(url.host, url.port) do |http|
    pry(main)*   http.request(req)
    pry(main)* end
    => #<Net::HTTPOK 200 OK readbody=true>
    pry(main)> cd res
    pry(#<Net::HTTPOK>):1> ls
    Net::HTTPHeader#methods:
    ...
    pry(main)>
    
    # go back to the main
    pry(main)> cd

## Calling pry from runtime

test.rb

    require 'pry'
    puts "Run Pry!"
    binding.pry

Run test.rb. It will start pry after running the script.

    $ ruby test5.rb
    Run Pry!
    
    From: /Users/teacher/Documents/rubytest/test5.rb @ line 5 :
    
        1: require 'pry'
        2:
        3:
        4: puts "Run Pry!"
     => 5: binding.pry

Another example

    # test.rb
    require 'pry'
    
    class A
      def hello() puts "hello world!" end
    end
    
    a = A.new
    
    # set x to 10
    x = 10
    
    # start a REPL session
    binding.pry
    
    # program resumes here (after pry session)
    puts "program resumes here. Value of x is: #{x}."

Run it from your terminal

    $ ruby test4.rb
    
    From: /Users/teacher/Documents/rubytest/test4.rb @ line 14 :
    
         9:
        10: # set x to 10
        11: x = 10
        12:
        13: # start a REPL session
     => 14: binding.pry
        15:
        16: # program resumes here (after pry session)
        17: puts "program resumes here. Value of x is: #{x}."
    
    [1] pry(main)> a.hello
    hello world!
    => nil
    [2] pry(main)> puts x
    10
    => nil
    [3] pry(main)> def a.goodbye
    [3] pry(main)*   puts "Goodbye world!"
    [3] pry(main)* end
    => nil
    [4] pry(main)> a.goodbye
    Goodbye world!
    => nil
    [5] pry(main)> x="changed"
    => "changed"
    [6] pry(main)> exit
    program resumes here. Value of x is: changed.

## Using pry on a rails project

Install pry-rails gem.
	
    $ gem install pry-rails

Add to Gemfile
	
    group :development do
      gem 'sqlite3', '1.3.7'
      gem 'pry-rails'
    end

You can add pry
Run rails console.
	
    $ rails c


