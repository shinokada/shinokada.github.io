---
layout: post
title: "PHP Basics"
date: 2013-05-23 21:04
comments: true
categories: [PHP]
---

<!-- more -->


## About PHP

### Difference between PHP and HTML

HTML isn’t processed until it gets to the browser, but the PHP is executed on the server, and its output(typically HTML or some other text) replaces the PHP code.

PHP is a server-side language. None of your PHP ever hits the browser — it’s processed on the server.

### What you can do with PHP
- You’ll be able to change values on your site based on user
input or other values (e.g. change the greeting based on the
time of day.)
- You’ll be able to use the information that a user enters into a form, maybe by giving them appropriate content based on that info (think search results) or by storing that information within a database.

- You’ll be able to let your users upload files to your server.
- You’ll be able to build pages “on the fly” by combining templates
with content from a database, all right as the viewer requests that specific page.
- And more and more…

## Installing PHP

### XAMPP or MAMP

[MAMP](http://www.mamp.info/en/index.html)

[XAMPP](http://www.apachefriends.org/en/xampp.html)

### Another tools

- [firebug](http://getfirebug.com/)
- [Sublime Text 2](http://www.sublimetext.com/)

### How to use php in terminal

	php -a //hit enter


## The Example Files and codes

### PHP files

- Open htdocs folder
- Create a folder called example

Create a file index.php

	<?php
		echo "The code will go here";
	?>

localhost:8888/examples/index.php on your browser.

### PHP Syntax

#### PHP Tags

	<?php
		//your php code goes here
	?>

#### PHP and HTML

	<html>
		<title>my php page</title>
		<body>
		<?php
			echo "hello there!";
		?>
		</body>
	</html>

#### Doing Cool Stuff with PHP and HTML

	<h1> Hello PHP! </h1>
	<?php
	echo "<p>I'm getting good with PHP.</p>";//you need ; for each statement
	?>
	<?php
	echo "<b>hello there!</b>";
	?>

	<?php
		echo "<font color='green'>hello there!</font>";
	?>
	
	<?php
	echo "today is ".date('Y-m-d');
	?>

	

### Variables

	$message = "<p>I'm getting good with PHP.</p>";
	echo $message;


## Basic Values/Datatypes

### Strings

Two quotes and sigle quotes

	<?php
	$str = "It's a nice day today."
	echo $str;
	?>
	<?php
	$str = 'This is a PHP string examples in single quotes';
	echo $str;
	?>
Using a single quote.
	<?php
		echo "That's how you do it. 'Gotcha,' you reply.";
	?>

#### Escaping Special Characters

Escaping a quote
	<?php
		$str = "\"This is a PHP string examples in double quotes\"";
		echo $str;
	?>

Escaping an apostrophe

	<?php
	$str = 'It\'s a nice day today.';
	echo $str;
	?>
you can put variables right inside the string	

	$name = "Sherlock";
	echo "Hello, $name.";
	

#### Concatenating Strings in PHP

	
	<?php
	$str1 = "I Love PHP.";
	$str2 = "PHP is fun to learn.";
	echo $str1." ".$str2;
	?>	

### Useful PHP String Functions

#### Example - strlen() Function

	<?php
	$str = "Hello!";
	echo strlen($str);// 6
	?>

#### Example - str_replace() Function

	<?php
	$str = "Hello! How are you today?";
	echo str_replace("Hello", "Hi", $str);//Hi! How are you today?
	?>

#### Example - strtoupper() Function

	<?php
	$str = "hello!";
	echo strtoupper($str);//HELLO!
	?>

#### Example - ucfirst() Function

	<?php
	$str = "hello!";
	echo ucfirst($str);//Hello!
	?>

#### Example - trim() Function

	<?php
	$str = " hello! ";
	echo trim($str);//"hello!
	?>


### Numbers 

	1001.234
	-10
	1.234e5	//123400
	echo 1.23e4 //12300

### Booleans
not case-sensitive true/TRUE and false/FALSE.	

	// return false
	"" (empty string)
	0
	false (of course)
	null
	array() (an empty array)
	

### Null

caseinsensitive null/NULL

### Array

#### numeric arrays

	$an_array = array("HTML", "CSS", "JavaScript", "PHP");

By default, arrays use numeric indices; also by default, the indices
start at 0, not 1. So, based on that array up there, 

	$an_array[0] holds the value HTML
	$an_array[2] is JavaScript.

#### Associative array using key => value

PHP arrays are not confined to one data type per array.
	$person = array(
		"name" => "Sherlock Holmes",
		"birthdate" => "January 6, 1854",
		"married" => false,
		"interests" => array("reading", "chemistry", "crime",
		"violin")
	);
	$person["name"] will be "Sherlock Holmes"
	

#### Assigning value to an array

	$an_array[4] = "SQL";
	$person["best friend"] = "John Watson";

## Comments

	# this is a comment
	// this is also a comment	
	/* this is
	a multiline
	comment */
	$name = "Sherlock"; # You can add comments here.
	$name = "Sherlock"; // You can add comments here as well.
	

## Operators

### Arithmetic Operators

	$num = 10;
	$num = $num + 10; # Addition
	$num = $num - 5; # Subtraction
	$num = $num / 2; # Division
	$num = $num * 0.2; # Multiplication
	echo $num; # outputs 1.5
We’re redefining the value of the variable $num every time

#### Modulus

It returns the remainder. The modulus operator is a great way to find out if a number is odd or even.
	//modulus %		
	5 % 3; # 2
	
### The string operator

	
	"first string " . "second string";// . concatenate strings

### Assignment Operators

	$num = 10;
	$num += 10; # Addition $num = $num + 10
	$num -= 5; # Subtraction
	$num /= 2; # Division
	$num *= 0.2; # Multiplication
	echo $num; # outputs 1.5

### Incrementing / Decrementing Operators

	// all are the same
	$num = $num + 1;
	$num += 1;
	$num++;
	
	$num=5;
	echo $num++;//5
	num=5;
	echo ++$num ;// 6
	$num=5;echo $num-- ;//5
	$num=5;echo --$num ; //4
	

### Comparison Operators

#### 	doubleequals ( == ) and triple-equals ( === )

double-equals (also called the equal operator) tries to convert both values to the same type before comparing.


{% codeblock doubleequals ( == ) and triple-equals ( === ) lang:php %}
1 == "1" // true
1 === "1" // false
10 != "10"; # false, because it converts the string to a number
10 !== "10"; # true
5 > 10; # false
5 < 10; # true
5 >= 10; # true
4 <= 4; # true
$name = "Sherlock";
var_dump(!$name)// bool(false)
$married = false;
var_dump(!$married);// true
{% endcodeblock %}

### 	Logical Operators	


{% codeblock Logical Operators lang:php %}
//&& AND
var_dump(6 > 5 && 1 < 7); //true
// || OR
var_dump(2 > 5 || 1 < 7);//true
{% endcodeblock %}




## PHP If Else Statement

{% codeblock if()…else… for one line code inbetween lang:php %}
<html>
<body>

<?php
//Give what day of the week it is. Returns Sunday through Saturday.
$day = date("l");

if ($day == "Saturday")
	echo "It's party time :)";
else
	echo "Ahhh! I hate work days.";
?>

</body>
</html>
{% endcodeblock %}


{% codeblock if( ){ }else{ } for more than one line inbetween lang:php %}
<html>
<body>
<?php
//Give what day of the week it is. Returns Sunday through Saturday.
$day = date("l");

if ($day == "Saturday")
{
	echo "It's party time :)";
	echo " Where are you going this evening?";
}
else
{
	echo "Ahhh! I hate work days.";
	echo " I want weekend to come :)";
}
?>

</body>
</html>
{% endcodeblock %}

	
	

### The Elseif Statement in PHP


{% codeblock Elseif Statement lang:php %}
<html>
<body>
<?php
//Give what day of the week it is. Returns Sunday through Saturday.
$day = date("l");

if ($day == "Saturday")
{
	echo "It's party time :)";
	echo " Where are you going this evening?";
}
elseif ($day == "Friday")
{
	echo "Have a nice day!";
}
else
{
	echo "Ahhh! I hate work days.";
	echo " I want weekend to come :)";
}
?>
</body>
</html>	
{% endcodeblock %}



## PHP Project Resources

[Making a Really Cool jQuery Gallery](http://tutorialzine.com/2009/09/cool-jquery-gallery/)

[A simple AJAX website with jQuery](http://tutorialzine.com/2009/09/simple-ajax-website-jquery/)
	
