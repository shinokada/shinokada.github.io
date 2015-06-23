---
layout: post
title: "My first provision"
date: 2013-11-09 09:00
categories: [vagrant, puppet]
---


While I was doing [this](https://github.com/shinokada/rails-deployment-example), I wanted to automate some process. I am not sure how to install rbenv yet, though.

## Note: rails-dev-box

rails-dev-box can be alternative to this.

[rails-dev-box](https://github.com/rails/rails-dev-box)

### What's in box.
	
{% highlight text %}
- Git
- RVM
- Ruby 2.0.0 (binary RVM install)
- Bundler
- SQLite3, MySQL, and Postgres
- System dependencies for nokogiri, sqlite3, mysql, mysql2, and pg
- Databases and users needed to run the Active Record test suite
- Node.js for the asset pipeline
- Memcached
{% endhighlight %}


Vagrantfile from rails-dev-box
	
{% highlight ruby %}
Vagrant.configure('2') do |config|
  config.vm.box      = 'precise32'
  config.vm.box_url  = 'http://files.vagrantup.com/precise32.box'
  config.vm.hostname = 'rails-dev-box'

  config.vm.network :forwarded_port, guest: 3000, host: 3000

  config.vm.provision :puppet do |puppet|
    puppet.manifests_path = 'puppet/manifests'
    puppet.module_path    = 'puppet/modules'
  end
end

{% endhighlight %}


manifests/default.pp from rails-dev-box

{% highlight ruby %}
$ar_databases = ['activerecord_unittest', 'activerecord_unittest2']
$as_vagrant   = 'sudo -u vagrant -H bash -l -c'
$home         = '/home/vagrant'

Exec {
  path => ['/usr/sbin', '/usr/bin', '/sbin', '/bin']
}

# --- Preinstall Stage ---------------------------------------------------------

stage { 'preinstall':
  before => Stage['main']
}

class apt_get_update {
  exec { 'apt-get -y update':
    unless => "test -e ${home}/.rvm"
  }
}
class { 'apt_get_update':
  stage => preinstall
}

# --- SQLite -------------------------------------------------------------------

package { ['sqlite3', 'libsqlite3-dev']:
  ensure => installed;
}

# --- MySQL --------------------------------------------------------------------

class install_mysql {
  class { 'mysql': }

  class { 'mysql::server':
    config_hash => { 'root_password' => '' }
  }

  database { $ar_databases:
    ensure  => present,
    charset => 'utf8',
    require => Class['mysql::server']
  }

  database_user { 'rails@localhost':
    ensure  => present,
    require => Class['mysql::server']
  }

  database_grant { ['rails@localhost/activerecord_unittest', 'rails@localhost/activerecord_unittest2']:
    privileges => ['all'],
    require    => Database_user['rails@localhost']
  }

  package { 'libmysqlclient15-dev':
    ensure => installed
  }
}
class { 'install_mysql': }

# --- PostgreSQL ---------------------------------------------------------------

class install_postgres {
  class { 'postgresql': }

  class { 'postgresql::server': }

  pg_database { $ar_databases:
    ensure   => present,
    encoding => 'UTF8',
    require  => Class['postgresql::server']
  }

  pg_user { 'rails':
    ensure  => present,
    require => Class['postgresql::server']
  }

  pg_user { 'vagrant':
    ensure    => present,
    superuser => true,
    require   => Class['postgresql::server']
  }

  package { 'libpq-dev':
    ensure => installed
  }

  package { 'postgresql-contrib':
    ensure  => installed,
    require => Class['postgresql::server'],
  }
}
class { 'install_postgres': }

# --- Memcached ----------------------------------------------------------------

class { 'memcached': }

# --- Packages -----------------------------------------------------------------

package { 'curl':
  ensure => installed
}

package { 'build-essential':
  ensure => installed
}

package { 'git-core':
  ensure => installed
}

# Nokogiri dependencies.
package { ['libxml2', 'libxml2-dev', 'libxslt1-dev']:
  ensure => installed
}

# ExecJS runtime.
package { 'nodejs':
  ensure => installed
}

# --- Ruby ---------------------------------------------------------------------

exec { 'install_rvm':
  command => "${as_vagrant} 'curl -L https://get.rvm.io | bash -s stable'",
  creates => "${home}/.rvm/bin/rvm",
  require => Package['curl']
}

exec { 'install_ruby':
  # We run the rvm executable directly because the shell function assumes an
  # interactive environment, in particular to display messages or ask questions.
  # The rvm executable is more suitable for automated installs.
  #
  # Thanks to @mpapis for this tip.
  command => "${as_vagrant} '${home}/.rvm/bin/rvm install 2.0.0 --latest-binary --autolibs=enabled && rvm --fuzzy alias create default 2.0.0'",
  creates => "${home}/.rvm/bin/ruby",
  require => Exec['install_rvm']
}

exec { "${as_vagrant} 'gem install bundler --no-rdoc --no-ri'":
  creates => "${home}/.rvm/bin/bundle",
  require => Exec['install_ruby']
}

{% endhighlight %}


## Provision with sh file

Vagrantfile

{% highlight ruby %}
Vagrant.configure("2") do |config|
  config.vm.box = "ubuntu32"
  config.vm.hostname = "vagrant.example.com"
  config.vm.provision :shell, path: './provision.sh'  
  config.vm.network "public_network"
  # config.vm.forward_port 80, 8080
end
{% endhighlight %}

myproject/provision.sh 
	
{% highlight ruby %}
apt-get update
apt-get -y install build-essential git-core python-software-properties nodejs
apt-get -y install vim
apt-get -y install curl
{% endhighlight %}

## Provision with puppet

Vagrantfile

{% highlight ruby %}
Vagrant.configure("2") do |config|
  config.vm.box = "ubuntu32"
  config.vm.hostname = "vagrant.example.com"
  # config.vm.provision :shell, path: './provision.sh'  
  config.vm.network "public_network"
  config.vm.provision :puppet
end
{% endhighlight %}

myproject/manifests/default.pp
	
{% highlight ruby %}

exec { "apt-get update":
    command => "/usr/bin/apt-get update"
}

package { [
    'build-essential',
    'vim',
    'curl',
    'git-core',
    'nodejs',
    'python-software-properties',
    'nginx'
  ]:
  ensure  => 'installed',
}

Exec["apt-get update"] -> Package <| |> 
{% endhighlight %}

Check it.
	
{% highlight text %}
$ vagrant up
$ vagrant ssh
vagrant@vagrant:~$ git --version
vagrant@vagrant:~$ node -v
vagrant@vagrant:~$ which nginx
vagrant@vagrant:~$ sudo service niginx start
# then go to localhost on the hosting(not VM) to see nginx page.
{% endhighlight %}


## Installing rbenv
[Follow this](https://github.com/shinokada/rails-deployment-example#using-rbenv)

## unicorn.rb
Check if the rails project is ok.
	
{% highlight text %}
# in rails project
$ rails s
{% endhighlight %}

Check it at localhost:8080.

config/unicorn.rb

[Resource 1](https://devcenter.heroku.com/articles/rails-unicorn)
[Resource 2](https://github.com/heroku/ruby-rails-unicorn-sample/blob/master/config/unicorn.rb)

{% highlight ruby %}
# config/unicorn.rb

worker_processes Integer(ENV['WEB_CONCURRENCY'] || 3)
timeout Integer(ENV['WEB_TIMEOUT'] || 15)
preload_app true

before_fork do |server, worker|

  Signal.trap 'TERM' do
    puts 'Unicorn master intercepting TERM and sending myself QUIT instead'
    Process.kill 'QUIT', Process.pid
  end

  defined?(ActiveRecord::Base) and
    ActiveRecord::Base.connection.disconnect!
end  

after_fork do |server, worker|

  Signal.trap 'TERM' do
    puts 'Unicorn worker intercepting TERM and doing nothing. Wait for master to sent QUIT'
  end

  defined?(ActiveRecord::Base) and
    ActiveRecord::Base.establish_connection
end
{% endhighlight %}

Then start unicorn server.
	
{% highlight text %}
$ vagrant ssh
vagrant@vagrant: cd /vagrant
# find ip address
vagrant@vagrant: ip addr
vagrant@vagrant:/vagrant$ bundle exec unicorn -c config/unicorn.rb
{% endhighlight %}

Go to http://your ip address:8080/ to check if is running. e.g. http://192.168.11.5:8080/



