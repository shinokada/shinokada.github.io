---
layout: post
title: "Personal Notes on Learning Ruby 2"
date: 2013-07-09 09:28
categories: Ruby
---

## Methods
- even?
- next
- method
- sort
- between?
- length

{% highlight ruby %}
21.even?

1.next.next

1.methods.sort

['rock','paper','scissors'].index('rock')

3.between?(2,4)

words = ["foo", "bar", "baz"]
words[1]

words = ["foo", "bar", "baz"]
words.[](1)

"Ruby".length
{% endhighlight %}

## String

- include?
- start_with?
- end_with?
- index
		
{% highlight ruby %}
# String Interploation
def string_length_interpolater(incoming_string)
  "The string you just gave me has a length of #{incoming_string.length}"
end

# Search in a String
"[Luke:] I can’t believe it. [Yoda:] That is why you fail.".include? 'Yoda'

"Ruby is a beautiful language".start_with? "Ruby"

"I can't work with any other language but Ruby".end_with? 'Ruby'

"I am a Rubyist".index 'R'

{% endhighlight %}

## String case change

- upcase
- downcase
- swapcase

{% highlight ruby %}
puts 'i am in lowercase'.upcase #=> 'I AM IN LOWERCASE'

'This is Mixed CASE'.downcase

"ThiS iS A vErY ComPlEx SenTeNcE".swapcase

{% endhighlight %}

## Advanced String Operations

- split
- +
- <<
- sub
- gsub

		
{% highlight ruby %}
'Fear is the path to the dark side'.split(' ')
# ["Fear", "is", "the", "path", "to", "the", "dark", "side"]

# concatenating strings
'Ruby' + 'Monk'

"Ruby"<<"Monk"

{% endhighlight %}

String object 'Monk' will be appended to the object represented by 'Ruby' itself.
This can make a huge difference in your memory utilization.

	
{% highlight ruby %}
# Replacing the first substring 
"I should look into your problem when I get time".sub('I','We')


# gsub has a global scope
"I should look into your problem when I get time".gsub('I','We')

#  replaces all the vowels with the number 1
'RubyMonk'.gsub(/[aeiou]/,'1')

# replace all the characters in capital case with number '0' 
'RubyMonk Is Pretty Brilliant'.gsub(/[A-Z]/,'0')
{% endhighlight %}

## Finding a substring using RegEx
	
{% highlight ruby %}
'RubyMonk Is Pretty Brilliant'.match(/ ./)
# I. regex search a space and any character, '.'

'RubyMonk Is Pretty Brilliant'.match(/ ./, 9)
# P
{% endhighlight %}

The second parameter specifies the position in the string to begin the search.


## Sorting words

{% highlight ruby %}
def sort_string(string)
  string.split(" ").sort{|x, y| x.length <=> y.length}.join(" ")
end
{% endhighlight %}

## Finding the frequency

{% highlight ruby %}
def find_frequency(sentence, word)
  sentence.downcase.split.count(word.downcase)
end
{% endhighlight %}


## Loops 

{% highlight ruby %}
# loop and break if
loop do
  monk.meditate
  break if monk.nirvana?
end

# times
def ring(bell, n)
  n.times do
    bell.ring
  end
end      
{% endhighlight %}

## Array

- new array
- push
- <<
	
{% highlight ruby %}
# empty ruby
[]

Array.new

[1,2,3,4,5]
# is the same as
(1..5).to_a 

[1, 2, 3, 4, 5][2] # 3

[1, 2, 3, 4, 5][-5] # 1
[1, 2, 3, 4, 5][-2] # 4


[1, 2, 3, 4, 5]<<"woot"
# is the same as 
[1, 2, 3, 4, 5].push("woot")
{% endhighlight %}

## Basic Array Operations

### Transforming arrays

- map
- select
- delete_if
- length, size

{% highlight ruby %}
[1, 2, 3, 4, 5].map { |i| i + 1 }
# => [2, 3, 4, 5, 6]

[1,2,3,4,5].map { |i| i * 3 }
[3, 6, 9, 12, 15]

# filtering with select
# select even numbers
[1,2,3,4,5,6].select {|number| number % 2 == 0}

# => [2, 4, 6]

names = ['rock', 'paper', 'scissors', 'lizard', 'spock']
names.select {|word| word.length > 5}
# => ["scissors", "lizard"]

# delete element 5
[1,3,5,4,6,7].delete(5)

# delete_if
[1,2,3,4,5,6,7].delete_if{|i| i < 4 }

# delete all even numbers
[1,2,3,4,5,6,7,8,9].delete_if{ |i| i % 2 == 0}

# length and size are the same
[ 1, 2, 3, 4, 5 ].length   #=> 5
{% endhighlight %}

length, size: Returns the number of elements in self. May be zero.

## Iteration

- for
- each
			
{% highlight ruby %}
array = [1, 2, 3, 4, 5]
for i in array
  puts i
end

# Copy the values less than 4 in the array stored in the source variable into the array in the destination variable
def array_copy(source)
  destination = []
  for number in source
    # Add number to destination if number
    # is less than 4
    destination << number if number < 4
  end
  return destination
