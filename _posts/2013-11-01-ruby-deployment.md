---
layout: post
title: "Ruby Deployment"
date: 2013-11-01 22:10
categories: Ruby
---

`git clone` from https://github.com/tutsplus/rails-deployment-example. Move all the files to the previouse directory. Or cd the dir and start vagrant here.


## Setting up VM
	
Check if you have vagrant.
	
{% highlight text %}
$ which vagrant

# list vagrant box installed
$ vagrant box list

# if you haven't installed any box yet, install it
$ vagrant box add ubuntu32 http://files.vagrantup.com/precise32.box

$ mkdir rubydevelopment && cd $_
$ vagrant init ubuntu32

# Open Vagrantfile 
$ vim Vagrantfile
# In vim
# uncomment config.vm.network :bridged
config.vm.network :bridged
# save and exit
:wq!

$ vagrant up
# It will ask about the network bridge. Select 1 for Wi-Fi

# ssh to VM
$ vagrant ssh

# check the IP address
vagrant@precise32:~$ ip addr
# IP address for the VM is something like
# 192.168.1.93 or 192.168.11.7
# the physical server has the different address.

# update the server
vagrant@precise32:~$ sudo apt-get update

# install
vagrant@precise32:~$ sudo apt-get install build-essential git-core python-software-properties nodejs

# install vim
vagrant@precise32:~$ sudo apt-get install vim

# check the installation
vagrant@precise32:~$ git --version
vagrant@precise32:~$ node -v
# cd to the shared folder
vagrant@precise32:~$ cd /vagrant 
vagrant@precise32:vagrant$ exit 

# shut down vagrant
$ vagrant halt
{% endhighlight %}

## Using rbenv

