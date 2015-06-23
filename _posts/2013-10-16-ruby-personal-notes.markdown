---
layout: post
title: "Ruby personal notes"
date: 2013-10-16 08:41
comments: true
categories: Ruby
---

<!-- more -->

{:.no_toc}

* Will be replaced with the ToC, excluding the "Contents" header
{:toc}

## Altering Loop Control

Use next/redo to jump to the beginning. Use break to break out the program.
	
{% highlight ruby %}
cont = "y"
while (cont == "y")
	print ("Enter a numerator: ")
	num = Integer(gets)
	print("Enter a denominator: ")
	denom = Integer(gets)
	if denom == 0 then
		next 
		# or 'redo'
		# or 'break' to exit
	end
	puts(num / denom)
	print ("More? (y/n) ")
	cont = gets
	cont = cont.chomp
end
{% endhighlight %}

## Using if statement in while statement
	
{% highlight ruby %}
answer = "Watson"
tries = 0
while tries < 3
	print("What is the name of the computer that played on Jeopardy? ")
	response = gets
	response = response.chomp
	tries += 1
	if response == "Watson"
		puts ("That's right!")
		exit
	elsif tries == 3
		puts ("Sorry. The answer is Watson.")
		exit
	else
		puts ("Sorry. Try again")
	end
end
{% endhighlight %}

## Split a string

{% highlight ruby %}
" now's  the time".split        #=> ["now's", "the", "time"]
" now's  the time".split(' ')   #=> ["now's", "the", "time"]
# split by word
" now's  the time".split(/ /)   #=> ["", "now's", "", "the", "time"]

