---
layout: post
title: "Commands in Getting Started with Rails"
date: 2013-09-21 16:57
comments: true
categories: Rails
---

<!-- more -->

{:.no_toc}

* Will be replaced with the ToC, excluding the "Contents" header
{:toc}


## [Getting Started with Rails](http://guides.rubyonrails.org/getting_started.html)

	
{% highlight text %}
$ ruby -v
$ rails new blog
$ cd blog
$ rails s
$ rails g controller welcome index
{% endhighlight %}

Edit config/routes.rb by adding resources and root to
{% highlight ruby %}
...
resources :posts

root to: "welcome#index"
...
{% endhighlight %}

	
{% highlight text %}
# Check RESTful actions.
$ rake routes

# create posts controller
$ rails g controller posts
{% endhighlight %}

http://localhost:3000/posts/new not working. Add new to 
posts_controller.rg

{% highlight ruby %}
def new
end
{% endhighlight %}

Create app/views/posts/new.html.erb

{% endhighlight %} erb
<h1>New Post</h1>
	
<%= form_for :post, url: posts_path do |f| %>
	<p>
	  <%= f.label :title %><br>
	  <%= f.text_field :title %>
	</p>

	<p>
	  <%= f.label :text %><br>
	  <%= f.text_area :text %>
	</p>

	<p>
	  <%= f.submit %>
	</p>
<% end %>

<%= link_to 'Back', posts_path %>
{% endhighlight %}

Add create to posts_controller.rb

	
{% highlight ruby %}
def create
end
{% endhighlight %}

Create Post model
	
{% highlight text %}
$ rails g model Post title:string text:text
{% endhighlight %}

This will create app/models/post.rb and db/migrate/20120419084633_create_posts.rb.

Check db/migrate/20120419084633_create_posts.rb and run migration.
	
{% highlight text %}
$ rake db:migrate
{% endhighlight %}

Edit posts_controller.rb for create.

{% highlight ruby %}
def create
  @post = Post.new(post_params)
 
  @post.save
  redirect_to @post
end
 
private
  def post_params
    params.require(:post).permit(:title, :text)
  end
{% endhighlight %}

Edit posts_controller.rb for show. Add it before private.
	
{% highlight ruby %}
def show
  @post = Post.find(params[:id])
end
{% endhighlight %}

Create app/views/posts/show.html.erb.
	
{% endhighlight %} erb
<p>
  <strong>Title:</strong>
  <%= @post.title %>
</p>
 
<p>
  <strong>Text:</strong>
  <%= @post.text %>
</p>
{% endhighlight %}

Can edit posts_controller.rb
	
{% highlight ruby %}
def create
  @post = Post.new(params[:post].permit(:title, :text))
 
  @post.save
  redirect_to @post
end
{% endhighlight %}

Edit posts_controller.rb index.
	
{% highlight ruby %}
def index
  @posts = Post.all
end
{% endhighlight %}

Edit app/views/posts/index.html.erb:
	
{% endhighlight %} erb
<h1>Listing posts</h1>
 
<table>
  <tr>
    <th>Title</th>
    <th>Text</th>
  </tr>
 
  <% @posts.each do |post| %>
    <tr>
      <td><%= post.title %></td>
      <td><%= post.text %></td>
    </tr>
  <% end %>
</table>
{% endhighlight %}

## Links 

Edit  app/views/welcome/index.html.erb.
	
{% endhighlight %} erb
<h1>Hello, Rails!</h1>
<%= link_to "My Blog", controller: "posts" %>
...
...
<%= link_to 'New post', new_post_path %>
{% endhighlight %}

Add in app/views/posts/new.html.erb.
	
{% endhighlight %} erb
...
<%= link_to 'Back', posts_path %>
{% endhighlight %}

Add in app/views/posts/show.html.erb.
	
{% endhighlight %} erb
...
<%= link_to 'Back', posts_path %>
{% endhighlight %}

## Validation

In app/models/post.rb.
	
{% highlight ruby %}
...
validates :title, presence: true,
                    length: { minimum: 5 }
...
{% endhighlight %}

Change new in app/controllers/posts_controller.rb.
	
