---
layout: post
title: "Notes on Personal Notes on Learning Rails 3"
date: 2013-09-17 14:43
comments: true
categories: Rails
---

<!-- more -->

{:.no_toc}

* Will be replaced with the ToC, excluding the "Contents" header
{:toc}


## [Bootstrap and custom CSS](http://ruby.railstutorial.org/chapters/filling-in-the-layout#sec-custom_css)

Add a gem to the Gemfile.

{% highlight ruby %}
gem 'bootstrap-sass', '2.3.2.0'
{% endhighlight %}

Install it. And restart the server.

{% highlight text %}
$ bundle install
{% endhighlight %}

Add a line, config.assets.precompile += %w(*.png *.jpg *.jpeg *.gif) to config/application.rb

{% highlight ruby %}
require File.expand_path('../boot', __FILE__)
.
.
.
module SampleApp
  class Application < Rails::Application
    .
    .
    .
    config.assets.precompile += %w(*.png *.jpg *.jpeg *.gif)
  end
end
{% endhighlight %}

Any stylesheets in app/assets/stylesheets will be included as part of the application.css.

Adding Bootstrap CSS.

app/assets/stylesheets/custom.css.scss

{% highlight css %}
@import "bootstrap";
{% endhighlight %}

## Partials

{% highlight ruby %}
<%= render 'layouts/shim' %>
{% endhighlight %}

This will insert app/views/layouts/_shim.html.erb. The underscore is the universal convention for naming partials.

## Asset pipeline

### Asset directories

app/assets: assets specific to the present application
lib/assets: assets for libraries written by your dev team
vendor/assets: assets from third-party vendors

### Manifest file

app/assets/stylesheets/application.css

{% highlight ruby %}
*= require_tree .
{% endhighlight %}
ensures that all CSS files in the app/assets/stylesheets directory (including the tree subdirectories) are included into the application CSS.

{% highlight ruby %}
*= require_self
{% endhighlight %}

application.css itself gets included.