[Install rbenv](https://github.com/sstephenson/rbenv#installation)
Or using installer.
[rbenv installer](https://github.com/fesplugas/rbenv-installer)

Installing Ruby build and server using rbenv installer.

{% highlight text %}
$ vagarnt up
$ vagrant ssh
vagrant@precise32:~$ curl https://raw.github.com/fesplugas/rbenv-installer/master/bin/rbenv-installer | bash

# it may ask to install curl
vagrant@precise32:~$ sudo apt-get install curl
# try it again
vagrant@precise32:~$ curl https://raw.github.com/fesplugas/rbenv-installer/master/bin/rbenv-installer | bash

# Read the output about adding rbenv to the load path
vagrant@precise32:~$ vim ~/.bashrc

# Copy and paste at the top of .bashrc. It must be at the top. After # for examples

export RBENV_ROOT="${HOME}/.rbenv"

if [ -d "${RBENV_ROOT}" ]; then
  export PATH="${RBENV_ROOT}/bin:${PATH}"
  eval "$(rbenv init -)"
fi

# save and close it.
:wq!

# load source .bashrc again
vagrant@precise32:~$ source .bashrc

# check rbenv
vagrant@precise32:~$ rbenv

# to see installed versions
$ rbenv versions

# install ruby 2.0.0
vagrant@precise32:~$ rbenv install 2.0.0-p0
# or
vagrant@precise32:~$ rbenv install 2.0.0-p247

# rehash after installing a ruby
vagrant@precise32:~$ rbenv rehash

# check the default ruby
vagrant@precise32:~$ ruby -v

# change the ruby version
vagrant@precise32:~$ rbenv global 2.0.0-p247
# or
vagrant@precise32:~$ rbenv local 2.0.0-p247
# or
vagrant@precise32:~$ cd /vagrant
vagrant@precise32:vagrant$ cat .ruby-version
2.0.0-p247
# if there is no .ruby-version create it
vagrant@precise32:vagrant$ vim .ruby-version
# and add the version you want
2.0.0-p247

# install bundler
vagrant@precise32:vagrant$ gem install bundler

# run bundle to install all dependencies for the project
vagrant@precise32:vagrant$ bundle

# If you have a problem to install pg, An error occurred while installing pg (0.15.0), install libpq-dev
vagrant@precise32:vagrant$ sudo apt-get install libpq-dev 
vagrant@precise32:vagrant$ gem install pg -v '0.15.0'

# If you get an error installing any gems, run sudo apt-get install ...
# for example installing sqlite3
vagrant@precise32:vagrant$ sudo apt-get install sqlite3 libsqlite3-dev

# run bundle again
vagrant@precise32:vagrant$ bundle

# rehash again after installing gem. otherwise it won't be available
vagrant@precise32:vagrant$ rbenv rehash

# bundling without production packages
vagrant@precise32:vagrant$ bundle --without production

# check your ip address
vagrant@precise32:vagrant$ ip addr
# my case is 192.168.11.8:3000

vagrant@precise32:vagrant$ bundle exec rake db:create
vagrant@precise32:vagrant$ bundle exec rake db:migrate
vagrant@precise32:vagrant$ bundle exec rails server

# In your host computer(not VM) go to http://192.168.11.8:3000/

{% endhighlight %}

## Installing Web server nginx
	
{% highlight text %}
# add repository ppa:nginx/stable
vagrant@precise32:vagrant$ sudo apt-add-repository ppa:nginx/stable

# update since you have a new source
vagrant@precise32:vagrant$ sudo apt-get update

# install nginx
vagrant@precise32:vagrant$ sudo apt-get install nginx

# check if nginx is installed
vagrant@precise32:vagrant$ sudo service nginx status
* could not access PID file for nginx

# at the moment it is not running. Start nginx
vagrant@precise32:vagrant$ sudo service nginx start

# you can restart nginx with 
sudo service nginx restart

# check ip address
vagrant@precise32:vagrant$ ip addr
# my case is 192.168.11.8
# Go to host browser and check 192.168.11.8
# It should give a nginx home page. 

# Check this index page
vagrant@precise32:vagrant$ cat /usr/share/nginx/html/index.html
{% endhighlight %}

## Problem with failed message 

(Don't do this if you don't have any problems)

I didn't have this problem. If you have the following after starting you need to config.

{% highlight text %}
nginx: [emerg] bind() to [::]:80 failed (98: Address already in use)
# nginx is bound to ipv6 as well.
vagrant@precise32:vagrant$ sudo vim /etc/nginx/nginx.conf
{% endhighlight %}

Notice in nginx.conf about error log location, 

access_log /var/log/nginx/access.log;
error_log /var/log/nginx/error.log;

include /etc/nginx/conf.d/*.conf;
include /etc/nginx/sites-enabled/*

Open /etc/nginx/sites-enabled/ in vim.
	
{% highlight text %}
vagrant@precise32:vagrant$ sudo vim /etc/nginx/sites-enabled/default

# comment listen [::]:80 default_server; This part enable ipv6 server which this case you don't need if you have failed message when you start nginx.
# listen [::]:80 default_server;
{% endhighlight %}

nginx index page is located at /usr/share/nginx/html/index.html.


## Application server unicorn
The following is in the host server which means you don't vagrant ssh yet. Testing in the development, inside the local machine.

Add gem 'unicorn' to Gemfile.
	
{% highlight ruby %}
...
gem 'unicorn'
...
{% endhighlight %}

Then run bundle.
	
{% highlight text %}
$ bundle
{% endhighlight %}

If you start rails server it starts WEBrick not unicorn. You need a config file and explicit call to unicorn.
	
{% highlight text %}
$ bundle exec rails s
=> Booting WEBrick
...
{% endhighlight %}

Open your railsapp/config and add unicorn.rb. Add the following to this file.
 This gives the root of this app and current file.

{% highlight ruby %}
working_directory File.expand_path("../..", __FILE__)

# This is the same as followings.
APP_PATH = "/vagrant"
working_directory APP_PATH
{% endhighlight %}

Run unicorn.
	
{% highlight text %}
$ bundle exec unicorn -c config/unicorn.rb
{% endhighlight %}

Visit http://localhost:8080/ to see it is working.

You can improve unicorn config. worker_processes, listen, timeout, pid, stdout_path and stderr_path.

nginx can communicate through socket.

pid stands for process id. pid specifies pid file for unicorn. If you are running more than one app, then give detailed name. This pid file stores the process id for the app. This will be handy for the deployment. Since process id is not fixed this is convenient way to store the process id. When you reboot, you know which process to kill.

If you have stdout_path, there will be no outputs on the termainal. It will be on the stdout_path log file. Error as well.

The following is for the development. Not in the vagrant/production server.

{% highlight ruby %}
working_directory File.expand_path("../..", __FILE__)
# APP_PATH = "/vagrant"
# working_directory APP_PATH
worker_processes 2
# listen "/tmp/unicorn.sock"
timeout 30

# for development
pid "/tmp/unicorn_rails3demo.pid"
stdout_path "/Users/teacher/projects/rails3demo/log/unicorn.log"
stderr_path "/Users/teacher/projects/rails3demo/log/unicorn.log"

# for production
# pid "/vagrant/tmp/pid/unicorn.pid"
# stdout_path "/vagrant/log/unicorn.log"
# stderr_path "/vagrant/log/unicorn.log"

# real production
# pid "/tmp/pid/unicorn.pid"
{% endhighlight %}

In order to suits the production

	
{% highlight text %}
$ vagrant up 
$ vagrant ssh
$ cd /vagrant

# update /config/unicorn.rb

$ vim config/unicorn.rb

working_directory File.expand_path("../..", __FILE__)
worker_processes 2
# listen "/tmp/unicorn.sock"
timeout 30

# for development
# pid "/tmp/unicorn_rails3demo.pid"
# stdout_path "/Users/teacher/projects/rails3demo/log/unicorn.log"
# stderr_path "/Users/teacher/projects/rails3demo/log/unicorn.log"

# for production
pid "/vagrant/tmp/pids/unicorn.pid"
stdout_path "/vagrant/log/unicorn.log"
stderr_path "/vagrant/log/unicorn.log"

# save it and run unicorn.rb
$ bundle exec unicorn -c config/unicorn.rb
{% endhighlight %}

Check ip addr. Mine is 192.168.11.8. So go to http://192.168.11.8:3000/.

	
{% highlight text %}
# to run unicorn in the real production
$ bundle exec unicorn -c config/unicorn.rb -E production
{% endhighlight %}

## Setting up database

[Ubuntu PPA](http://www.postgresql.org/download/linux/ubuntu/)
[PostgreSQL backports for stable Ubuntu releases](https://launchpad.net/~pitti/+archive/postgresql)
{% highlight text %}
$ vagrant up
$ vagrant ssh
# we don't have a latest version of postgresql in the official repository. Search it.
vagrant@precise32:~$ sudo aptitude search postgresql
vagrant@precise32:~$ sudo apt-add-repository ppa:pitti/postgresql
# -y update without confirmation
vagrant@precise32:~$ sudo apt-get -y update 
(# list all postgresql, not working)
(vagrant@precise32:~$ sudo apt-get install postgresql )
# check the latest online and select the latest and libraries
vagrant@precise32:~$ sudo apt-get install postgresql-9.2 libpq-dev

vagrant@precise32:~$ sudo service postgresql status
9.2/main (port 5432): online
# you need to integrate postgresql with rails
# postgre has a user called postgre. Use it to create a new role for our db.
# vagrant control the rails, so create a user vagrant and give the power to create a db.
# switch to postgre user and run postgre shell
# -c allow to run command after logging in.
vagrant@precise32:~$ sudo su -c psql postgres
psql (9.2.4)
Type "help" for help.

# use a single quote for password
postgres=# create role vagrant with password 'secret';
CREATE ROLE
# change to vagrant
postgres=# alter role vagrant createdb;
ALTER ROLE
# ctrl+d to exit or use \q
postgres=# \q
{% endhighlight %}

## Integrating unicorn, nginx and postgresql manually
This didn't work for me.

Boot the application in the production mode.
	
{% highlight text %}
# tweak nginx config
# update nginx to interact with unicorn, so that nginx can serve public assets and proxy the call to rails server when it is needed.
# open standard config for nginx
vagrant@precise32:~$ sudo touch /etc/nginx/sites-enabled/rails3demo
vagrant@precise32:~$ sudo vim /etc/nginx/sites-enabled/rails3demo

{% endhighlight %}

In /etc/nginx/sites-enabled/rails3demo add the followings.

{% highlight text %}
upstream unicorn {
  server unix:/tmp/unicorn.sock fail_timeout=0;
}

server {
  listen 80 default_server;
  
  root /vagrant/public/;
  
  error_page 404 /404.html;
  error_page 500 502 503 504 /50x.html;
  
  location / {
    try_files $uri/index.html $uri @unicorn;
    # http://192,168.1.93/images
  }

  location @unicorn {
    proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    proxy_set_header Host $http_host;
    proxy_redirect off;
    proxy_pass http://unicorn;
  }

}
{% endhighlight %}


Restart nginx.
	
{% highlight text %}

vagrant@precise32:~$ sudo service nginx restart
{% endhighlight %}

Go to a browser in the host and type ip address. It will give 502 or error. Because unicorn instance is not running.

	
{% highlight text %}
vagrant@precise32:~$ vim /vagrant/config/unicorn.rb
# and comment out stdout_path and stderr_path

vagrant@precise32:~$ cd /vagrant/
# add -e production so that production environment will run
vagrant@precise32:~$ bundle exec unicorn -c config/unicorn.rb -E production

# IF you have an error for the postgresql adapter
vagrant@precise32:~$ bundle install --without development test
# You should have pg in gem list by now

# the browser gives an error, then go to log/production.log
vagrant@precise32:~$ vim log/production.log
# It may say not sending password in config/databse.yml
vagrant@precise32:~$ vim config/database.yml

{% endhighlight %}

Add the following to config/database.yml
	
{% highlight ruby %}
development:
  adapter: sqlite3
  database: db/development.sqlite3

# Warning: The database defined as "test" will be erased and
# re-generated from your development database when you run "rake".
# Do not set this db to the same as development or production.
test:
  adapter: sqlite3
  database: db/test.sqlite3
  pool: 5
  timeout: 5000

production:
  adapter: postgresql
  database: rails3demo
  host: localhost
  password: secret
{% endhighlight %}

Restart the server.
	
{% highlight text %}
vagrant@precise32:~$ bundle exec unicorn -c config/unicorn.rb -E production
{% endhighlight %}

Allow vagrant role is not permitted to login. Fix this.
	
{% highlight text %}
vagrant@precise32:~$ sudo su -c psql postgres
postgres=# alter role vagrant login;
ALTER ROLE
# exit ctrl+d or \q
postgres=# \q
{% endhighlight %}

Check the browser. Gives an error. Check the log/production.log. It doesn't have the rails3demo database.
	
{% highlight text %}
vagrant@precise32:~$ sudo su -c psql postgres
# database name and the owner 
postgres=# create database rails3demo owner vagrant;
CREATE DATABASE
postgres=# \q

vagrant@precise32:~$ RAILS_ENV=production bundle exec rake db:migrate

# after success of migration
# run unicron command and this is how to run it in a production environment
vagrant@precise32:~$ bundle exec unicorn -c config/unicorn.rb -E production

{% endhighlight %}

It gives an error in a browser. Application.css isn't precompiled.

	
{% highlight text %}
vagrant@precise32:~$ bundle exec rake assets:precompile

vagrant@precise32:~$ bundle exec unicorn -c config/unicorn.rb -E production
{% endhighlight %}

This will create assets dir in public dir.

Then you can check the browser and it is supposed to work. But for me it didn't.

## Capistrano Basics

Capistrano 3.0.1 is ready, but I am use 2.14.2.

{% highlight text %}
# install capistrano
$ mkdir capy
$ cd capy
$ rbenv local 2.0.0-p247
$ ls -la
$ sudo gem install capistrano -v 2.14.2
# for the latest, but not for this.
# $ gem install capistrano
$ rbenv rehash
# for Capistrano 2.x
$ capify .
add] writing './Capfile'
[add] making directory './config'
[add] writing './config/deploy.rb'
[done] capified!

# for Capistrano 3.x
$ cap install
mkdir -p config/deploy
create config/deploy.rb
create config/deploy/staging.rb
create config/deploy/production.rb
mkdir -p lib/capistrano/tasks
Capified
# to list cap commands
$ cap -vT
{% endhighlight %}

Open config/deploy.rb with vim and delete all and add the followings.

Adding cap status to cap task.

{% highlight ruby %}
# capy/config/deploy.rb
task :status do
  run "service nginx status"
end
{% endhighlight %}

Check at the end, 
	
{% highlight text %}
$ cap -vT
cap deploy                   # Deploys your project.
...
cap shell                    # Begin an interactive Capistrano session.
cap status                   #
{% endhighlight %}

Now run cap status
	
{% highlight text %}
$ cap status
  * 2013-11-03 16:13:19 executing `status'
  * executing "service nginx status"
`status' is only run for servers matching {}, but no servers matched
{% endhighlight %}

We need to define a server on top of which we need to run the command. Set up the host, which is the vagrant server.

{% highlight ruby %}
# capy/config/deploy.rb
task :status, hosts: "192.168.11.5" do
  run "service nginx status"
end
{% endhighlight %}

Move to the project file and vagrant up.
	
{% highlight text %}
$ cd ..
$ cd rails-deployment-example
$ vagrant up
# go back to the capy folder
$ cd ..
$ cd capy
$ cap status
  * 2013-11-03 16:26:00 executing `status'
  * executing "service nginx status"
    servers: ["192.168.11.5"]
Password: 
# if it asks password, use vagrant
onnection failed for: 192.168.11.5 (Net::SSH::AuthenticationFailed: teacher)
# if connection failed then set up a new user vagrant in capy/config/deploy.rb and use it.
{% endhighlight %}

	
{% highlight ruby %}
set :user, "vagrant"
task :status, hosts: "192.168.11.5" do
  run "service nginx status"
end
{% endhighlight %}

	
{% highlight text %}
$ cat status
    [192.168.11.5] executing command
 ** [out :: 192.168.11.5] * nginx is running
    command finished in 182ms
{% endhighlight %}

Setting postgresql status.

{% highlight ruby %}
set :user, "vagrant"
task :status, hosts: "192.168.11.5" do
  run "service nginx status"
  run "service postgresql status"  
end
{% endhighlight %}

Run cap status.
	
{% highlight text %}
➜  capy  cap status
  * 2013-11-03 16:35:20 executing `status'
  * executing "service nginx status"
    servers: ["192.168.11.5"]
Password:
    [192.168.11.5] executing command
 ** [out :: 192.168.11.5] * nginx is running
    command finished in 81ms
  * executing "service postgresql status"
    servers: ["192.168.11.5"]
    [192.168.11.5] executing command
 ** [out :: 192.168.11.5] 9.2/main (port 5432): online
    command finished in 407ms
{% endhighlight %}

Isolate each task.
	
{% highlight ruby %}
set :user, "vagrant"
task :nginx_status, hosts: "192.168.11.5" do
  run "service nginx status"
end

task :pg_status, hosts: "192.168.11.5" do
  run "service postgresql status"  
end
{% endhighlight %}

Run cap nginx_status, pg_status or status.
	
{% highlight text %}
➜  capy  cap nginx_status

{% endhighlight %}

If you are using VPS, you may have different ip address for app, web and db.
	
{% highlight ruby %}
set :user, "vagrant"

# role :app, "www.myproject.com"
role :app, "192.168.11.5"
role :web, "192.168.11.5"
role :db, "192.168.11.5"


task :nginx_status, hosts: "192.168.11.5" do
  run "service nginx status"
end

task :pg_status, hosts: "192.168.11.5" do
  run "service postgresql status"  
end
{% endhighlight %}

Short-hand for this. You need to pass primary: true for your db, in order to have a master db setup running from there. This is required even if you don't have a master-slave application.
	
{% highlight ruby %}
set :user, "vagrant"

server "192.168.11.5", :app, :web, :db, primary: true

task :nginx_status, roles: :web do
  run "service nginx status"
end

task :pg_status, roles: :db do
  run "service postgresql status"  
end
{% endhighlight %}

Check if it still works.
	
{% highlight text %}
➜  capy  cap nginx_status
  * 2013-11-03 17:00:11 executing `nginx_status'
  * executing "service nginx status"
    servers: ["192.168.11.5"]
Password:
    [192.168.11.5] executing command
 ** [out :: 192.168.11.5] * nginx is running
    command finished in 80ms
➜  capy  cap pg_status
  * 2013-11-03 17:00:21 executing `pg_status'
  * executing "service postgresql status"
    servers: ["192.168.11.5"]
Password:
    [192.168.11.5] executing command
 ** [out :: 192.168.11.5] 9.2/main (port 5432): online
    command finished in 117ms
{% endhighlight %}

## Using Capistrano for deployment 1

Create a new dir and capify.
	
{% highlight text %}
➜  ~  mkdir capy2 && $_
➜  capy2  capify .
[add] writing './Capfile'
[add] making directory './config'
[add] writing './config/deploy.rb'
[done] capified!
➜  capy2
{% endhighlight %}


Open Capfile. And uncomment load 'deploy/assets'
	
{% highlight ruby %}
load 'deploy'
# Uncomment if you are using Rails' asset pipeline
load 'deploy/assets'
load 'config/deploy' # remove this line to skip loading any of the default tasks
{% endhighlight %}

Open rails-deployment-example/config/deploy.rb and set the followings.
	
{% highlight ruby %}
set :application, "rails3demo"
set :repository,  "git://github.com/josemota/rails3demo"
set :user, "vagrant"
set :scm, :git 
# check with ip addr in VM
server "92.168.11.5", :app, :web, :db, primary: true
set :user, "vagrant"
{% endhighlight %}

### cap deploy:check

{% highlight text %}
➜  capy2  cap -vT
...
cap deploy:check        # Test deployment dependencies.
...
➜  capy2  cap deploy:check
  * 2013-11-03 19:59:11 executing `deploy:check'
  * executing "test -d /home/vagrant/apps/rails3demo/releases"
    servers: ["192.168.11.5"]
Password:
# use 'vagrant' for password
    [192.168.11.5] executing command
    command finished in 70ms
  * executing "test -w /u/apps/rails3demo"
    servers: ["192.168.11.5"]
    [192.168.11.5] executing command
    command finished in 71ms
  * executing "test -w /u/apps/rails3demo/releases"
    servers: ["192.168.11.5"]
    [192.168.11.5] executing command
    command finished in 67ms
  * executing "which git"
    servers: ["192.168.11.5"]
    [192.168.11.5] executing command
    command finished in 69ms
The following dependencies failed. Please check them and try again:
--> `/u/apps/rails3demo/releases' does not exist. Please run `cap deploy:setup'. (192.168.11.5)
--> You do not have permissions to write to `/u/apps/rails3demo'. (192.168.11.5)
--> You do not have permissions to write to `/u/apps/rails3demo/releases'. (192.168.11.5)
{% endhighlight %}

Dependencies failed. /u/apps/rails3demo/releases is strange dir. In order to prevent permission errors, set deploy_to in deploy.rb. We set a folder in which we have a permissions. set :deploy_to, "/home/vagrant/apps/rails3demo"
	
{% highlight ruby %}
set :application, "rails3demo"
set :repository,  "git://github.com/josemota/rails3demo"
set :user, "vagrant"
set :deploy_to, "/home/vagrant/apps/rails3demo"
# set :use_sudo, false

set :scm, :git # You can set :scm explicitly or Capistrano will make an intelligent guess based on known version control directory names
# Or: `accurev`, `bzr`, `cvs`, `darcs`, `git`, `mercurial`, `perforce`, `subversion` or `none`

server "192.168.11.5", :app, :web, :db, primary: true

{% endhighlight %}

Now deploy:setup. This will creat necessary dir and files.
	
{% highlight text %}
➜  capy2  cap deploy:setup
  * 2013-11-03 20:14:35 executing `deploy:setup'
  * executing "sudo -p 'sudo password: ' mkdir -p /home/vagrant/apps/rails3demo /home/vagrant/apps/rails3demo/releases /home/vagrant/apps/rails3demo/shared /home/vagrant/apps/rails3demo/shared/system /home/vagrant/apps/rails3demo/shared/log /home/vagrant/apps/rails3demo/shared/pids"
    servers: ["192.168.11.5"]
Password:
    [192.168.11.5] executing command
    command finished in 81ms
  * executing "sudo -p 'sudo password: ' chmod g+w /home/vagrant/apps/rails3demo /home/vagrant/apps/rails3demo/releases /home/vagrant/apps/rails3demo/shared /home/vagrant/apps/rails3demo/shared/system /home/vagrant/apps/rails3demo/shared/log /home/vagrant/apps/rails3demo/shared/pids"
    servers: ["192.168.11.5"]
    [192.168.11.5] executing command
    command finished in 71ms
➜  capy2
{% endhighlight %}

Now run cap deploy:check again.
	
{% highlight text %}
➜  capy2  cap deploy:check
  * 2013-11-03 20:18:30 executing `deploy:check'
  * executing "test -d /home/vagrant/apps/rails3demo/releases"
    servers: ["192.168.11.5"]
Password:
    [192.168.11.5] executing command
    command finished in 82ms
  * executing "test -w /home/vagrant/apps/rails3demo"
    servers: ["192.168.11.5"]
    [192.168.11.5] executing command
    command finished in 60ms
  * executing "test -w /home/vagrant/apps/rails3demo/releases"
    servers: ["192.168.11.5"]
    [192.168.11.5] executing command
    command finished in 119ms
  * executing "which git"
    servers: ["192.168.11.5"]
    [192.168.11.5] executing command
    command finished in 203ms
The following dependencies failed. Please check them and try again:
--> You do not have permissions to write to `/home/vagrant/apps/rails3demo'. (192.168.11.5)
--> You do not have permissions to write to `/home/vagrant/apps/rails3demo/releases'. (192.168.11.5)
➜  capy2
{% endhighlight %}

Dependencies failed with permissions error.
In vagrant machine,
	
{% highlight text %}
vagrant@precise32:~$ ls -l
drwxr-xr-x 3 root    root    4096 Nov  3 11:14 apps
...
{% endhighlight %}

apps folder has group and owner to root. We need to have app's group to vagrant. In order to solve this, add :use_sudo, false to deploy.rb. This makes vagrant user creates everything. And we are not using sudo.

Make it sure that you create your github repository and git push it.
	
{% highlight ruby %}
set :application, "rails3demo"
set :repository,  "git://github.com/shinokada/rails-deployment-example"
set :user, "vagrant"
set :deploy_to, "/home/vagrant/apps/rails3demo"
set :use_sudo, false

set :scm, :git # You can set :scm explicitly or Capistrano will make an intelligent guess based on known version control directory names
# Or: `accurev`, `bzr`, `cvs`, `darcs`, `git`, `mercurial`, `perforce`, `subversion` or `none`

server "192.168.11.5", :app, :web, :db, primary: true
{% endhighlight %}

Remove apps folder.
	
{% highlight text %}
vagrant@precise32:~$ sudo rm -rf apps
# check if it is removed.
vagrant@precise32:~$ ls -l 
{% endhighlight %}

Run cap deploy:setup again in the terminal.
	
{% highlight text %}
➜  capy2  cap deploy:setup
  * 2013-11-03 20:27:46 executing `deploy:setup'
  * executing "mkdir -p /home/vagrant/apps/rails3demo /home/vagrant/apps/rails3demo/releases /home/vagrant/apps/rails3demo/shared /home/vagrant/apps/rails3demo/shared/system /home/vagrant/apps/rails3demo/shared/log /home/vagrant/apps/rails3demo/shared/pids"
    servers: ["192.168.11.5"]
Password:
    [192.168.11.5] executing command
    command finished in 69ms
  * executing "chmod g+w /home/vagrant/apps/rails3demo /home/vagrant/apps/rails3demo/releases /home/vagrant/apps/rails3demo/shared /home/vagrant/apps/rails3demo/shared/system /home/vagrant/apps/rails3demo/shared/log /home/vagrant/apps/rails3demo/shared/pids"
    servers: ["192.168.11.5"]
    [192.168.11.5] executing command
    command finished in 60ms
{% endhighlight %}

See there is no sudo. Check ls -l in vagrant machine to see the owner and group is assigned to vagrant.
	
{% highlight text %}
vagrant@precise32:~$ ls -l
total 12
drwxrwxr-x 3 vagrant vagrant 4096 Nov  3 11:27 apps
-rwxr-xr-x 1 vagrant vagrant 6487 Sep 14  2012 postinstall.sh
vagrant@precise32:~$
{% endhighlight %}

Now check with cap deploy:check for dependencies checking.
	
{% highlight text %}
➜  capy2  cap deploy:check
  * 2013-11-03 20:30:22 executing `deploy:check'
  * executing "test -d /home/vagrant/apps/rails3demo/releases"
    servers: ["192.168.11.5"]
Password:
    [192.168.11.5] executing command
    command finished in 68ms
  * executing "test -w /home/vagrant/apps/rails3demo"
    servers: ["192.168.11.5"]
    [192.168.11.5] executing command
    command finished in 64ms
  * executing "test -w /home/vagrant/apps/rails3demo/releases"
    servers: ["192.168.11.5"]
    [192.168.11.5] executing command
    command finished in 62ms
  * executing "which git"
    servers: ["192.168.11.5"]
    [192.168.11.5] executing command
    command finished in 65ms
You appear to have all necessary dependencies installed
➜  capy2
{% endhighlight %}

Notice there is no error. Then run cap deploy:update. This will pull all the codes inside the repository and put it in a snapshot under the release folder.
	
{% highlight text %}
➜  capy2  cap deploy:update
  * 2013-11-03 21:57:42 executing `deploy:update'
 ** transaction: start
  * 2013-11-03 21:57:42 executing `deploy:update_code'
    executing locally: "git ls-remote git://github.com/shinokada/rails-deployment-example HEAD"
    command finished in 723ms
  * executing "git clone -q git://github.com/shinokada/rails-deployment-example /home/vagrant/apps/rails3demo/releases/20131103125743 && cd /home/vagrant/apps/rails3demo/releases/20131103125743 && git checkout -q -b deploy 8ee5a81b89ff69aa81d669083920dbbc2b8251a6 && (echo 8ee5a81b89ff69aa81d669083920dbbc2b8251a6 > /home/vagrant/apps/rails3demo/releases/20131103125743/REVISION)"
    servers: ["192.168.11.5"]
Password:
*** [deploy:update_code] rolling back
  * executing "rm -rf /home/vagrant/apps/rails3demo/releases/20131103125743; true"
    servers: ["192.168.11.5"]
 ** [deploy:update_code] exception while rolling back: Capistrano::ConnectionError, connection failed for: 192.168.11.5 (Net::SSH::AuthenticationFailed: vagrant)
{% endhighlight %}

It will execute 'deploy:update'. deploy:update_code' pull from github repository. It clones from the repository.

It pulls the source codes, prepare the release with all the shared configuration. Compile all the assets. Update the current directory location.

## Automatic deployment


## Using Capistrano for deployment 2


## Regular Deployment

