"1, 2.34,56, 7".split(%r{,\s*}) #=> ["1", "2.34", "56", "7"]
# split by letter
"hello".split(//)               #=> ["h", "e", "l", "l", "o"]

"hello".split(//, 3)            #=> ["h", "e", "llo"]
"hi mom".split(%r{\s*})         #=> ["h", "i", "m", "o", "m"]
{% endhighlight %}


## Block
1. A block need to associate with a method call.
2. A block can be between do-end with a method call.
3. A block can have parameters between pipes like | param |
4. A method can have parameters with block as well
5. each method can take a block

{% highlight ruby %}
# 1.
# a block do nothing
# use {} for a single line
{ puts "Echo"}

# associate with a method call
3.times { puts "Echo"}

# 2. Use do-end for multiple lines
10.times do
	puts "situp"
end

# 3. (count from 0-9)
10.times do |number|
	puts "#{number} situp"
end

# 4. (count from 1-10)
1 upto(10) do |number|
	puts "#{number} situp"
end

# 5.
movies. each do |movie|
	movie.thumbs_up
	puts movie
end
{% endhighlight %}

## Select 
select takes a block. All are the same in the followings.

	
{% highlight ruby %}
numbers.select do |number| 
	number>5
end

numbers.select{ |number| number>5 }

numbers.select{ |n| n>5}
{% endhighlight %}

## Rspec

movie_spec.rb
{% highlight ruby %}
require_relative 'movie'

describe Movie do
	before do
		@initial_rank = 10
		@movie = Movie.new("goonies", @initial_rank)
	end

	it "has a capitalized title" do
		@movie.title.should == "Goonies"
	end

	it "has an initial rank" do
		@movie.rank.should == 10
	end

	it "has a string representation" do
		@movie.to_s.should == "Goonies has a rank of 10"
	end

	it "increases rank by 1 when given a thumbs up" do
		@movie.thumbs_up
		@movie.rank.should == @initial_rank + 1
	end

	it "decreases rank by 1 when given a thumbs down" do
		@movie.thumbs_down
		@movie.rank.should == @initial_rank - 1
	end

	context "created with a default rank" do
		before do
		@movie = Movie.new("goonies")
		end

		it "has a rank of 0" do
			@movie.rank.should == 0
		end 
	end
end
{% endhighlight %}


{% highlight text %}
$ rspec movie_spec.rb --color --format doc
{% endhighlight %}

In the terminal this will show the results with colors and detailed document.


Running all Rspec

{% highlight text %}
$ rspec . --color
{% endhighlight %}

## Exception handling

[Raising An Exception](http://rubylearning.com/satishtalim/ruby_exceptions.html)
The following is a basic since it will not deal the second error. If you enter 0 twice, it will throw an error.

{% highlight ruby %}
begin
  print("Enter numerator: ")
  num = Integer(gets)
  print("Enter denominator: ")
  denom = Integer(gets)
  ratio = num / denom
  print(ratio)
rescue
	print $! #print error
	puts
	print("Enter a denominator other than 0: ")
	denom = Integer(gets)
	ratio = num / denom
	print(ratio)
end
{% endhighlight %}
	
{% highlight ruby %}
begin
  print(3/0) # or print(3/1) to show the second error
  File.open("myfile.txt")
#rescue ZeroDivisionError
#  print("tried to deivide by 0")
# or
rescue ZeroDivisionError => oops
  print(oops)
rescue SystemCallError
  print("file not found")
end
{% endhighlight %}

raise "exception raised" exit the program.
	
{% highlight ruby %}
print("In program")
raise "exception raised"
print("Back in program")
{% endhighlight %}

{% highlight ruby %}
# ex 1
def inverse(x)  
  raise ArgumentError, 'Argument is not numeric' unless x.is_a? Numeric  
  1.0 / x  
end  
puts inverse(2)  
puts inverse('not a number')  


# ex 2
def ctof(temp)
  raise ArgumentError, "argument is not numeric" unless temp.is_a? Numeric
  return (9.0/5.0) * temp + 32.0
end

puts ctof(4)
puts ctof('not a number')

# ex 3
def ctof(temp)
  raise ArgumentError, "argument is not numeric" unless temp.is_a? Numeric
  return (9.0/5.0) * temp + 32.0
end

begin
  print("Enter a temperature to convert: ")
  t = Integer(gets)
  print(ctof(t))
rescue
  print("Argument was not a number")
end
{% endhighlight %}

### Catch and throw
	
{% highlight ruby %}
alist = [6,1,10,5,9,3,8,4,7,2]
passnum = alist.length-1
catch (:stop) do # catch here
  while passnum > 0
    for i in 0..passnum-1
      print(alist[i])
      if i > 6
        throw :stop # throwing here
      end
    end
  passnum - 1
  end
end
{% endhighlight %}

Exercise for exception. list.rb

{% highlight ruby %}
begin
  print("Name of file? :")
  name = gets
  myFile = File.open(name.chomp)
  lines = myFile.read
  puts lines
rescue
  print("File not found. Enter another name.")
  retry # this will take back to 'begin'.
end
{% endhighlight %}

Create a file called text.txt.
	
{% highlight text %}
$ touch text.txt
$ vim text.txt
line 1
line 2
line 3
line 4
:wq!
$ ruby list.rb
{% endhighlight %}


## IO
### Reading a file
readfile.rb
	
{% highlight ruby %}
while line = gets
  puts line
end
{% endhighlight %}
	
{% highlight text %}
# to read a file
ruby readfile.rb data.txt
{% endhighlight %}

Without writing a file name. 

{% highlight ruby %}
File.open("data.txt") do |file|
  while line = file.gets
    puts line
  end
end
{% endhighlight %}
	
{% highlight text %}
$ ruby readfile.rb
{% endhighlight %}

### Writing a file
writefile.rb
	
{% highlight ruby %}
outfile = File.new("myfile.txt","w") # w is for writing
outfile.print("Hello, world\n")
outfile.print("Goodbye, world!")
outfile.close
{% endhighlight %}

### network

Simple approach for tcp protocl.

client.rb socket

{% highlight ruby %}
require 'socket' # socket library for tcp 

host = 'localhost'
port = 1500

sock = TCPSocket.open(host, port) # open the socket with host and port number.

while line = sock.gets
  puts line.chop
end

sock.close
{% endhighlight %}

create a server file, server.rb
	
{% highlight ruby %}
require 'socket'

server = TCPServer.open(1500) # open the port
loop {
	client = server.accept
	client.puts("Hello, client!")
	client.puts("Goodby, client!")
	client.puts("closing connection...")
	client.close
}
{% endhighlight %}

{% highlight text %}
$ ruby server.rb #first run the server
# open a tab and run a client
$ ruby client.rb
# This will display the content from the server.rb
{% endhighlight %}
 
### Reading and writing a data file

Create a text file, temps.txt and add data.

40
38
41
39
43
56

Create a file, tempavg.rb
	
{% highlight ruby %}
count = 0
total = 0
while temp = gets
  total += Integer(temp)
  count += 1
end
puts total / count
{% endhighlight %}

Create addtmps.rb
	
{% highlight ruby %}
tempfile = File.open("temps.txt","a+") # append to the file
day = 1
while day < 8
  print("Enter temperature: ")
  temp = gets
  tempfile.puts(temp)
  day += 1
end
tempfile.close
{% endhighlight %}

Run addtemp.rb
	
{% highlight text %}
$ ruby addtemps.rb
# add number one by one
# check it by open it.
$ cat temps.txt
$ ruby tempavg.rb temps.txt

{% endhighlight %}

## Ruby debugger

[Ruby debugger](http://www.tutorialspoint.com/ruby/ruby_debugger.htm)

debug1.rb
	
{% highlight ruby %}
nums = [1,2,3,4,5]
for i in nums
  print (i, i*i)
end
{% endhighlight %}

Debug can be done by setting require switch.
{% highlight text %}
$ ruby -r debug debug1.rb # -r means require switch
# This will show the first line. 
# By typing l and enter you can list with the whole programe with number lines.
# n and enter shows line by line. And this will show the output in front of debug.
{% endhighlight %}

debug2.rb
	
{% highlight ruby %}
nums = [5,2,8,3,1,6]
min = 5
for i in nums
  if i > min # intentional bug. this is supposed to be if i < min
    min = i
  end
end
print("The minimum value is ", min)
{% endhighlight %}

Start a debugger.
	
{% highlight text %}
$ ruby -r debug debug2.rb
$ n # to show the next line.
$ disp min # this will display min value
# each time when you disp min, it will add 1,2 etc and display min.
# You just need to do it once.
# by using b 6, it will set a break at 6
{% endhighlight %}

## Setting a watchpoint

debug3.rb
	
{% highlight ruby %}
# fibonacci sequence
t1 = 0
t2 = 1
n = 10
print(0, " ", 1, " ")
while n > 0
  t = t1 + t2
  print(t, " ")
  temp = t2
  t2 = t
  t1 = temp
  n -= 1
end
{% endhighlight %}

watchpoint to watch an expression become true.

{% highlight text %}
$ ruby -r debug debug3.rb
$ watch n ==1
Set watchpoint 1:n==1 
# program will run until this watchpoint becomes true.
$ c # c to continue
0 1 1 2 3 5 8 13 21 34 55 Watchpoint 1, toplevel at debug3.rb:7
# the program stopped when n==1 and displayed.

$ ruby -r debug debug3.rb
# run for a few times by n
# then type var local
$ var local # to display all variable at that time
# run it again by typing n
$ v l # same as var local
$ quit # to quit debug
{% endhighlight %}

### debugging exercise

max.rb to practice debugging.
	
{% highlight ruby %}
# this will create an infinite loop. It is not increasing pos += 1
nums = [1,3,4,6,2,5]
max = 0
pos = 1
while pos <= nums.length # supposed to be while pos < nums.length
  if nums[pos] > nums[max]
    max = pos
  end
  # pos += 1 # through debugging you can uncomment this.
end
print(nums[max])
{% endhighlight %}

## Graphical use interface Tk

tkexample.rb

{% highlight ruby %}
require 'tk'

root = TkRoot.new(title "First Example")
TkLabel.new(root) do
  text 'A sample GUI using Tk'
  pack { padx 15; pady 15; side 'left'}
end
Tk.mainloop

{% endhighlight %}


## How to install gtk2

I had a problem to install gtk2. This is what I had to do.

	
{% highlight text %}
cd `brew --prefix`
git fetch origin
git reset --hard origin/master
brew update
brew install glib
brew install gtk+
sudo gem install gtk2
{% endhighlight %}










