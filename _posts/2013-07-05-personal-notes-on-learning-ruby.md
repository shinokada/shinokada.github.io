---
layout: post
title: "Personal Notes on Learning Ruby"
date: 2013-07-05 22:22
categories: Ruby
---


## First Ruby code
    
    mkdir learnruby
    cd learnruby
    irb
    2+3
    5*40
    25/2
    25.0/2
    25.class #Fixnum
    25.0.class # Float
    25.methods # [: to_s :-0, :+ ...etc.]
    "Jimmy"
    "Jimmy".reverse
    "Jimmy"*5
    def add_and_power a,b # or (a,b)
    (a+b)**(a+b)
    end # nil
    add_and_power (2,3) # 3125
    self # main
    self.class # Object
    self.methods # [:to_s, :public ....etc]
    s1 = :foo
    s2 = :foo
    s1.equal? s2 # true
    str1 = "Jimmy"
    str2 = "Jimmy" 
    str1 == str2 # true
    str1.equal? str2 # false
    # exit irb by ctrl+d
    
    
    # creating first ruby file
    vim add_and_power.rb
    # in vim
    def add_and_pwer a,b
    (a+b)**(a+b)
    end
    
    input1 = gets
    input2 = gets
    
    puts add_and_power input1, input2
    
    # save it and run ruby code
    esc
    :!w
    :!ruby add_and_power.rb
    # this gives an error
    # You need to change input1 and 2 to integer
    # change to 
    puts add_and_power input1.to_i, input2.to_i
    
    esc
    :!w
    :!ruby add_and_power.rb
    
    # make it more interactive
    # final code
    def add_and_power a,b
    (a+b)**(a+b)
    end
    
    puts *First number please? *
    input1 = gets
    
    puts *Second number please? *
    input2 = gets
    
    esc
    :!w
    :!ruby add_and_power.rb



## Conditionals

Use if elsif, but watch out not elseif like php.

    
    #1. ask for age
    
    age = gets.to_i
    
    #2. process
    
    if age < 10
      puts "It's a young person."
    elsif age < 19
      puts "It's a teen ager."
    elsif age < 45
      puts "It's an adult."
    elsif age < 95
      puts "It's an old person."
    else
      puts "Is he dead?"
    end
    
    :w age.rb
    :!ruby age.rb
    
    
        
    # ex 2
    output = if age < 10
          "it's a young person."
        elsif age < 19
          "it's a teen ager."
        elsif age < 45
          "it's an adult."
        elsif age < 95
          "it's an old person."
        else
          "is he dead?"
        end
    
    puts output



## Sublime text and Ruby

Use which ruby to find your default Ruby and use it in the following.

If you want to use sublime text build with Ruby, you need to change /Users/yourname/Library/Application Support/Sublime Text 2/Packages/Ruby/Ruby.sublime-build


    
    {
        "cmd": ["/Users/yourname/.rbenv/shims/ruby", "$file"],
        "file_regex": "^(...*?):([0-9]*):?([0-9]*)",
        "selector": "source.ruby"
    }




After installing SublimeREPL, you need to change 
/Users/yourname/Library/Application Support/Sublime Text 2/Packages/SublimeREPL/config/Ruby/Main.sublime-menu

For my case I chagned osx.

    
    "cmd":{
            "windows":[
              "ruby.exe",
              "${packages}/SublimeREPL/config/Ruby/pry_repl.rb",
              "$editor"
            ],
            "linux":[
              "ruby",
              "${packages}/SublimeREPL/config/Ruby/pry_repl.rb",
              "$editor"
            ],
            "osx":[
              "/Users/yourname/.rbenv/shims/ruby",
              "${packages}/SublimeREPL/config/Ruby/pry_repl.rb",
              "$editor"
            ]