{% highlight ruby %}
def new
  @post = Post.new
end
 
def create
  @post = Post.new(params[:post].permit(:title, :text))
 
  if @post.save
    redirect_to @post
  else
    render 'new'
  end
end
{% endhighlight %}

Added @post = Post.new in posts_controller is that otherwise @post would be nil in our view, and calling @post.errors.any? would throw an error.

Use render instead of redirect_to when save returns false. The render method is used so that the @post object is passed back.

Add error message to app/views/posts/new.html.erb after form tag.
	
{% highlight ruby %}
  <% if @post.errors.any? %>
  <div id="errorExplanation">
    <h2><%= pluralize(@post.errors.count, "error") %> prohibited
      this post from being saved:</h2>
    <ul>
    <% @post.errors.full_messages.each do |msg| %>
      <li><%= msg %></li>
    <% end %>
    </ul>
  </div>
  <% end %>
{% endhighlight %}

## Updating posts

In posts_controller.

{% highlight ruby %}
def edit
  @post = Post.find(params[:id])
end
{% endhighlight %}

Create a file called app/views/posts/edit.html.erb.
	
{% endhighlight %} erb
<h1>Editing post</h1>
 
<%= form_for :post, url: post_path(@post.id) },
method: :patch do |f| %>
  <% if @post.errors.any? %>
  <div id="errorExplanation">
    <h2><%= pluralize(@post.errors.count, "error") %> prohibited
      this post from being saved:</h2>
    <ul>
    <% @post.errors.full_messages.each do |msg| %>
      <li><%= msg %></li>
    <% end %>
    </ul>
  </div>
  <% end %>
  <p>
    <%= f.label :title %><br>
    <%= f.text_field :title %>
  </p>
 
  <p>
    <%= f.label :text %><br>
    <%= f.text_area :text %>
  </p>
 
  <p>
    <%= f.submit %>
  </p>
<% end %>
 
<%= link_to 'Back', posts_path %>
{% endhighlight %}

Define update in app/controllers/posts_controller.rb.
	
{% highlight ruby %}
def update
  @post = Post.find(params[:id])
 
  if @post.update(params[:post].permit(:title, :text))
    redirect_to @post
  else
    render 'edit'
  end
end
{% endhighlight %}

Update app/views/posts/index.html.erb.
	
{% endhighlight %} erb
<table>
  <tr>
    <th>Title</th>
    <th>Text</th>
    <th></th>
    <th></th>
  </tr>
 
<% @posts.each do |post| %>
  <tr>
    <td><%= post.title %></td>
    <td><%= post.text %></td>
    <td><%= link_to 'Show', post_path(post) %></td>
    <td><%= link_to 'Edit', edit_post_path(post) %></td>
  </tr>
<% end %>
</table>
{% endhighlight %}

And in app/views/posts/show.html.erb.
	
{% endhighlight %} erb
...
<%= link_to 'Back', posts_path %>
| <%= link_to 'Edit', edit_post_path(@post) %>
{% endhighlight %}

### Typo in the website
	
{% highlight ruby %}
# views/posts/edit.html.erb
<%= form_for :post, url: post_path(@post.id) },
# should be 
<%= form_for :post, url: post_path(@post.id),

# views/posts/index.html.erb
<td><%= link_to 'Show', post_path %></td>
# should be 
<td><%= link_to 'Show', post_path(post) %></td>

{% endhighlight %}

## Using partials

Create a new file app/views/posts/_form.html.erb. 
	
{% endhighlight %} erb
<%= form_for @post do |f| %>
  <% if @post.errors.any? %>
  <div id="errorExplanation">
    <h2><%= pluralize(@post.errors.count, "error") %> prohibited
      this post from being saved:</h2>
    <ul>
    <% @post.errors.full_messages.each do |msg| %>
      <li><%= msg %></li>
    <% end %>
    </ul>
  </div>
  <% end %>
  <p>
    <%= f.label :title %><br>
    <%= f.text_field :title %>
  </p>
 
  <p>
    <%= f.label :text %><br>
    <%= f.text_area :text %>
  </p>
 
  <p>
    <%= f.submit %>
  </p>
