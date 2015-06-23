---
layout: post
title: "Command line apps with Ruby"
date: 2013-10-27 22:34
categories: Ruby
---

	
{% highlight text %}
$ cd /Users/myname/Documents/command_line_apps_in_ruby_demo
{% endhighlight %}

app.rb

{% highlight ruby %}
require "thor"

class App < Thor
  desc "hello WORD", "Prints 'Hello WORD' to the screen."
  def hello word
    puts "Hello #{word}"
  end

  desc "list_recipes [KEYWORD] [OPTIONS]", "List all recipes. If a keyword is given, it filters the list based off it."
  option :format
  def list_recipes keyword=nil
    # puts options[:format]
    recipes = [
      {
        title: "Ratatouille",
        cooking_time: "60 min",
        ingredients:  %w(potatoes carrots peppers onions zucchini tomatoes)
      },
      {
        title: "Mac & Cheese",
        cooking_time: "20 min",
        ingredients: %w(macarroni cheese mustard milk)
      },
      {
        title: "Caesar Salad",
        cooking_time: "10 min",
        ingredients:  %w(chicken lettuce croutons eggs)
      }
    ]

  recipes_to_be_listed = if keyword.nil? then recipes
                         else recipes.select { |recipe| recipe[:title].downcase.include? keyword.downcase}
                         end

    recipes_to_be_listed.each do | recipe |
      puts "-------------"
      puts "Recipe: #{recipe[:title]}"
      puts "It takes: #{recipe[:cooking_time]} to cook."
      puts "The ingredients are: #{recipe[:ingredients].join(", ")}"
      puts ""
    end
  end	  
end

App.start ARGV
# ARGV is for options, arguments, subcommand to be parsed in app.
{% endhighlight %}

desc will be used when you type ruby app.rb to describe the method.

To find out a recipes which name includes "oui" and print out format option to test it if it picks up the format or not.

{% highlight text %}
$ ruby app.rb list_recipes oui --format sometext
{% endhighlight %}


## Adding options and arguments

{% highlight ruby %}
require "thor"