[RailsGuides asset pipeline](http://guides.rubyonrails.org/asset_pipeline.html)

[The Bootstrap page of LESS variables](http://bootstrapdocs.com/v2.0.4/docs/less.html)

 LESS uses an “at” sign @, Sass uses a dollar sign $. $grayLight.


## Layout links

Commenting out.

	
{% highlight ruby %}
<%#= link_to "Contact", '#' %>
{% endhighlight %}

## Rails routes

{% highlight ruby %}
# change from
get 'static_pages/help'
# to
match '/help', to: 'static_pages#help', via: 'get'
{% endhighlight %}

match ’/about’ also automatically creates named routes for use in the controllers and views:
	
{% highlight ruby %}
about_path -> '/about'
about_url  -> 'http://localhost:3000/about'
{% endhighlight %}

Use the url form for redirects since the HTTP standard technically requires a full URL, although in most browsers it will work either way.

For the root route.
	
{% highlight ruby %}
# You can have the root of your site routed with "root"
root 'welcome#index'
# for this sample_app
root  'static_pages#home'

# this will give us
root_path -> '/'
root_url  -> 'http://localhost:3000/'
{% endhighlight %}


{% highlight text %}
# run all the request specs by passing the whole directory 
$ bundle exec rspec spec/requests/

# to run all the specs:
$ bundle exec rspec spec/
# or
$ bundle exec rake spec
{% endhighlight %}



## [User model](http://ruby.railstutorial.org/chapters/modeling-users#sec-user_model)

Generate a User model.

{% highlight text %}
$ rails generate model User name:string email:string
{% endhighlight %}

Migration for the User model

The above rails generate command creates db/migrate/[timestamp]_create_users.rb
	
{% highlight ruby %}
class CreateUsers < ActiveRecord::Migration
  def change
    create_table :users do |t|
      t.string :name
      t.string :email

      t.timestamps
    end
  end
end
{% endhighlight %}

Here the table name is plural (users) even though the model name is singular (User), which reflects a linguistic convention followed by Rails: a model represents a single user, whereas a database table consists of many users. 

{% highlight text %}
# run the migration
$ bundle exec rake db:migrate

# to migrate down/undo
$ bundle exec rake db:rollback

# Migrate up again
$ bundle exec rake db:migrate
{% endhighlight %}


Running the migration created a file db/development.sqlite3 and model/user.rb.

Using sandbox which will be rolled back on exit.

{% highlight text %}
$ rails console --sandbox
{% endhighlight %}

User.new returns an object with all nil attributes.
	
{% highlight text %}
>> User.new

>> user = User.new(name: "Michael Hartl", email: "mhartl@example.com")
>> user.save
>> user

# or use create 
>> User.create(name: "A Nother", email: "another@example.org")

>> foo = User.create(name: "Foo", email: "foo@bar.com")

# The inverse of create is destroy:
>> foo.destroy
{% endhighlight %}

## Finding user objects

  
{% highlight text %}
>> User.find(1)
>> User.find_by_email("mhartl@example.com")
>> User.find_by(email: "mhartl@example.com")

# The preferred method to find by attribute is 
>> User.find_by(email: "mhartl@example.com")

>> User.first
>> User.all

{% endhighlight %}

## Updating user objects

  
{% highlight text %}
>> user.email = "mhartl@example.net"
=> "mhartl@example.net"
>> user.save

# We can see what happens without a save by using reload, which reloads the object based on the database information
>> user.email
=> "mhartl@example.net"
>> user.email = "foo@bar.com"
=> "foo@bar.com"
>> user.reload.email
=> "mhartl@example.net"

# Update with update_attributes
>> user.update_attributes(name: "The Dude", email: "dude@abides.org")
=> true
>> user.name
=> "The Dude"
>> user.email
=> "dude@abides.org"

# Update with update_attribute (note singular)
>> user.update_attribute(:name, "The Dude")
{% endhighlight %}

## Validations

$ bundle exec rake test:prepare

This just ensures that the data model from the development database, db/development.sqlite3, is reflected in the test database, db/test.sqlite3.

respond_to methods accepts a symbol and returns true if the object responds to the given method or attribute and false
  
{% highlight ruby %}
it "should respond to 'name'" do
  expect(@user).to respond_to(:name)
end

# This can be written with one-line style
subject { @user }
it { should respond_to(:name) }

{% endhighlight %}


## Validating the presence of attributes

app/models/user.rb

{% highlight ruby %}
class User < ActiveRecord::Base
  # You can use this one 
  # validates :name, presence: true
  # or this one
  validates(:name, presence: true)
end
{% endhighlight %}

Checking it in the console.
  
{% highlight text %}
$ rails console --sandbox
>> user = User.new(name: "", email: "mhartl@example.com")
>> user.save
=> false
>> user.valid?
=> false
>> user.errors.full_messages
=> ["Name can't be blank"]
{% endhighlight %}

In spec/models/user_spec.rb
  
{% highlight ruby %}
it "should be valid" do
  expect(@user).to be_valid
end

# can be written with one-line style
it { should be_valid }
{% endhighlight %}

spec/models/user_spec.rb
  
{% highlight ruby %}
require 'spec_helper'

describe User do
  before { @user = User.new(name: "Example User", email: "user@example.com") }

  subject { @user }

  it { should respond_to(:name) }
  it { should respond_to(:email) }

  it { should be_valid }

  describe "when name is not present" do
    before { @user.name = " " }
    it { should_not be_valid }
  end

    describe "when email is not present" do
    before { @user.email = " " }
    it { should_not be_valid }
  end
end

{% endhighlight %}

app/models/user.rb
  
{% highlight ruby %}
class User < ActiveRecord::Base
  validates :name,  presence: true
  validates :email, presence: true
end
{% endhighlight %}


## Validating length

spec/models/user_spec.rb
  
{% highlight ruby %}
  describe "when name is too long" do
    before { @user.name = "a" * 51 }
    it { should_not be_valid }
  end
{% endhighlight %}

app/models/user.rb
  
{% highlight ruby %}
class User < ActiveRecord::Base
  validates :name,  presence: true, length: { maximum: 50 }
  validates :email, presence: true
end
{% endhighlight %}