<% end %>
{% endhighlight %}

Update the app/views/posts/new.html.erb
	
{% endhighlight %} erb
<h1>New post</h1>
 
<%= render 'form' %>
 
<%= link_to 'Back', posts_path %>
{% endhighlight %}

And update app/views/posts/edit.html.erb view:
	
{% endhighlight %} erb
<h1>Edit post</h1>
 
<%= render 'form' %>
 
<%= link_to 'Back', posts_path %>
{% endhighlight %}

Add destroy to posts_controller.rb
	
{% highlight ruby %}
def destroy
  @post = Post.find(params[:id])
  @post.destroy
 
  redirect_to posts_path
end
{% endhighlight %}

Add destroy link to app/views/posts/index.html.erb.
	
{% endhighlight %} erb
<h1>Listing Posts</h1>
<table>
  <tr>
    <th>Title</th>
    <th>Text</th>
    <th></th>
    <th></th>
    <th></th>
  </tr>
 
<% @posts.each do |post| %>
  <tr>
    <td><%= post.title %></td>
    <td><%= post.text %></td>
    <td><%= link_to 'Show', post_path %></td>
    <td><%= link_to 'Edit', edit_post_path(post) %></td>
    <td><%= link_to 'Destroy', post_path(post), method: :delete, data: { confirm: 'Are you sure?' } %></td>
  </tr>
<% end %>
</table>
{% endhighlight %}

### Typo
	
{% endhighlight %} erb
# Don'd add any extra space in 

    <td><%= link_to 'Destroy', post_path(post),
                    method: :delete, data: { confirm: 'Are you sure?' } %></td>

# Should be

    <td><%= link_to 'Destroy', post_path(post),method: :delete, data: { confirm: 'Are you sure?' } %></td>
{% endhighlight %}

## Second model
	
{% highlight text %}
$ rails generate model Comment commenter:string body:text post:references
{% endhighlight %}

This command creates four files.
db/migrate/20100207235629_create_comments.rb,
app/models/comment.rb,
test/models/comment_test.rb,
test/fixtures/comments.yml

Run migration.
	
{% highlight text %}
$ rake db:migrate
{% endhighlight %}

Edit app/models/post.rb
	
{% highlight ruby %}
class Post < ActiveRecord::Base
	has_many :comments
...
{% endhighlight %}

## Adding a Route for Comments

config/routes.rb
	
{% highlight ruby %}
resources :posts do
  resources :comments
end
{% endhighlight %}

This creates comments as a nested resource within posts.

## Generating a comments controller
	
{% highlight text %}
$ rails g controller Comments
{% endhighlight %}

This command creates the following files.  

app/controllers/comments_controller.rb  
app/views/comments/  
test/controllers/comments_controller_test.rb  
app/helpers/comments_helper.rb  
test/helpers/comments_helper_test.rb  
app/assets/javascripts/comment.js.coffee  
app/assets/stylesheets/comment.css.scss  

Edit app/views/posts/show.html.erb for comment.
	
{% endhighlight %} erb
<p>
  <strong>Title:</strong>
  <%= @post.title %>
</p>
 
<p>
  <strong>Text:</strong>
  <%= @post.text %>
</p>
 
<h2>Add a comment:</h2>
<%= form_for([@post, @post.comments.build]) do |f| %>
  <p>
    <%= f.label :commenter %><br />
    <%= f.text_field :commenter %>
  </p>
  <p>
    <%= f.label :body %><br />
    <%= f.text_area :body %>
  </p>
  <p>
    <%= f.submit %>
  </p>
<% end %>
 
<%= link_to 'Edit Post', edit_post_path(@post) %> |
<%= link_to 'Back to Posts', posts_path %>
{% endhighlight %}

Edit app/controllers/comments_controller.rb:
	
{% highlight ruby %}
class CommentsController < ApplicationController
  def create
    @post = Post.find(params[:post_id])
    @comment = @post.comments.create(params[:comment].permit(:commenter, :body))
    redirect_to post_path(@post)
  end
end
{% endhighlight %}

Edit  app/views/posts/show.html.erb to show comments.
	
{% endhighlight %} erb
<p>
  <strong>Title:</strong>
  <%= @post.title %>