class App < Thor
  desc "hello WORD", "Prints 'Hello WORD' to the screen."
  def hello word
    puts "Hello #{word}"
  end

  desc "list_recipes [KEYWORD] [OPTIONS]", "List all recipes. If a keyword is given, it filters the list based off it."
  option :format
  option :show_time, type: :boolean, default: true #--show-time --no-show-time
  def list_recipes keyword=nil
    recipes = [
      {
        title: "Ratatouille",
        cooking_time: "60 min",
        ingredients:  %w(potatoes carrots peppers onions zucchini tomatoes)
      },
      {
        title: "Mac & Cheese",
        cooking_time: "20 min",
        ingredients: %w(macarroni cheese mustard milk)
      },
      {
        title: "Caesar Salad",
        cooking_time: "10 min",
        ingredients:  %w(chicken lettuce croutons eggs)
      }
    ]

    recipes_to_be_listed = if keyword.nil? then recipes
                         else recipes.select { |recipe| recipe[:title].downcase.include? keyword.downcase}
                         end

    recipes_to_be_listed.each do | recipe |
      if options[:format].nil?
        print_default recipe
      else options[:format] == "oneline"
        print_oneline recipe
      end

    end
  end	  

  private

  def print_default recipe
    puts "-------------"
    puts "Recipe: #{recipe[:title]}"
    puts "It takes: #{recipe[:cooking_time]} to cook."
    puts "The ingredients are: #{recipe[:ingredients].join(", ")}"
    puts ""
  end

  def print_oneline recipe
    if options[:show_time]
      time = "(#{recipe[:cooking_time]})"
    else
      time = ""
    end

    puts %Q{#{recipe[:title]} #{time}}
  end

end

App.start ARGV
# ARGV is for options, arguments, subcommand to be parsed in app.

{% endhighlight %}

option :show_time in code and --show-time in command line. Thor is cleaver enough to understand no in --no-show-time.
Test this with the followings.
	
{% highlight text %}
# 1.
$ ruby app.rb list_recipes --format oneline
# or 
$ ruby app.rb list_recipes --format "oneline"

# 2.
$ ruby app.rb list_recipes --format="oneline"

# 3.
$ ruby app.rb list_recipes --format="oneline" --no-show-time
{% endhighlight %}

option :show_time, type: :boolean, default: true #--show-time --no-show-time

If there is no default: true, the default is false. Thor is clever enough to recognize --show-time as true and --no-show-time as false.

In option you use "-" like --show-time, but in method use under-bar, options[:show_time].

If you add required true, like option: format, required: true, then you are required the option.


## Subcommands

### Adding recipes add method

Examples are "git remote add", "git remote list", etc.

Every Thor application can be hooked inside another application.

subcommand `<name of command>`, `<name of class to hook>`. This can be used as app.rb recipes add. The add options will be defined as Thor options, not arguments. Arguments are explicit in the form of options. Rather than app.rb recipes add title time description, it is better to be app.rb recipes add --title="" --time="" --description=""

app2.rb

{% highlight ruby %}
require "thor"

RECIPES = [
      {
        title: "Ratatouille",
        cooking_time: "60 min",
        ingredients:  %w(potatoes carrots peppers onions zucchini tomatoes)
      },
      {
        title: "Mac & Cheese",
        cooking_time: "20 min",
        ingredients: %w(macarroni cheese mustard milk)
      },
      {
        title: "Caesar Salad",
        cooking_time: "10 min",
        ingredients:  %w(chicken lettuce croutons eggs)
      }
    ]

class Recipes < Thor
  desc "add --title --cooking-time --description", "Adds a new recipe."
  option :title, required: true
  option :cooking_time, required: true
  option :description, required: true

  def add # app.rb recipes add --title="" --cooking-time="" --description=""
    recipe = {
      title: options[:title],
      cooking_time: options[:cooking_time],
      description: options[:description]
    }

    RECIPES << recipe

    RECIPES.each do |recipe|
      puts recipe[:title]
    end
  end
end

class App < Thor
  desc "hello WORD", "Prints 'Hello WORD' to the screen."
  def hello word
    puts "Hello #{word}"
  end

  desc "recipes", "Manages recipes"
  subcommand "recipes", Recipes # app.rb recipes add 

  desc "list_recipes [KEYWORD] [OPTIONS]", "List all recipes. If a keyword is given, it filters the list based off it."
  option :format
  option :show_time, type: :boolean, default: true #--show-time --no-show-time
  def list_recipes keyword=nil
    recipes = RECIPES
    recipes_to_be_listed = if keyword.nil? then recipes
                         else recipes.select { |recipe| recipe[:title].downcase.include? keyword.downcase}
                         end

    recipes_to_be_listed.each do | recipe |
      if options[:format].nil?
        print_default recipe
      else options[:format] == "oneline"
        print_oneline recipe
      end

    end
  end   

  private

  def print_default recipe
    puts "-------------"
    puts "Recipe: #{recipe[:title]}"
    puts "It takes: #{recipe[:cooking_time]} to cook."
    puts "The ingredients are: #{recipe[:ingredients].join(", ")}"
    puts ""
  end

  def print_oneline recipe
    if options[:show_time]
      time = "(#{recipe[:cooking_time]})"
    else 
      time = ""
    end

    puts %Q{#{recipe[:title]} #{time}}
  end

end

App.start ARGV
# ARGV is for options, arguments, subcommand to be parsed in app.

{% endhighlight %}

Test it.
  
{% highlight text %}
# 1. 
$ ruby app2.rb recipes add # this will give an error

# 2. 
$ ruby app2.rb recipes add --title="Steak" --cooking-time="10 min" --description="Good ol' steak"
{% endhighlight %}

### Adding recipes list

app3.rb

{% highlight ruby %}
require "thor"

RECIPES = [
      {
        title: "Ratatouille",
        cooking_time: "60 min",
        ingredients:  %w(potatoes carrots peppers onions zucchini tomatoes)
      },
      {
        title: "Mac & Cheese",
        cooking_time: "20 min",
        ingredients: %w(macarroni cheese mustard milk)
      },
      {
        title: "Caesar Salad",
        cooking_time: "10 min",
        ingredients:  %w(chicken lettuce croutons eggs)
      }
    ]

class Recipes < Thor
  desc "add --title --cooking-time --description", "Adds a new recipe."
  option :title, required: true
  option :cooking_time, required: true
  option :description, required: true

  def add # app.rb recipes add --title="" --cooking-time="" --description=""
    recipe = {
      title: options[:title],
      cooking_time: options[:cooking_time],
      description: options[:description]
    }

    RECIPES << recipe

    RECIPES.each do |recipe|
      puts recipe[:title]
    end
  end

  desc "list [KEYWORD] [OPTIONS]", "List all recipes. If a keyword is given, it filters the list based off it."
  option :format
  option :show_time, type: :boolean, default: true #--show-time --no-show-time

  def list keyword=nil
    recipes = RECIPES
    recipes_to_be_listed = if keyword.nil? then recipes
                         else recipes.select { |recipe| recipe[:title].downcase.include? keyword.downcase}
                         end

    recipes_to_be_listed.each do | recipe |
      if options[:format].nil?
        print_default recipe
      else options[:format] == "oneline"
        print_oneline recipe
      end

    end
  end   

  private

  def print_default recipe
    puts "-------------"
    puts "Recipe: #{recipe[:title]}"
    puts "It takes: #{recipe[:cooking_time]} to cook."
    puts "The ingredients are: #{recipe[:ingredients].join(", ")}"
    puts ""
  end

  def print_oneline recipe
    if options[:show_time]
      time = "(#{recipe[:cooking_time]})"
    else 
      time = ""
    end

    puts %Q{#{recipe[:title]} #{time}}
  end
end

class App < Thor
  desc "hello WORD", "Prints 'Hello WORD' to the screen."
  def hello word
    puts "Hello #{word}"
  end

  desc "recipes", "Manages recipes"
  subcommand "recipes", Recipes # app.rb recipes add 


end

App.start ARGV
# ARGV is for options, arguments, subcommand to be parsed in app.

{% endhighlight %}

Test it.
  
{% highlight text %}
# 1. 
$ ruby app3.rb recipes list

# 2. This will show help message for recipes.
$ ruby app3.rb recipes 

{% endhighlight %}

Thor will automatically insert options, so change,
    desc "add --title --cooking-time --description", "Adds a new recipe."
to
      desc "add", "Adds a new recipe."

Adding alias for options. This allow to type -t rather than --title etc. You can use only one letter after -, like, -t, -c or -d.
- is mandatory.
  
{% highlight ruby %}
  option :title, required: true, aliases: "-t"
  option :cooking_time, required: true, aliases: "-c"
  option :description, required: true, aliases: "-d"
{% endhighlight %}

Test it. 
  
{% highlight text %}
# 1. To display help
$ ruby app4.rb recipes

# 2.
$ ruby app4.rb recipes add -c="10 min" -t "Steak" -d "Good ol' steak."
{% endhighlight %}

## GLI

Aims for a higher standard.
Better documentation, better testing, better dependency management.

Install gem gli
  
{% highlight text %}
$ gem install gli
# Initialize with name of app and methods you want.
$ gli init recipes list add
{% endhighlight %}

switch in GLI is the same as boolean flag in Thor.

In bin/recipes.rb

{% highlight ruby %}
# In Thor
option :s, type :boolean
# In GLI
switch [:s,:switch]

# flag in GLI is the same as option in Thor without using boolean type.

{% endhighlight %}


