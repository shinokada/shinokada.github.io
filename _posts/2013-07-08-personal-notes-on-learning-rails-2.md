---
layout: post
title: "Personal Notes on Learning Rails 2"
date: 2013-07-08 17:27
categories: Rails
---


## each, map and times

		
{% highlight ruby %}
>> (1..5).each{|i| puts i**2}
1
4
9
16
25
=> 1..5

>> (1..5).map{|i| i**2}
=> [1, 4, 9, 16, 25]

>> 3.times { puts "Betelgeuse!" }   # 3.times takes a block with no variables.
"Betelgeuse!"
"Betelgeuse!"
"Betelgeuse!"
=> 3

>> %w[a b c]                 # Recall that %w makes string arrays.
=> ["a", "b", "c"]
>> %w[a b c].map { |char| char.upcase }
=> ["A", "B", "C"]
>> %w[A B C].map { |char| char.downcase }
=> ["a", "b", "c"]


{% endhighlight %}

map method returns the result of applying the given block to each element in the array or range.

## %w and -1

{% highlight ruby %}
>> a = %w[foo bar baz quux]    # Use %w to make a string array.

>> a = %w[0 1 2 3 4 5 6 7 8 9]
>> a[2..-1]                    # Use the index -1 trick.
=> ["2", "3", "4", "5", "6", "7", "8", "9"]
{% endhighlight %}

### bang!, push, << 

{% highlight ruby %}
>> a
=> [42, 8, 17]
>> a.sort!
=> [8, 17, 42]
>> a
=> [8, 17, 42]

>> a.push(6)                  # Pushing 6 onto an array
=> [42, 8, 17, 6]
>> a << 7                     # Pushing 7 onto an array
=> [42, 8, 17, 6, 7]
>> a << "foo" << "bar"        # Chaining array pushes
=> [42, 8, 17, 6, 7, "foo", "bar"]

{% endhighlight %}


### join and split

split convert a string to an array. join method from an array to a string.

{% highlight ruby %}
>> a
=> [42, 8, 17, 7, "foo", "bar"]
>> a.join                       # Join on nothing.
=> "428177foobar"
>> a.join(', ')                 # Join on comma-space.
=> "42, 8, 17, 7, foo, bar"
{% endhighlight %}


### to_a and range


{% highlight ruby %}
# converting a range to arrays using the to_a method
>> (0..9).to_a            # Use parentheses to call to_a on the range.
=> [0, 1, 2, 3, 4, 5, 6, 7, 8, 9]

>> a = %w[foo bar baz quux]         # Use %w to make a string array.
=> ["foo", "bar", "baz", "quux"]
>> a[0..2]
=> ["foo", "bar", "baz"]

# Using rang on array
>> a[2..-1]                         # Use the index -1 trick.
=> [2, 3, 4, 5, 6, 7, 8, 9]
{% endhighlight %}

### Curly braces and do..end

Using curly braces only for short one-line blocks and the do..end syntax for longer one-liners and for multi-line blocks:


### shuffle
	
{% highlight ruby %}
>> ('a'..'z').to_a.shuffle[0..7].join
=> "bjglxomw"
>> ('a'..'z').to_a.shuffle[0..7]
=> ["y", "h", "l", "v", "i", "d", "p", "f"]
{% endhighlight %}

## Hashes and symbols
	
{% highlight ruby %}
>> user = {}                          # {} is an empty hash.
=> {}
>> user["first_name"] = "Michael"     # Key "first_name", value "Michael"
=> "Michael"
>> user["last_name"] = "Hartl"        # Key "last_name", value "Hartl"
=> "Hartl"
>> user["first_name"]                 # Element access is like arrays.
=> "Michael"
>> user                               # A literal representation of the hash
=> {"last_name"=>"Hartl", "first_name"=>"Michael"}
{% endhighlight %}

Hashes don’t generally guarantee keeping their elements in a particular order. If order matters, use an array.


	
{% highlight ruby %}
# hashrocket => 
>> user = { "first_name" => "Michael", "last_name" => "Hartl" }
=> {"last_name"=>"Hartl", "first_name"=>"Michael"}
{% endhighlight %}

### Symbols

Symbols are a special Ruby data type

	
{% highlight ruby %}
>> user = { :name => "Michael Hartl", :email => "michael@example.com" }
=> {:name=>"Michael Hartl", :email=>"michael@example.com"}
>> user[:name]
=> "Michael Hartl"

