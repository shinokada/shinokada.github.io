---
layout: post
title: "Personal Notes on Learning Rails"
date: 2013-07-05 20:47
categories: Rails
---

Install RubyGems

{% highlight text %}
which gem

gem update --system x.x.x

# Creating a gem configuration file.
$ subl ~/.gemrc

# Suppressing the ri and rdoc documentation in .gemrc.
install: --no-rdoc --no-ri
update:  --no-rdoc --no-ri

$ gem install rails --version 4.0.0

$ rails -v
{% endhighlight %}



## The first application

  
{% highlight text %}
$ mkdir rails_projects
$ cd rails_projects
$ rails new first_app
{% endhighlight %}



## Bundler and Gemfile

  
{% highlight text %}

$ cd first_app/
$ subl Gemfile

# Gemfile
source 'https://rubygems.org'
ruby '2.0.0'

gem 'rails', '4.0.0'

group :development do
  gem 'sqlite3', '1.3.7'
end

gem 'sass-rails', '4.0.0'
gem 'uglifier', '2.1.1'
gem 'coffee-rails', '4.0.0'
gem 'jquery-rails', '2.2.1'
gem 'turbolinks', '1.1.1'
gem 'jbuilder', '1.0.2'

group :doc do
  gem 'sdoc', '0.3.20', require: false
end

group :production do
  gem 'pg', '0.15.1'
end

$ bundle install --without production
$ bundle update
$ bundle install

#rails server
$ rails server
{% endhighlight %}



## git init, gitignore, add and commit

  
{% highlight text %}

# creaing an alias
git config --global alias.co checkout

git init

# change .gitignore
# Ignore bundler config.
/.bundle