end

# prints all the values in an array
array = [1, 2, 3, 4, 5]
array.each do |i|
  puts i
end

# using each in stead of for
def array_copy(source)
  destination = []
  source.each do |number|
    # Add number to destination if number
    # is less than 4
    destination << number if number < 4
  end
  return destination
end
{% endhighlight %}

The each method takes two arguments - an element and a block. The element, contained within the pipes, is like a placeholder. Whatever you put in the pipes will be used in the block to represent each element of the array in turn. The block is the line of code that is executed on each of the array items, and is handed the element to process.

## Problem Number shuffle

Given a 3 or 4 digit number with distinct digits, return a sorted array of all the unique numbers than can be formed with those digits.
	
{% highlight ruby %}
def number_shuffle(number)
  no_of_combinations = number.to_s.size == 3 ? 6 : 24
  digits = number.to_s.split(//) #  '' is an empty String, whereas // is an empty Regexp:
  combinations = []
  combinations << digits.shuffle.join.to_i while combinations.uniq.size!=no_of_combinations
  combinations.uniq.sort
end
{% endhighlight %}


## Hash

[symbol vs string in key](http://stackoverflow.com/questions/8189416/why-use-symbols-as-hash-keys-in-ruby)

Hashes allow an alternate syntax form when your keys are always symbols. Instead of

[Read ruby-doc](http://www.ruby-doc.org/core-2.0/Hash.html)

{% highlight ruby %}
options = { :font_size => 10, :font_family => "Arial" }
{% endhighlight %}

You could write it as:
	
{% highlight ruby %}
options = { font_size: 10, font_family: "Arial" }
{% endhighlight %}

A Hash can also be created through its ::new method:
	
{% highlight ruby %}
grades = Hash.new
grades["Dorothy Doe"] = 9
{% endhighlight %}



Using symbols not only saves time when doing comparisons, but also saves memory, because they are only stored once.


{% highlight ruby %}
restaurant_menu = {
  "Ramen" => 3,
  "Dal Makhani" => 4,
  "Tea" => 2
  }
  
restaurant_menu["Ramen"] #=> 3
{% endhighlight %}

## Modifing a hash

{% highlight ruby %}
restaurant_menu = {}
# set the values for each item separately here:
restaurant_menu["Dal Makhani"] =4.5
restaurant_menu["Tea"] =  2
{% endhighlight %}


### Iterating a hash with each

- each
- keys
- values

Iterate over a Hash using each, it passes two values to the block: the key and the value of each element.

If no default is set nil is used. You can set the default value by sending it as an argument to ::new:

{% highlight ruby %}
# default value
grades = Hash.new(0)

# Or by using the default= method:
grades = {"Timmy Doe" => 8}
grades.default = 0

# Accessing a value in a Hash requires using its key:
puts grades["Jane Doe"] # => 10

# each in a hash
restaurant_menu = { "Ramen" => 3, "Dal Makhani" => 4, "Coffee" => 2 }
restaurant_menu.each do | item, price |
  puts "#{item}: $#{price}"
end

# the above is the same as the following
restaurant_menu = { "Ramen" => 3, "Dal Makhani" => 4, "Coffee" => 2 }
restaurant_menu.each { |item, price| puts "#{item}: $#{price}"}

# Increase the price of all the items in the restaurant_menu by 10%.
restaurant_menu = { "Ramen" => 3, "Dal Makhani" => 4, "Coffee" => 2 }
restaurant_menu.each do |item, price|
  restaurant_menu[item] = price + (price * 0.1)
end



# keys
restaurant_menu = { "Ramen" => 3, "Dal Makhani" => 4, "Coffee" => 2 }
restaurant_menu.keys

# Creating hash in different ways
normal = Hash.new
was_not_there = normal[:zig]
puts "Wasn't there:"
p was_not_there

# create a default value
usually_brown = Hash.new("brown")
pretending_to_be_there = usually_brown[:zig] # :zig can be anything
puts "Pretending to be there:"
p pretending_to_be_there

#=> Wasn't there:
#=> nil
#=> Pretending to be there:
#=> "brown"

chuck_norris = Hash[:punch, 99, :kick, 98, :stops_bullets_with_hands, true]
p chuck_norris

#=> {:punch=>99, :kick=>98, :stops_bullets_with_hands=>true}

def artax
  a = [:punch, 0]
  b = [:kick, 72]
  c = [:stops_bullets_with_hands, false]
  key_value_pairs = [a, b, c]
  Hash[key_value_pairs]
end
p artax

#=> {:punch=>0, :kick=>72, :stops_bullets_with_hands=>false}
{% endhighlight %}



## Other notes

- Puts adds a newline to the end of the output. Print does not.
- p "hi" is the same as  "hi".inspect or puts "hi".inspect

  
{% highlight ruby %}
>> p :name             # Same as 'puts :name.inspect'
:name
{% endhighlight %}


[Notes from rubymond.com](http://rubymonk.com/learning/books/1-ruby-primer/)
