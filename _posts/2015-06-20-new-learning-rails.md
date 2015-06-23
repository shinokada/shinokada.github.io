---
layout: post
title: "New Learning Rails"
date: 2015-06-20 16:17
categories: Rails
---

## Install RubyGems

    which gem
    # install the latest
    gem update --system 
    
    # Creating a gem configuration file.
    $ vim ~/.gemrc
    
    # Suppressing the ri and rdoc documentation in .gemrc.
    # Otherwise it will take more time to install
    install: --no-rdoc --no-ri
    update:  --no-rdoc --no-ri
    
    $ gem install rails --version 4.2.2
    
    $ rails -v


# The first application

  
    $ mkdir rails_projects
    $ cd rails_projects
    $ rails _4.2.2_ new hello_app


## Bundler and Gemfile

  
    $ cd hello_app/
    $ vim Gemfile

    # Gemfile
    source 'https://rubygems.org'
    
    gem 'rails',                '4.2.2'
    gem 'sass-rails',           '5.0.2'
    gem 'uglifier',             '2.5.3'
    gem 'coffee-rails',         '4.1.0'
    gem 'jquery-rails',         '4.0.3'
    gem 'turbolinks',           '2.3.0'
    gem 'jbuilder',             '2.2.3'
    gem 'sdoc',                 '0.4.0', group: :doc
    
    group :development, :test do
      gem 'sqlite3',     '1.3.9'
      gem 'byebug',      '3.4.0'
      gem 'web-console', '2.0.0.beta3'
      gem 'spring',      '1.1.3'
    end
    
    $ bundle install 
    
    #rails server
    $ rails server

## Hello, world!

In app/controllers/application_controller.rb

    class ApplicationController < ActionController::Base
      # Prevent CSRF attacks by raising an exception.
      # For APIs, you may want to use :null_session instead.
      protect_from_forgery with: :exception
    
      def hello
        render text: "hello, world!"
      end
    end

In config/routes.rb

    Rails.application.routes.draw do
      .
      .
      # You can have the root of your site routed with "root"
      root 'application#hello'
      .
    end


## git init, gitignore, add and commit

  

    $ git config --global user.name "Your Name"
    $ git config --global user.email your.email@example.com
    $ git config --global push.default matching
    $ git config --global alias.co checkout
    
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
    


## Undoing by checkout

  

    # you do something wrong
    $ ls app/controllers/
    application_controller.rb
    $ rm -rf app/controllers/
    $ ls app/controllers/
    ls: app/controllers/: No such file or directory
    
    # the changes are only on the working tree. The changes are not commited yet.
    git status 
    
    # Undo the chnages with -f flag to force overwriting
    git checkout -f
    # or delete the branch
    git checkout master
    git -D mess-branch
    git status

## GitHub
  
    # adding to github
    # first create a repo in github
    
    git remote add origin https//github.com/<username>/first_app.git
    git push -u origin master



## Branch, edit, commit, merge

  
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


## Heroku setup