# Ignore the default SQLite database.
/db/*.sqlite3
/db/*.sqlite3-journal

# Ignore all logfiles and tempfiles.
/log/*.log
/tmp

# Ignore other unneeded files.
doc/
*.swp
*~
.project
.DS_Store
.idea

git add .

git commit -m "Initial commit"

git log

q # to quit

{% endhighlight %}


## Undoing by checkout

  
{% highlight text %}

# you do something wrong
$ ls app/controllers/
application_controller.rb
$ rm -rf app/controllers/
$ ls app/controllers/
ls: app/controllers/: No such file or directory

# the changes ae only on the working tree. The changes are not commited yet.
git status 

# Undo the chnages with -f flag to force overwriting
git checkout -f 
git status
{% endhighlight %}

## GitHub
  
{% highlight text %}
# adding to github
# first create a repo in github

git remote add origin https//github.com/<username>/first_app.git
git push -u origin master
{% endhighlight %}



## Branch, edit, commit, merge

  
{% highlight text %}
# Branch
# create a branch called modify-README
git checkout -b modify-README

# Switched to a new branch 'modify-README'
$ git branch
master
* modify-README

# Edit
git mv README.rdoc README.md
subl README.md

# Commit
git status
git commit -a -m "Improve the README file"

# Merge
# Switched to branch 'master'
git checkout master

git merge modify-README

# delete the brance if you want
git branch -d modify-README


# For illustration only; don't do this unless you mess up a branch
$ git checkout -b topic-branch
$ <really screw up the branch>
$ git add .
$ git commit -a -m "Major screw up"
$ git checkout master
# to abandon your topic branch changes, in this case with git branch -D:
# -D flag will delete the branch even though we haven’t merged in the changes.
$ git branch -D topic-branch

#Push
git push
{% endhighlight %}


## Heroku setup

  
{% highlight text %}
# Heroku uses PostgreSQL
# in Gemfile add this
group :production do
  gem 'pg', '0.15.1'
  gem 'rails_12factor', '0.0.2'
end

bundle install --without production
{% endhighlight %}

rails_12factor gem is to serve static assets such as images and stylesheets

In order to install it,

{% highlight text %}
$ bundle install --without production

$ git commit -a -m "Update Gemfile.lock for Heroku"

#  To create the files Heroku needs to serve static assets like images and CSS
$ rake assets:precompile
$ git commit -a -m "Add precompiled assets for Heroku"
{% endhighlight %}

--without production prevents installing production gems.

{% highlight text %}
# if heroku is not installed yet run the following.
$ gem install heroku

# to create a heroku stack
$ heroku create --stack cedar

# to login 
$ heroku login
$ cd /Desktop/rails_projects/first_app
$ heroku create
Created http://stormy-cloud-5881.herokuapp.com/ |
git@heroku.com:stormy-cloud-5881.herokuapp.com
Git remote heroku added

# Heroku deployment
$ git push heroku master

# Open it in a browser
$ heroku open
Opening stormy-headland-3006... done
# The resulting page is an error at the moment.
{% endhighlight %}

## Heroku commands

{% highlight text %}
# if you need to rename
$ heroku rename new-name
{% endhighlight %}

## [Chapter 2 A demo app](http://ruby.railstutorial.org/chapters/a-demo-app#top)

## The Users resource

Rails scaffolding is generated by passing the scaffold command to the rails generate script. The argument of the scaffold command is the singular version of the resource name (in this case, User), together with optional parameters for the data model’s attributes.

  
{% highlight text %}
# create a new project 
$ cd /Desktop/rails_projects
$ rails new demo_app
$ cd demo_app
{% endhighlight %}

Open the Gemfile and change it to the followings. 

{% highlight text %}
source 'https://rubygems.org'
ruby '2.0.0'
#ruby-gemset=railstutorial_rails_4_0

gem 'rails', '4.0.0'

group :development do
  gem 'sqlite3', '1.3.7'
end

gem 'sass-rails', '4.0.0'
gem 'uglifier', '2.1.1'
gem 'coffee-rails', '4.0.0'
gem 'jquery-rails', '3.0.4'
gem 'turbolinks', '1.1.1'
gem 'jbuilder', '1.0.2'

group :doc do
  gem 'sdoc', '0.3.20', require: false
end

group :production do
  gem 'pg', '0.15.1'
  gem 'rails_12factor', '0.0.2'
end

{% endhighlight %}

Install these without production

{% highlight text %}
$ bundle install --without production
$ bundle update
$ bundle install
{% endhighlight %}

Using git create a new github repository of demo_app.

{% highlight text %}
$ git init
# Add .DS_Store to .gitignore for Mac users.

$ add .
$ git commit -m "Initial commit"
$ git remote add origin http://github.com/shinokada/demo_app.git
$ git push -u origin master
{% endhighlight %}



{% highlight text %}
$ rails generate scaffold User name:string email:string

# Migrate DB, This simply updates the database with our new users data model. 
# This simply updates the database with our new users data model.
# You may be able to use rake command
$ bundle exec rake db:migrate

# check it on a server http://localhost:3000/, s stands for server.
$ rails s

# To see all the Rake tasks available, run
$ bundle exec rake -T
# for rake db
$ bundle exec rake -T db
{% endhighlight %}

Check out 

* http://localhost:3000/users
* http://localhost:3000/users/new
* http://localhost:3000/users/2
* http://localhost:3000/users/2/edit

Now check config/routes.rb. resources :users is the key code which coresponds url and source codes. 

{% highlight ruby %}
resources :users
{% endhighlight %}

Check the source codes. Open app/controllers/users_controller.rb. 

{% highlight ruby %}
class UsersController < ApplicationController

  def index
    @users = User.all
  end


end
{% endhighlight %}

Open app/views/users/index.html.erb to see the index view.

{% highlight erb %}
<% @users.each do |user| %>
...
  <tr>
    <td><%= user.name %></td>
    <td><%= user.email %></td>
    <td><%= link_to 'Show', user %></td>
    <td><%= link_to 'Edit', edit_user_path(user) %></td>
    <td><%= link_to 'Destroy', user, method: :delete,
                                     data: { confirm: 'Are you sure?' } %></td>
  </tr>
<% end %>
...
{% endhighlight %}

And check app/models/user.rb for it's model.

{% highlight ruby %}
class User < ActiveRecord::Base
end
{% endhighlight %}

@users get from the user model (user.rb) with all method.

## Microposts scaffold

{% highlight text %}
$ rails generate scaffold Micropost content:string user_id:integer

$ bundle exec rake db:migrate
{% endhighlight %}

Check app/config/routes.rb to see a new resource.

{% highlight ruby %}
DemoApp::Application.routes.draw do
  resources :microposts
  resources :users
  ...
end
{% endhighlight %}


## Validation

  
{% highlight ruby %}
class Micropost < ActiveRecord::Base
  validates :content, length: { maximum: 140 }
end
{% endhighlight %}

Try to add more than 140 characters at http://localhost:3000/microposts/new to see the validation at work.

## A user has_many microposts
  
{% highlight ruby %}
# app/models/user.rb
class User < ActiveRecord::Base
  has_many :microposts
end

# app/models/micropost.rb
class Micropost < ActiveRecord::Base
  belongs_to :user
  validates :content, length: { maximum: 140 }
end
{% endhighlight %}

Invoking rails console.
 
{% highlight text %}
# terminal
$ rails console
>> first_user = User.first
>> first_user # to display first_user
>> first_user.name
>> first_user.email
>> first_user.microposts # to display all microposts by first_user

>> User.all # to display all users

$ exit # or ctrl+d
{% endhighlight %}

## Inheritance hierarchies
User model and the Micropost model inherit (via the left angle bracket <) from ActiveRecord::Base which is the base class for models provided by ActiveRecord.

The User class, with inheritance. 
app/models/user.rb

{% highlight ruby %}
class User < ActiveRecord::Base
  .
  .
  .
end
{% endhighlight %}

The Micropost class, with inheritance. 
app/models/micropost.rb

{% highlight ruby %}
class Micropost < ActiveRecord::Base
  .
  .
  .
end
{% endhighlight %}

The Users controller and the Microposts controller inherit from the Application controller.

The UsersController class, with inheritance. 
app/controllers/users_controller.rb

{% highlight ruby %}
class UsersController < ApplicationController
  .
  .
  .
end
{% endhighlight %}

The MicropostsController class, with inheritance. 
app/controllers/microposts_controller.rb

{% highlight ruby %}
class MicropostsController < ApplicationController
  .
  .
  .
end
{% endhighlight %}

The ApplicationController class, with inheritance. 
app/controllers/application_controller.rb

{% highlight ruby %}
class ApplicationController < ActionController::Base
  .
  .
  .
end
{% endhighlight %}

## Deploying the demo app
Add to git and push it.

{% highlight text %}
# git
$ git add .
$ git commit -m "Finish demo app"
$ git push

$ heroku create
$ rake assets:precompile
$ git commit -a -m "Add precompiled assets for Heroku"
$ git push heroku master

# To get the application’s database to work, you’ll also have to migrate the production database
$ heroku run rake db:migrate

$ heroku logs
{% endhighlight %}

Check the Heroku site on a browser. Going http://lit-sierra-2020.herokuapp.com/users or http://lit-sierra-2020.herokuapp.com/microposts. If it gives an error, check it with heroku logs. 

====== Old notes =====

## Serving static assets, images and CSS
  
{% highlight ruby %}
DemoApp::Application.configure do
  ...
  config.serve_static_assets = true
  ...
end
{% endhighlight %}

  
{% highlight text %}
$ git commit -a -m "Serve static assets"
$ heroku create
$ git push heroku master

# migrate the production database 
$ heroku run rake db:migrate
{% endhighlight %}
====== end =========


## Sample_app [Lesson 3](http://ruby.railstutorial.org/chapters/static-pages#top)

{% highlight text %}
$ cd ~/rails_projects
# Not using the default test suit, using an alternate testing framework called RSpec to write a thorough test suite.
$ rails new sample_app --skip-test-unit
$ cd sample_app
{% endhighlight %}
  
{% highlight ruby %}
source 'https://rubygems.org'
ruby '2.0.0'

gem 'rails', '4.0.0'
gem 'bootstrap-sass', '2.3.2.0'
gem 'bcrypt-ruby', '3.0.1'
gem 'faker', '1.1.2'
gem 'will_paginate', '3.0.4'
gem 'bootstrap-will_paginate', '0.0.9'

group :development, :test do
  gem 'sqlite3', '1.3.7'
  gem 'rspec-rails', '2.13.1'
  gem 'guard-rspec', '2.5.0'
  # The following optional lines are part of the advanced setup.
  gem 'spork-rails', github: 'railstutorial/spork-rails'
  gem 'guard-spork', '1.5.0'
  gem 'childprocess', '0.3.6'
end

group :test do
  gem 'selenium-webdriver', '2.35.1'
  gem 'capybara', '2.1.0'

  # Uncomment this line on OS X.
  gem 'growl', '1.0.3'

  # Uncomment these lines on Linux.
  # gem 'libnotify', '0.8.0'

  # Uncomment these lines on Windows.
  # gem 'rb-notifu', '0.0.4'
  # gem 'win32console', '1.3.2'
end

gem 'sass-rails', '4.0.0'
gem 'uglifier', '2.1.1'
gem 'coffee-rails', '4.0.0'
gem 'jquery-rails', '3.0.4'
gem 'turbolinks', '1.1.1'
gem 'jbuilder', '1.0.2'

group :doc do
  gem 'sdoc', '0.3.20', require: false
end

group :production do
  gem 'pg', '0.15.1'
  gem 'rails_12factor', '0.0.2'
end
{% endhighlight %}


### Installing gem
  
{% highlight text %}
$ bundle install --without production
$ bundle update
$ bundle install
{% endhighlight %}

### Dynamically generating a secret token
Add config/initializers/secret_token.rb to .gitignore file

config/initializers/secret_token.rb

{% highlight ruby %}
# Be sure to restart your server when you modify this file.

# Your secret key is used for verifying the integrity of signed cookies.
# If you change this key, all old signed cookies will become invalid!

# Make sure the secret is at least 30 characters and all random,
# no regular words or you'll be exposed to dictionary attacks.
# You can use `rake secret` to generate a secure secret key.

# Make sure your secret_key_base is kept private
# if you're sharing your code publicly.
require 'securerandom'

def secure_token
  token_file = Rails.root.join('.secret')
  if File.exist?(token_file)
    # Use the existing token.
    File.read(token_file).chomp
  else
    # Generate a new token and store it in token_file.
    token = SecureRandom.hex(64)
    File.write(token_file, token)
    token
  end
end

SampleApp::Application.config.secret_key_base = secure_token
{% endhighlight %}

### Installing rspec and git

{% highlight text %}
$ rails generate rspec:install

$ git init
# add DS_Store to .gitignore

$ git add .
$ git commit -m "Initial commit"

# Change README.rdoc file
$ git mv README.rdoc README.md
$ git commit -am "Improve the README"
# Change the README file content.

# Create a new rep in github first. Then
$ git remote add origin https://github.com/<username>/sample_app.git
$ git push -u origin master
{% endhighlight %}

Deploy the app to Heroku.

{% highlight text %}
$ heroku create
$ rake assets:precompile
$ git commit -a -m "Add precompiled assets for Heroku"
$ git push heroku master
$ heroku run rake db:migrate

$ git push
$ git push heroku
$ heroku run rake db:migrate

$ heroku logs
{% endhighlight %}

In the video,

{% highlight text %}
$ heroku create --stack cedar
$ git push heroku master
{% endhighlight %}

To find URL run less .git/config

{% highlight text %}
$ less .git/config

[core]
        repositoryformatversion = 0
        filemode = true
        bare = false
        logallrefupdates = true
        ignorecase = true
        precomposeunicode = false
[remote "origin"]
        url = https://github.com/shinokada/sample_app.git
        fetch = +refs/heads/*:refs/remotes/origin/*
[branch "master"]
        remote = origin
        merge = refs/heads/master
[remote "heroku"]
        url = git@heroku.com:whispering-spire-8180.git
        fetch = +refs/heads/*:refs/remotes/heroku/*
.git/config (END)

# to quit/exit
$ q 
{% endhighlight %}

Easier way to open it is heroku open.

{% highlight text %}
$ heroku open
{% endhighlight %}


## Static pages

### Serving assets in production

/config/environments/production.rb

{% highlight ruby %}
SampleApp::Application.configure do
  ...
  config.serve_static_assets = true
  ...
end
{% endhighlight %}
  
{% highlight text %}
$ git push
$ git push heroku
$ heroku logs
{% endhighlight %}


### StaticPage controller

Default index page is in /public/index.html. You can create a static page in /public dir. Create /public/hello.html and add some contents. Visit http://localhost:3000/hello.html

Remove it.

{% highlight text %}
rm public/hello.html
{% endhighlight %}

Creat a working branch.

{% highlight text %}
$ git checkout -b static-pages

# --no-test-framework to suppress the generation of the default RSpec tests
$ rails generate controller StaticPages home help --no-test-framework
# check differences
$ git status
# create  app/controllers/static_pages_controller.rb
# check config/routes.rb to see the followins are added

get "static_pages/home"

get "static_pages/help"
{% endhighlight %}

Check on a browser http://localhost:3000/static_pages/home.

Run git instaweb or git instaweb --httpd webrick to see the log on a browser. You may need to install lighttpd by brew install lighttpd.

{% highlight text %}
$ git instaweb
$ git instaweb --httpd webrick
{% endhighlight %}

### Undoing 
  
{% highlight text %}
# controller
$ rails generate controller FooBars baz quux
$ rails destroy  controller FooBars baz quux
 
# model
$ rails generate model Foo bar:string baz:integer
$ rails destroy model Foo

# Migrageion
$ rake db:migrate
$ rake db:rollback
$ rake db:migrate VERSION=0
{% endhighlight %}

Notes from the video. Undoing by using rails destroy.

{% highlight text %}
$ rails generate controller StaticPages home help
# to undo
$ rails destroy controller StaticPages home help
{% endhighlight %}


### routes and controller

/config/routes.rb

{% highlight ruby %}
SampleApp::Application.routes.draw do
  get "static_pages/home"
  get "static_pages/help"
  ...
end
{% endhighlight %}

Find the view at app/views/static_pages/home.html.erb. Find the controller at app/controllers/static_pages_controller.rb
  
{% highlight ruby %}
class StaticPagesController < ApplicationController

  def home
  end

  def help
  end
end
{% endhighlight %}

  
{% highlight text %}
$ git add .
$ git commit -m "Add a StaticPages controller"
{% endhighlight %}

## Test-driven development
  
{% highlight text %}
$ rails generate integration_test static_pages
      invoke  rspec
      create    spec/requests/static_pages_spec.rb
{% endhighlight %}
  
{% highlight ruby %}
# Add this to spec/requests/static_pages_spec.rb
require 'spec_helper'

describe "Static pages" do

  describe "Home page" do

    it "should have the content 'Sample App'" do
      visit '/static_pages/home'
      expect(page).to have_content('Sample App')
    end
  end
end
{% endhighlight %}

it "should have the content 'Sample App'" - do part should be human readable. RSpec doesn't care.

visit '/static_pages/home' - visiting the URL /static_pages/home.

expect(page).to have_content('Sample App') - uses the page variable (also provided by Capybara) to express the expectation that the resulting page should have the right content.

At the moment, to get the test to run properly, we have to add a line to the spec_helper.rb file


Adding the Capybara DSL to the RSpec helper file. 
spec/spec_helper.rb

{% highlight text %}
# This file is copied to spec/ when you run 'rails generate rspec:install'
...
RSpec.configure do |config|
  ...
  config.include Capybara::DSL
end
{% endhighlight %}

  
{% highlight text %}
# And run a test to fail
$ bundle exec rspec spec/requests/static_pages_spec.rb
# Or
$ rspec spec/requests/static_pages_spec.rb
{% endhighlight %}

Add this to app/views/static_pages/home.html.erb

{% highlight erb %}
<h1>Sample App</h1>
<p>
bla bla
</p>
{% endhighlight %}

And run a test. This should pass with 0 failures.

{% highlight text %}
$ bundle exec rspec spec/requests/static_pages_spec.rb
# Or
$ rspec spec/requests/static_pages_spec.rb
{% endhighlight %} 

Add this to spec/requests/static_pages_spec.rb

{% highlight ruby %}
  describe "Help page" do

    it "should have the content 'Help'" do
      visit '/static_pages/help'
      expect(page).to have_content('Help')
    end
  end
{% endhighlight %}

{% highlight text %}
# And run a test to fail
$ bundle exec rspec spec/requests/static_pages_spec.rb
# Or
$ rspec spec/requests/static_pages_spec.rb
{% endhighlight %} 

Add this to app/views/static_pages/help.html.erb
  
{% highlight erb %}
<h1>Help</h1>
<p>
  Bla bla
</p>
{% endhighlight %}

{% highlight text %}
# Run a test to pass
$ bundle exec rspec spec/requests/static_pages_spec.rb
# Or
$ rspec spec/requests/static_pages_spec.rb
{% endhighlight %} 

## Adding About page manually

In stead of using `$ rails generate integration_test static_pages`, using TDD by writing a test, running RSpec. 

### Red

Add to spec/requests/static_pages_spec.rb

{% highlight ruby %}
describe "About page" do

  it "should have the content 'About Us'" do
    visit '/static_pages/about'
    expect(page).to have_content('About Us')
  end
end
{% endhighlight %}

{% highlight text %}
# And run a test to fail
$ bundle exec rspec spec/requests/static_pages_spec.rb
# Or
$ rspec spec/requests/static_pages_spec.rb
# This give an error
No route matches [GET] "/static_pages/about"
{% endhighlight %} 

### Green

{% highlight ruby %}
# Add to config/routes.rb
SampleApp::Application.routes.draw do
...
  get "static_pages/about"
{% endhighlight %}

{% highlight text %}
# And run a test to fail
$ bundle exec rspec spec/requests/static_pages_spec.rb
# Or
$ rspec spec/requests/static_pages_spec.rb

# error message
The action 'about' could not be found for StaticPagesController
{% endhighlight %} 

To fix this, add to app/controllers/static_pages_controller.rb
  
{% highlight ruby %}
class StaticPagesController < ApplicationController
...

  def about
  end
end
{% endhighlight %}

{% highlight text %}
# And run a test to fail
$ bundle exec rspec spec/requests/static_pages_spec.rb
# Or
$ rspec spec/requests/static_pages_spec.rb

# error message
Missing template static_pages/about
{% endhighlight %} 

To solve this, add app/views/static_pages/about.html.erb
  
{% highlight erb %}
<h1>About Us</h1>
<p>
Bla bla
</p>
{% endhighlight %}

{% highlight text %}
# And run a test to pass
$ bundle exec rspec spec/requests/static_pages_spec.rb
# Or
$ rspec spec/requests/static_pages_spec.rb
{% endhighlight %} 

### Adding a title Test
  
{% highlight text %}
$ mv app/views/layouts/application.html.erb foobar   # temporary change
{% endhighlight %}

  
{% highlight ruby %}
# Add to spec/requests/static_pages_spec.rb
...
    it "should have the title 'Home'" do
      visit '/static_pages/home'
      expect(page).to have_title("Ruby on Rails Tutorial Sample App | Home")
    end
...
    it "should have the title 'Help'" do
      visit '/static_pages/help'
      expect(page).to have_title("Ruby on Rails Tutorial Sample App | Help")
    end
...
    it "should have the title 'About Us'" do
      visit '/static_pages/about'
      expect(page).to have_title("Ruby on Rails Tutorial Sample App | About Us")
    end
...
{% endhighlight %}

{% highlight text %}
# And run a test to fail
$ bundle exec rspec spec/requests/static_pages_spec.rb
# Or
$ rspec spec/requests/static_pages_spec.rb
{% endhighlight %} 

#### Passing title tests
  
{% highlight erb %}
# app/views/static_pages/home.html.erb
<% provide(:title, 'Home') %>
<!DOCTYPE html>
<html>
  <head>
    <title>Ruby on Rails Tutorial Sample App | <%= yield(:title) %></title>
  </head>
  <body>
    <h1>Sample App</h1>
    <p>
      Bla bla
    </p>
  </body>
</html>
{% endhighlight %}


  
{% highlight erb %}
# app/views/static_pages/help.html.erb
<% provide(:title, 'Help') %>
<!DOCTYPE html>
<html>
  <head>
    <title>Ruby on Rails Tutorial Sample App | <%= yield(:title) %></title>
  </head>
  <body>
    <h1>Help</h1>
    <p>
      Bla bla
    </p>
  </body>
</html>
{% endhighlight %}


  
{% highlight erb %}
# app/views/static_pages/about.html.erb
<% provide(:title, 'About Us') %>
<!DOCTYPE html>
<html>
  <head>
    <title>Ruby on Rails Tutorial Sample App | <%= yield(:title) %></title>
  </head>
  <body>
    <h1>About Us</h1>
    <p>
      Bla bla
    </p>
  </body>
</html>
{% endhighlight %}

## Refactor

{% highlight text %}
$ mv foobar app/views/layouts/application.html.erb
{% endhighlight %}


  
{% highlight erb %}
app/views/layouts/application.html.erb
<!DOCTYPE html>
<html>
<head>
  <title>Ruby on Rails Tutorial Sample App | <%= yield(:title) %></title>
  <%= stylesheet_link_tag    "application", media: "all",
                                            "data-turbolinks-track" => true %>
  <%= javascript_include_tag "application", "data-turbolinks-track" => true %>
  <%= csrf_meta_tags %>
</head>
<body>

<%= yield %>

</body>
</html>
{% endhighlight %}

#### <%= yield %>

page /static_pages/home converts the contents of home.html.erb to HTML and then inserts it in place of <%= yield %>

  
{% highlight erb %}
# app/views/static_pages/home.html.erb
<% provide(:title, 'Home') %>
<h1>Sample App</h1>
<p>
Bla bla
</p>

# app/views/static_pages/help.html.erb
<% provide(:title, 'Help') %>
<h1>Help</h1>
<p>
Bla bla
</p>

# app/views/static_pages/about.html.erb
<% provide(:title, 'About Us') %>
<h1>About Us</h1>
<p>
Bla bla
</p>
{% endhighlight %}

{% highlight text %}
# And run a test to pass
$ bundle exec rspec spec/requests/static_pages_spec.rb
# Or
$ rspec spec/requests/static_pages_spec.rb
{% endhighlight %} 


### git commit
  
{% highlight text %}
$ git add .
$ git commit -m "Finish static pages"

$ git checkout master
$ git merge static-pages

$ git push

$ git push heroku
{% endhighlight %}


## Using Guard

{% highlight ruby %}
# Gemfile
...
group :development, :test do
  gem 'sqlite3', '1.3.7'
  gem 'rspec-rails', '2.13.1'
  gem 'guard-rspec', '2.5.0'
end
...
{% endhighlight %}

Install them.

{% highlight text %}
$ bundle install
{% endhighlight %}

Initialize Guard to work with RSpec.

  
{% highlight text %}
$ bundle exec guard init rspec
{% endhighlight %}

Modify Guardfile.

{% highlight ruby %}
# Guardfile.
require 'active_support/inflector'
# ensures that Guard doesn’t run all the tests after a failing test passes (to speed up the Red-Green-Refactor cycle).
guard 'rspec', all_after_pass: false do
...
...
# Add more custom Rails specs
{% endhighlight %}

Start guard.

  
{% highlight text %}
$ bundle exec guard
{% endhighlight %}


### Notes about Stop typing bundle exec

After adding an executable, run this.

{% highlight text %}
$ bundle binstubs <gemname>
$ bundle binstubs rspec-rails
$ bundle binstubs rspec-core
$ bundle binstubs guard
$ bundle binstubs spork
# Don't do this
# bundle install --binstubs

# To delete bin from .bundle/config and remove contents of bin dir
$ bundle config --delete bin && rm -rf bin
$ rake rails:update:bin
# In order to install binstubs
$ bundle binstubs <gemname>
{% endhighlight %}

bundle install --binstubs creates a bin directory at the root of your project, and fills it with Bundler-enabled wrappers for all of the executables installed by the gems listed in your Gemfile. This enables you to type bin/rake instead of bundle exec rake.
[stop typing bundle exec](http://blog.davidchelimsky.net/2011/07/18/stop-typing-bundle-exec/)

  
{% highlight text %}
# run guard
$ bin/guard
# And you can use like this.
$ bin/rspec spec/
$ bin/rake db:migrate
{% endhighlight %}

Update .gitignore file.  

{% highlight text %}
# Ignore other unneeded files.
doc/
*.swp
*~
.project
.DS_Store
.idea
bundler_stubs/
{% endhighlight %}

## Speeding up tests with Spork
  
 bundle exec rspec has to reload the entire Rails environment. Spork loads the environment once. Spork is particularly useful when combined with Guard.

Add the spork gem to the Gemfile.

{% highlight ruby %}
# Gemfile
...
group :development, :test do
  ...
  gem 'spork-rails', github: 'railstutorial/spork-rails'
  gem 'guard-spork', '1.5.0'
  gem 'childprocess', '0.3.6'
end
{% endhighlight %}

Install spork.
  
{% highlight text %}
$ bundle install

# Next, bootstrap the Spork configuration:
$ bundle exec spork --bootstrap
{% endhighlight %}

Update RSpec configuration file, spec/spec_helper.rb.
[Update spec/spec_helper.rb](http://ruby.railstutorial.org/chapters/static-pages?version=4.0#code-spork_spec_helper)

Before running Spork, we can get a baseline for the testing overhead by timing our test suite as follows. Check the time.
  
{% highlight text %}
$ time bundle exec rspec spec/requests/static_pages_spec.rb
# After running bundle install --binstubs 
# time bin/rspec spec/requests/static_pages_spec.rb
{% endhighlight %}

Open another terminal and start spork
  
{% highlight text %}
$ bundle exec spork
# Or
# bin/spork
{% endhighlight %}

Then test it again to see it is faster.
  
{% highlight text %}
$ time bundle exec rspec spec/requests/static_pages_spec.rb --drb
# OR 
# time bin/rspec spec/requests/static_pages_spec.rb --drb
{% endhighlight %}

Run spork when you do testing.

In order to eliminate --drb, modify .rspec
  
{% highlight ruby %}
--colour
--drb
{% endhighlight %}

### Guard with Spork
  
{% highlight text %}
$ bundle exec guard init spork
# OR
# $ bin/guard init spork
{% endhighlight %}

[And Modify the Guardfile].(http://ruby.railstutorial.org/chapters/static-pages?version=4.0#code-spork_guardfile)

Start Guard and Spork at once.

  
{% highlight text %}
$ bundle exec guard
# OR
# bin/guard
# OR 
# guard
{% endhighlight %}

## Testing with Sublime Text 2

### RubyTest

Install [RubyTest](https://github.com/maltize/sublime-text-2-ruby-tests).


#### Usage 

- Run single ruby test: Command-Shift-R
- Run all ruby tests from current file: Command-Shift-T
- Run last ruby test(s): Command-Shift-E
- Show test panel: Command-Shift-X (when test panel visible hit esc to hide it)
- Check RB, ERB file syntax: Alt-Shift-V
- Switching between code and test (create a file if not found):
    - Single View: Command-.
    - Split View: Command-Ctrl-.
- Easy file creation: Command-Shift-C Keys: 'Command' (OSX) 'Ctrl' (Linux / Windows)


#### Sequence of testing 

1. Start Spork in a terminal window. $ bundle exec spork
2. Write a single test or small group of tests.
3. Run Command-Shift-R to verify that the test or test group is red.
4. Write the corresponding application code.
5. Run Command-Shift-E to run the same test/group again, verifying that it’s green.
6. Repeat steps 2–5 as necessary.
7. When reaching a natural stopping point (such as before a commit), run rspec spec/ at the command line to confirm that the entire test suite is still green.








