---
layout: post
title: "Personal Notes on Learning Ruby 3"
date: 2013-07-10 17:00
categories: Ruby
---


## Classes
- class
- is_a
- new
- State and Behaviour

	
{% highlight ruby %}
puts 1.class
puts "".class
puts [].class

#=> Fixnum
#=> String
#=> Array


puts 1.is_a?(Integer) 	# true
puts 1.is_a?(String) 	# false

1.class.class 			# Class

# Create an instance of Object
Object.new
{% endhighlight %}



All classes shall have names that begin with a capital letter.

	
{% highlight ruby %}
class Rectangle
end
{% endhighlight %}

### State and Behaviour
	
{% highlight ruby %}
class Rectangle
  def perimeter
  end
end

class Rectangle
  def perimeter
    2 * (@length + @breadth)
  end
end
{% endhighlight %}

Ruby follows a two-space indentation convention and sections of code are typically closed using the keyword end.

@ symbol placed in front of variable is a convention which designates them as being a part of the state of the class, or they are the "instance variables of the class." This means that every instance of the class Rectangle will have its own unique copies of these variables and is in effect, a distinct rectangle.

{% highlight ruby %}
class Rectangle
  def initialize(length, breadth)
    @length = length
    @breadth = breadth
  end

  def perimeter
    2 * (@length + @breadth)
  end

  def area
    @length * @breadth
  end
end


n = Rectangle.new(2,5)
n.perimeter 				#=> 14
n.area 						#=> 10

Rectangle.new(4,6).area 	#=> 24
{% endhighlight %}


## Arraying arguments

- splat operator
	
{% highlight ruby %}
# with list
def add(*numbers)
  numbers.inject(0) { |sum, number| sum + number }
end

puts add(1)
puts add(1, 2)
puts add(1, 2, 3)
puts add(1, 2, 3, 4)

# splat an array
def add(a_number, another_number, yet_another_number)
  a_number + another_number + yet_another_number
end

numbers_to_add = [1, 2, 3] # Without a splat, this is just one parameter
puts add(*numbers_to_add)  # Try removing the splat just to see what happens


{% endhighlight %}

[Ruby Inject](http://blog.jayfields.com/2008/03/ruby-inject.html)


## Exercises
- abs
- round
- inject
- shift
- pop


Create a method called introduction that accepts a person's age, gender and any number of names, then returns a String 
	
{% highlight ruby %}
def introduction(age, gender, *names)
  name = names.join(" ")
  "Meet "+ name + ", who's "+ age.to_s + " and " + gender
end

# is the same as 
def introduction(age, gender, *names)
  "Meet #{names.join(' ')}, who's #{age} and #{gender}"
end

# Naming parameters
def add(a_number, another_number, options = {})
  sum = a_number + another_number
  sum = sum.abs if options[:absolute]
  sum = sum.round(options[:precision]) if options[:round]
  sum
end

puts add(1.0134, -5.568)
puts add(1.0134, -5.568, absolute: true)
puts add(1.0134, -5.568, absolute: true, round: true, precision: 2)

# add, subtract and calculate assignment
def add(*numbers)
	numbers.inject(0) { |sum, number| sum + number }  
end

def subtract(*numbers)
  sum = numbers.shift
  numbers.inject(sum) { |sum, number| sum - number }  
end

def calculate(*arguments)
  # if the last argument is a Hash, extract it 
  # otherwise create an empty Hash
  options = arguments[-1].is_a?(Hash) ? arguments.pop : {}
  options[:add] = true if options.empty?
  return add(*arguments) if options[:add]
  return subtract(*arguments) if options[:subtract]
end
{% endhighlight %}

	
{% highlight ruby %}
# shift: Removes the first element of self and returns it (shifting all other elements down by one).
args = [ "-m", "-q", "filename" ]
args.shift     #=> "-m"
args           #=> ["-q", "filename"]

# pop: Removes the last element from self and returns it
a = [ "a", "b", "c", "d" ]
a.pop     #=> "d"
a.pop(2)  #=> ["b", "c"]
a         #=> ["a"]
{% endhighlight %}

## Lambdas 

	
{% highlight ruby %}
l = lambda { "Do or do not" }
puts l.call 			#=> Do or do not

# lambda with parameter
l = lambda do |string|
  if string == "try"
    return "There's no such thing" 
  else
    return "Do or do not."
  end
end
puts l.call("try") # Feel free to experiment with this

# Increment 1
Increment = lambda { |number|  number + 1 } 
{% endhighlight %}

Use {} for single line lambdas and do..end for lambdas that are longer than a single line.

	
{% highlight ruby %}
[1,2,3,4,5].each{ |num| puts num**2}

# OR
[1,2,3,4,5­].each do |num|­
puts num**­2
end
{% endhighlight %}



[Notes from rubymond.com](http://rubymonk.com/learning/books/1-ruby-primer/)