# New notation with :
>> h1 = { :name => "Michael Hartl", :email => "michael@example.com" }
=> {:name=>"Michael Hartl", :email=>"michael@example.com"}
>> h2 = { name: "Michael Hartl", email: "michael@example.com" }
=> {:name=>"Michael Hartl", :email=>"michael@example.com"} 
>> h1 == h2
=> true
{% endhighlight %}



The each method for arrays takes a block with only one variable, each for hashes takes two, a key and a value. 

	
{% highlight ruby %}
>> flash = { success: "It worked!", error: "It failed." }
=> {:success=>"It worked!", :error=>"It failed."}
>> flash.each do |key, value|
?>   puts "Key #{key.inspect} has value #{value.inspect}"
>> end
Key :success has value "It worked!"
Key :error has value "It failed."
{% endhighlight %}


### inspect


inspect returns a string with a literal representation of the object it’s called on.

	
{% highlight ruby %}
>> puts (1..5).to_a            # Put an array as a string.
1
2
3
4
5
>> puts (1..5).to_a.inspect    # Put a literal array.
[1, 2, 3, 4, 5]
>> p :name             # Same as 'puts :name.inspect'
:name
{% endhighlight %}


## notes

Parenthesese and curly braces on the final hash are optional.

{% highlight ruby %}
# Parentheses on function calls are optional.
stylesheet_link_tag("application", :media => "all")
stylesheet_link_tag "application", :media => "all"

# Curly braces on final hash arguments are optional.
stylesheet_link_tag "application", { :media => "all" }
stylesheet_link_tag "application", :media => "all"
{% endhighlight %}

	
{% endhighlight %} erb
# outputs
<link href="/assets/application.css" media="all" rel="stylesheet"
type="text/css" />
{% endhighlight %}

## Classes

### contructor
	
{% highlight ruby %}
>> s = "foobar" 

>> s = String.new("foobar") 

>> a = Array.new([1, 3, 2])

# Hash is different
>> h = Hash.new
=> {}
>> h[:foo]            # Try to access the value for the nonexistent key :foo.
=> nil
>> h = Hash.new(0)    # Arrange for nonexistent keys to return 0 instead of nil.
=> {}
>> h[:foo]
=> 0
{% endhighlight %}


Array constructor Array.new takes an initial value for the array, Hash.new takes a default value for the hash, which is the value of the hash for a nonexistent key.


### superclass
	
{% highlight ruby %}
>> s = String.new("foobar")
=> "foobar"
>> s.class                        # Find the class of s.
=> String
>> s.class.superclass             # Find the superclass of String.
=> Object
>> s.class.superclass.superclass  # Ruby 1.9 uses a new BasicObject base class
=> BasicObject 
>> s.class.superclass.superclass.superclass
=> nil
{% endhighlight %}


### Class inheritance
	
{% highlight ruby %}
>> class Word
>>   def palindrome?(string)
>>     string == string.reverse
>>   end
>> end
=> nil
>> w = Word.new              # Make a new Word object.
=> #<Word:0x22d0b20>
>> w.palindrome?("foobar")
=> false
>> w.palindrome?("level")
=> true


# Better solution
>> class Word < String             # Word inherits from String.
>>   # Returns true if the string is its own reverse.
>>   def palindrome?
>>     self == self.reverse        # self is the string itself.
>>   end
>> end
=> nil
>> s = Word.new("level")    # Make a new Word, initialized with "level".
=> "level"                  
>> s.palindrome?            # Words have the palindrome? method.
=> true                     
>> s.length                 # Words also inherit all the normal string methods.
=> 5
{% endhighlight %}



## Modifing built-in classes (temporarily)


	
{% highlight ruby %}
# This doesn't work
>> "level".palindrome?

# so modify String class
>> class String
>>   # Returns true if the string is its own reverse.
>>   def palindrome?
>>     self == self.reverse
>>   end
>> end
=> nil
>> "deified".palindrome?
=> true

# find it out in methods
>> "test".methods.sort
{% endhighlight %}

Once you get out of irb, then it is not available any more. It’s considered bad form to add methods to built-in classes without having a really good reason.

## rails' blank

Rails adds a blank? method to Ruby.

	
{% highlight ruby %}

{% endhighlight %}