</p>
 
<p>
  <strong>Text:</strong>
  <%= @post.text %>
</p>
 
<h2>Comments</h2>
<% @post.comments.each do |comment| %>
  <p>
    <strong>Commenter:</strong>
    <%= comment.commenter %>
  </p>
 
  <p>
    <strong>Comment:</strong>
    <%= comment.body %>
  </p>
<% end %>
 
<h2>Add a comment:</h2>
<%= form_for([@post, @post.comments.build]) do |f| %>
  <p>
    <%= f.label :commenter %><br />
    <%= f.text_field :commenter %>
  </p>
  <p>
    <%= f.label :body %><br />
    <%= f.text_area :body %>
  </p>
  <p>
    <%= f.submit %>
  </p>
<% end %>
 
<%= link_to 'Edit Post', edit_post_path(@post) %> |
<%= link_to 'Back to Posts', posts_path %>
{% endhighlight %}

## Refactoring

Create app/views/comments/_comment.html.erb and put the following into it:

{% endhighlight %} erb
<p>
  <strong>Commenter:</strong>
  <%= comment.commenter %>
</p>
 
<p>
  <strong>Comment:</strong>
  <%= comment.body %>
</p>
{% endhighlight %}

Create app/views/comments/_form.html.erb containing:

{% endhighlight %} erb
<%= form_for([@post, @post.comments.build]) do |f| %>
  <p>
    <%= f.label :commenter %><br />
    <%= f.text_field :commenter %>
  </p>
  <p>
    <%= f.label :body %><br />
    <%= f.text_area :body %>
  </p>
  <p>
    <%= f.submit %>
  </p>
<% end %>
{% endhighlight %}

And edit app/views/posts/show.html.erb look like the following:

{% endhighlight %} erb
<p>
  <strong>Title:</strong>
  <%= @post.title %>
</p>
 
<p>
  <strong>Text:</strong>
  <%= @post.text %>
</p>
 
<h2>Comments</h2>
<%= render @post.comments %>
 
<h2>Add a comment:</h2>
<%= render "comments/form" %>
 
<%= link_to 'Edit Post', edit_post_path(@post) %> |
<%= link_to 'Back to Posts', posts_path %>
{% endhighlight %}

## Deleting comments
Add to app/views/comments/_comment.html.erb partial:

{% endhighlight %} erb
<p>
  <strong>Commenter:</strong>
  <%= comment.commenter %>
</p>
 
<p>
  <strong>Comment:</strong>
  <%= comment.body %>
</p>
 
<p>
  <%= link_to 'Destroy Comment', [comment.post, comment],
               method: :delete,
               data: { confirm: 'Are you sure?' } %>
</p>

{% endhighlight %}

Edit app/controllers/comments_controller.rb:

{% highlight ruby %}
class CommentsController < ApplicationController
 
  def create
    @post = Post.find(params[:post_id])
    @comment = @post.comments.create(params[:comment])
    redirect_to post_path(@post)
  end
 
  def destroy
    @post = Post.find(params[:post_id])
    @comment = @post.comments.find(params[:id])
    @comment.destroy
    redirect_to post_path(@post)
  end
 
end
{% endhighlight %}

## Deleting posts delete comments

Edit app/models/post.rb, as follows:

{% highlight ruby %}
class Post < ActiveRecord::Base
  has_many :comments, dependent: :destroy
  validates :title, presence: true,
                    length: { minimum: 5 }
  [...]
end
{% endhighlight %}

## Security

Edit app/controllers/posts_controller.rb:

{% highlight ruby %}
class PostsController < ApplicationController
 
  http_basic_authenticate_with name: "dhh", password: "secret", except: [:index, :show]
 
  def index
    @posts = Post.all
  end
 
  # snipped for brevity
{% endhighlight %}

Edit app/controllers/comments_controller.rb) we write:

{% highlight ruby %}
class CommentsController < ApplicationController
 
  http_basic_authenticate_with name: "dhh", password: "secret", only: :destroy
 
  def create
    @post = Post.find(params[:post_id])
    ...
  end
  # snipped for brevity
{% endhighlight %}