Install [Heroku toolbelt](https://toolbelt.heroku.com/).
  
    # Heroku uses PostgreSQL
    # in Gemfile add this
    group :production do
      gem 'pg', '0.17.1'
      gem 'rails_12factor', '0.0.2'
    end
    
    bundle install --without production

rails_12factor gem is to serve static assets such as images and stylesheets

In order to install it,

    $ bundle install --without production

    $ git commit -a -m "Update Gemfile.lock for Heroku"

--without production prevents installing production gems.

    $ heroku login
    $ heroku keys:add
    $ cd /Desktop/rails_projects/first_app
    $ heroku create
    
    $ heroku git:remote -a name-inheroku-1234
    
    # Heroku deployment
    $ git push heroku master
    
    # Open it in a browser
    $ heroku open
    Opening ****-****-**** ... done

## Heroku commands

    # if you need to rename
    $ heroku rename new-name

# [Chapter 2 A toy app](https://railstutorial.org/book/toy-app)

## The Users resource

Rails scaffolding is generated by passing the scaffold command to the rails generate script. The argument of the scaffold command is the singular version of the resource name (in this case, User), together with optional parameters for the data model’s attributes.

  
    # create a new project 
    $ cd /Desktop/rails_projects
    $ rails _4.2.2_ new toy_app
    $ cd toy_app

Open the Gemfile and change it to the followings. 

    source 'https://rubygems.org'
    
    gem 'rails',        '4.2.2'
    gem 'sass-rails',   '5.0.2'
    gem 'uglifier',     '2.5.3'
    gem 'coffee-rails', '4.1.0'
    gem 'jquery-rails', '4.0.3'
    gem 'turbolinks',   '2.3.0'
    gem 'jbuilder',     '2.2.3'
    gem 'sdoc',         '0.4.0', group: :doc
    
    group :development, :test do
      gem 'sqlite3',     '1.3.9'
      gem 'byebug',      '3.4.0'
      gem 'web-console', '2.0.0.beta3'
      gem 'spring',      '1.1.3'
    end
    
    group :production do
      gem 'pg',             '0.17.1'
      gem 'rails_12factor', '0.0.2'
    end

Install these without production

    $ bundle install --without production

Using git create a new github repository of demo_app.

    $ git init
    # Add .DS_Store to .gitignore for Mac users.
    
    $ add .
    $ git commit -m "Initial commit"
    $ git remote add origin git@bitbucket.org/sokada/toy_app.git
    $ git push -u origin -all

Add "hello world!" in route

    $ git commit -am "Add hello"
    $ heroku create
    $ git push heroku master

## The Users resource

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

Check out 

* http://localhost:3000/users
* http://localhost:3000/users/new
* http://localhost:3000/users/1
* http://localhost:3000/users/1/edit

Now check config/routes.rb. resources :users is the key code which coresponds url and source codes. 

    Rails.application.routes.draw do
      resources :users
      root 'users#index'
      .
      .
    ends

Check the source codes. Open app/controllers/users_controller.rb. 

`index`, `show`, `new` and `edit` correspond to pages. `create`, `update`, `destroy` modify information in the database.

A variable `@users` stores all the users. 

    class UsersController < ApplicationController
    
      def index
        @users = User.all
      end
    
    
    end

Open app/views/users/index.html.erb to see the index view.

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
    <%= link_to 'New User', new_user_path %>

And check app/models/user.rb for it's model.

    class User < ActiveRecord::Base
    end

@users get from the user model (user.rb) with all method.

# Microposts scaffold

    $ rails generate scaffold Micropost content:string user_id:integer
    
    $ bundle exec rake db:migrate

Check app/config/routes.rb to see a new resource.

    DemoApp::Application.routes.draw do
      resources :microposts
      resources :users
      ...
    end


## Validation

app/models/micropost.rb
  
    class Micropost < ActiveRecord::Base
      validates :content, length: { maximum: 140 }
    end

Try to add more than 140 characters at http://localhost:3000/microposts/new to see the validation at work.

## A user has_many microposts
  
    # app/models/user.rb
    class User < ActiveRecord::Base
      has_many :microposts
    end
    
    # app/models/micropost.rb
    class Micropost < ActiveRecord::Base
      belongs_to :user
      validates :content, length: { maximum: 140 }
    end

Invoking rails console.
 
    # terminal
    $ rails console
    >> first_user = User.first
    >> first_user # to display first_user
    >> first_user.name
    >> first_user.email
    >> first_user.microposts # to display all microposts by first_user
    
    >> User.all # to display all users
    
    >> micropost = first_user.mocroposts.first
    >> micropost.user
    
    $ exit # or ctrl+d

## Inheritance hierarchies
User model and the Micropost model inherit (via the left angle bracket <) from ActiveRecord::Base which is the base class for models provided by ActiveRecord.

The User class, with inheritance. 
app/models/user.rb

    class User < ActiveRecord::Base
      .
      .
      .
    end

The Micropost class, with inheritance. 
app/models/micropost.rb

    class Micropost < ActiveRecord::Base
      .
      .
      .
    end

The Users controller and the Microposts controller inherit from the Application controller.

The UsersController class, with inheritance. 
app/controllers/users_controller.rb

    class UsersController < ApplicationController
      .
      .
      .
    end

The MicropostsController class, with inheritance. 
app/controllers/microposts_controller.rb

    class MicropostsController < ApplicationController
      .
      .
      .
    end

The ApplicationController class, with inheritance. 
app/controllers/application_controller.rb

    class ApplicationController < ActionController::Base
      .
      .
      .
    end

## Deploying the demo app
Add to git and push it.

    # git
    $ git add .
    $ git commit -m "Finish demo app"
    $ git push
    
    $ heroku create # if you have not created it
    $ git push heroku 
    $ heroku run rake db:migrate
    
    # To get the application’s database to work, you’ll also have to migrate the production database
    $ heroku run rake db:migrate
    
    $ heroku logs

Check the Heroku site on a browser. If it gives an error, check it with heroku logs. 


# Sample app [Lesson 3](http://railstutorial.org/book/static-pages#top)

## Setup 

    $ cd ~/rails_projects
    $ rails _4.2.2_ new sample_app
    $ cd sample_app
  
    source 'https://rubygems.org'
    
    gem 'rails',        '4.2.2'
    gem 'sass-rails',   '5.0.2'
    gem 'uglifier',     '2.5.3'
    gem 'coffee-rails', '4.1.0'
    gem 'jquery-rails', '4.0.3'
    gem 'turbolinks',   '2.3.0'
    gem 'jbuilder',     '2.2.3'
    gem 'sdoc',         '0.4.0', group: :doc
    
    group :development, :test do
      gem 'sqlite3',     '1.3.9'
      gem 'byebug',      '3.4.0'
      gem 'web-console', '2.0.0.beta3'
      gem 'spring',      '1.1.3'
    end
    
    group :test do
      gem 'minitest-reporters', '1.0.5'
      gem 'mini_backtrace',     '0.1.3'
      gem 'guard-minitest',     '2.3.1'
    end
    
    group :production do
      gem 'pg',             '0.17.1'
      gem 'rails_12factor', '0.0.2'
    end

Install, init git, heroku setup.

    $ bundle install --without production
    
    $ git init
    $ git add -A
    $ git commit -m "Initialize repository"
    $ git mv README.rdoc README.md
    # modify README.md
    $ git commit -am "Improve the README"
    $ git remote add origin git@bitbucket.org:<username>/sample_app.git
    $ git push -u origin --all
    # add hello to config/routes and application_controller.rb as we did in the first chapter
    $ git commit -am "Add hello"
    $ heroku create
    $ git push heroku master
    $ heroku logs

## Static pages

    $ git checkout master
    $ git checkout -b static-pages

### Generated static pages

Generating a static pages controller for Home and Help.

    $ rails generate controller StaticPages home help

Rails shortcuts

    $ rails s # shortcut for rails server
    $ rails g # shorcut for rails generate
    $ rails c # for rails console
    $ bundle # for bundle install
    $ rake # for rake test


    $ git status
    $ git add -A
    $ git commit -m "Add a Static Pages controller"
    $ git push -u origin static-pages


### Dynamically generating a secret token
Add config/initializers/secret_token.rb to .gitignore file

config/initializers/secret_token.rb

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

### Installing rspec and git

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

Deploy the app to Heroku.

    $ heroku create
    $ rake assets:precompile
    $ git commit -a -m "Add precompiled assets for Heroku"
    $ git push heroku master
    $ heroku run rake db:migrate
    
    $ git push
    $ git push heroku
    $ heroku run rake db:migrate
    
    $ heroku logs

In the video,

    $ heroku create --stack cedar
    $ git push heroku master

To find URL run less .git/config

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

Easier way to open it is heroku open.

    $ heroku open


## Static pages

### Serving assets in production

/config/environments/production.rb

    SampleApp::Application.configure do
      ...
      config.serve_static_assets = true
      ...
    end
  
    $ git push
    $ git push heroku
    $ heroku logs

### Undoing 
  
    # controller
    $ rails generate controller StaticPages home help
    $ rails destroy  controller StaticPages home help
     
    # model
    $ rails generate model User name:string email:string
    $ rails destroy model User
    
    # Migrageion
    $ bundle exec rake db:migrate
    # or
    $ rake db:migrate
    $ bundle exec rake db:rollback
    # or
    $ rake db:rollback
    $ bundle exec rake db:migrate VERSION=0 # to go all the way back to the beginning
    # or
    $ rake db:migrate VERSION=0

Check it on a browser to visit `http://localhost:3000/static_pages/help` and `http://localhost:3000/static_pages/home`.

    rails s


### routes and controller

/config/routes.rb

    SampleApp::Application.routes.draw do
      get "static_pages/home"
      get "static_pages/help"
      ...
    end

Find the view at `app/views/static_pages/home.html.erb`. Find the controller at app/controllers/static_pages_controller.rb
  
    class StaticPagesController < ApplicationController
    
      def home
      end
    
      def help
      end
    end

Add some contents to app/views/static_pages/home.html.erb and help.html.erb.


## Test-driven development

Scafold create test/controllers/static_pages_controller_test.rb.

Run tests.

    $ bundle exec rake test 
    # or
    $ rake test

### Red

test/controllers/static_pages_controller_test.rb

    require 'test_helper'
    
    class StaticPagesControllerTest < ActionController::TestCase
    
      test "should get home" do
        get :home
        assert_response :success
      end
    
      test "should get help" do
        get :help
        assert_response :success
      end
    
      test "should get about" do
        get :about
        assert_response :success
      end
    end

Run a test

    $ bundle exec rake test # or rake test

### Green

Add the following to config/routes.rb

    get 'static_pages/about'

Run a test to fail again.

Modify 
app/controllers/static_pages_controller.rb

    class StaticPagesController < ApplicationController
    
      def home
      end
    
      def help
      end
    
      def about
      end
    end

Run a test and it will give an error about missing template static_pages/about.

Add the following to app/views/static_pages/about.html.erb

    <h1>About</h1>
    <p>
      This is the sample application for the tutorial.
    </p>

Run a test to pass.


### Adding a title Test
  
    $ mv app/views/layouts/application.html.erb foobar   # temporary change

  
Add to test/controllers/static_pages_controller_test.rb

    require 'test_helper'
    
    class StaticPagesControllerTest < ActionController::TestCase
    
      test "should get home" do
        get :home
        assert_response :success
        assert_select "title", "Home | Ruby on Rails Tutorial Sample App"
      end
    
      test "should get help" do
        get :help
        assert_response :success
        assert_select "title", "Help | Ruby on Rails Tutorial Sample App"
      end
    
      test "should get about" do
        get :about
        assert_response :success
        assert_select "title", "About | Ruby on Rails Tutorial Sample App"
      end
    end

And run a test to fail

    $ bundle exec rake test 
    # Or
    $ rake test

### Passing title tests

ERB is the primary template system for including dynamic content. 
`<% ... %>` executes the code inside and `<%= ...%>` executes and inserts the result.

app/views/static_pages/home.html.erb

    <% provide(:title, "Home") %>
    <!DOCTYPE html>
    <html>
      <head>
        <title><%= yield(:title) %> | Ruby on Rails Tutorial Sample App</title>
      </head>
      <body>
        <h1>Sample App</h1>
        <p>
          This is the home page for the
          <a href="http://www.railstutorial.org/">Ruby on Rails Tutorial</a>
          sample application.
        </p>
      </body>
    </html>


  
app/views/static_pages/help.html.erb

    <% provide(:title, 'Help') %>
    <!DOCTYPE html>
    <html>
      <head>
        <title><%= yield(:title) %> | Ruby on Rails Tutorial Sample App</title>
      </head>
      <body>
        <h1>Help</h1>
        <p>
          Bla bla
        </p>
      </body>
    </html>


  
app/views/static_pages/about.html.erb

    <% provide(:title, 'About Us') %>
    <!DOCTYPE html>
    <html>
      <head>
        <title><%= yield(:title) %> | Ruby on Rails Tutorial Sample App</title>
      </head>
      <body>
        <h1>About Us</h1>
        <p>
          Bla bla
        </p>
      </body>
    </html>

## Refactor

    $ mv foobar app/views/layouts/application.html.erb

app/views/layouts/application.html.erb

    <!DOCTYPE html>
    <html>
    <head>
         <title><%= yield(:title) %> | Ruby on Rails Tutorial Sample App</title>
      <%= stylesheet_link_tag    "application", media: "all",
                                                "data-turbolinks-track" => true %>
      <%= javascript_include_tag "application", "data-turbolinks-track" => true %>
      <%= csrf_meta_tags %>
    </head>
    <body>
    
    <%= yield %>
    
    </body>
    </html>

### <%= yield %>

page /static_pages/home converts the contents of home.html.erb to HTML and then inserts it in place of <%= yield %>

  
app/views/static_pages/home.html.erb

    <% provide(:title, 'Home') %>
    <h1>Sample App</h1>
    <p>
    Bla bla
    </p>
    
app/views/static_pages/help.html.erb

    <% provide(:title, 'Help') %>
    <h1>Help</h1>
    <p>
    Bla bla
    </p>
    
app/views/static_pages/about.html.erb

    <% provide(:title, 'About Us') %>
    <h1>About Us</h1>
    <p>
    Bla bla
    </p>

And run a test to pass

    $ bundle exec rake test 
    # Or
    $ rake test

Setting the root route.

config/routes.rb

    Rails.application.routes.draw do
      root 'static_pages#home'
      get  'static_pages/help'
      get  'static_pages/about'
    end

### git commit
  
    $ git add .
    $ git commit -m "Finish static pages"
    
    $ git checkout master
    $ git merge static-pages
    
    $ git push
    $ rake test
    $ git push heroku

## Advanced testing setup

### MiniTest reporters

test/test_helper.rb

    ENV['RAILS_ENV'] ||= 'test'
    require File.expand_path('../../config/environment', __FILE__)
    require 'rails/test_help'
    require "minitest/reporters"
    Minitest::Reporters.use!
    
    class ActiveSupport::TestCase
      # Setup all fixtures in test/fixtures/*.yml for all tests in alphabetical
      # order.
      fixtures :all
    
      # Add more helper methods to be used by all tests here...
    end

### Backtrace silencer

config/initializers/backtrace_silencers.rb

    # Be sure to restart your server when you modify this file.
    
    # You can add backtrace silencers for libraries that you're using but don't
    # wish to see in your backtraces.
    Rails.backtrace_cleaner.add_silencer { |line| line =~ /rvm/ }


### Using Guard
Guard automates the running of the tests. Guard monitors changes in the filesystem so that when we change a file, only those tests get run. Or we can configure Guard so that a file is modified, the static_pages_controller_test.rb runs.

Initialize Guard. 
  
    $ bundle exec guard init

Modify Guardfile.

Defines the matching rules for Guard.

    guard :minitest, spring: true, all_on_start: false do
      watch(%r{^test/(.*)/?(.*)_test\.rb$})
      watch('test/test_helper.rb') { 'test' }
      watch('config/routes.rb')    { integration_tests }
      watch(%r{^app/models/(.*?)\.rb$}) do |matches|
        "test/models/#{matches[1]}_test.rb"
      end
      watch(%r{^app/controllers/(.*?)_controller\.rb$}) do |matches|
        resource_tests(matches[1])
      end
      watch(%r{^app/views/([^/]*?)/.*\.html\.erb$}) do |matches|
        ["test/controllers/#{matches[1]}_controller_test.rb"] +
        integration_tests(matches[1])
      end
      watch(%r{^app/helpers/(.*?)_helper\.rb$}) do |matches|
        integration_tests(matches[1])
      end
      watch('app/views/layouts/application.html.erb') do
        'test/integration/site_layout_test.rb'
      end
      watch('app/helpers/sessions_helper.rb') do
        integration_tests << 'test/helpers/sessions_helper_test.rb'
      end
      watch('app/controllers/sessions_controller.rb') do
        ['test/controllers/sessions_controller_test.rb',
         'test/integration/users_login_test.rb']
      end
      watch('app/controllers/account_activations_controller.rb') do
        'test/integration/users_signup_test.rb'
      end
      watch(%r{app/views/users/*}) do
        resource_tests('users') +
        ['test/integration/microposts_interface_test.rb']
      end
    end
    
    # Returns the integration tests corresponding to the given resource.
    def integration_tests(resource = :all)
      if resource == :all
        Dir["test/integration/*"]
      else
        Dir["test/integration/#{resource}_*.rb"]
      end
    end
    
    # Returns the controller tests corresponding to the given resource.
    def controller_test(resource)
      "test/controllers/#{resource}_controller_test.rb"
    end
    
    # Returns all tests for the given resource.
    def resource_tests(resource)
      integration_tests(resource) << controller_test(resource)
    end
    

Add spring/ directory to the .gitignore file.

Ignore Spring files.

    /spring/*.pid

#### Unix processes

To see all the processes on your system

    $ ps aux
    # filter the processes by type
    $ ps aux | grep spring
    sokada          31639   0.2  1.2  2559416 104196   ??  Ss    9:20PM   0:04.58 spring app    | sample_app | started 17 mins ago | development mode
    
    # To eliminate an individual unwanted process
    $ kill -9 31639
    
    # kill all spring processes
    $ spring stop
    # if this doesn't work 
    $ pkill -9 -f spring
    

Start guard.

    $ bundle exec guard
    # or guard
    $ guard
    
    # To exit guard
    Ctrl+D

