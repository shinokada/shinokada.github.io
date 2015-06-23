---
layout: post
title: "Vagrant and Puppet notes"
date: 2013-10-31 14:28
categories: puppet, vagrant
---

Personal notes on Vagrant and Puppet.

## Resources

[Automating Development Environments with Vagrant and Puppet](http://blog.kloudless.com/2013/07/01/automating-development-environments-with-vagrant-and-puppet/)

## Installing Virtual box and vagrant

Install Virtual box and also VM VirtualBox Extension Pack.

{% highlight text %}
$ install vagrant
{% endhighlight %}


## Vagrant Boxes

Adding vagrant box Ubuntu 12.04
	
{% highlight text %}
$ vagrant box add precise32 http://files.vagrantup.com/precise32.box
{% endhighlight %}
	
List all the box.

{% highlight text %}
$ vagrant box list
{% endhighlight %}

Make a dir 

{% highlight text %}	
$ mkdir firstVM
$ cd firstVM
{% endhighlight %}

Initialize vagrant virtual machine with box you want to use.

{% highlight text %}
$ vagrant init precise32
$ ls // to check Vagrantfile
{% endhighlight %}

[Link: Vagrantbox.es](http://www.vagrantbox.es). You can find the url of boxes.

How to remove a box.

{% highlight text %}
$ vagrant box remove lucid32 virtualbox // virtualbox is a provider
// because you can have a differnt box with a different provider.
{% endhighlight %}


## Starting a virtual machine

{% highlight text %}
($ ls firstVM // change the dir where you have your Vagrantfile)
$ vagrant up	
{% endhighlight %}

Check your virtual box that firstVM is running. It is headless, so it does not have window to interact. In stead we use the command line.

How to stop the VM, [Teardown](http://docs.vagrantup.com/v2/getting-started/teardown.html)

{% highlight text %}
$ vagrant suspend
{% endhighlight %}

suspend will save the hard drive and RAM in the VM.

To restart,

{% highlight text %}
$ vagrant resume
{% endhighlight %}

To shut down,

{% highlight text %}
$ vagrant halt
{% endhighlight %}

This won't save RAM, but save the hard drive.

To start,

{% highlight text %}
$ vagrant up
{% endhighlight %}

halt will take up some space in the main hard drive since it will save the VM hard drive.

To destroy the VM when you finished your project. However you can save all the config and files in Vagrantfile. If you setup it properly you don't lose anything.

{% highlight text %}
$ vagrant destroy // y for yes.
{% endhighlight %}


## SSHing into the VM

{% highlight text %}
$ vagrant up
$ vagrant ssh // this will take you to the inside of VM.
{% endhighlight %}

Once you are in ssh, ctrl+D is how to exit ssh.

	
Now in the VM, the command line has vagrant@precise32:~$. (The vagrant@precise32:~$ in the followings are in the VM.)

{% highlight text %}
vagrant@precise32:~$ cd /vagrant
vagrant@precise32:/vagrant ls 
Vagrantfile
{% endhighlight %}

In the /vagrant file, there is the same Vagrantfile as my firstVM dir and they are synced. Also /vagrant dir is a shared dir.

Don't forget to update apt-get and upgrade

{% highlight text %}
vagrant@precise32:~$ sudo apt-get update
vagrant@precise32:~$ sudo apt-get upgrade 
// or
vagrant@precise32:~$ sudo apt-get dist-upgrade
// dist do more than upgrade so all in one with
vagrant@precise32:~$ sudo apt-get update&&sudo apt-get dist-upgrade
{% endhighlight %}

[Link: What does “sudo apt-get update” do?](http://askubuntu.com/questions/222348/what-does-sudo-apt-get-update-do)


### Note

You can add the following to the Vagrantfile to update as well.

{% highlight text %}
# Update the server
config.vm.provision :shell, :inline => "apt-get update --fix-missing"
{% endhighlight %}

[pyrocms Vagrantfile sample](https://github.com/pyrocms/devops-vagrant/blob/master/Vagrantfile)

This line in Vagrantfile should be uncommented.

{% highlight text %}
config.vm.network :forwarded_port, guest: 80, host: 8080
{% endhighlight %}

Let's create a index.html in /vagrant and start a server.

{% highlight text %}
# vagrant@precise32:/vagrant~$ vim index.html // this VM doesn't have vim yet.
# write it without vim
vagrant@precise32:/vagrant~$ echo "<h1> Hi! </h1>" > index.html
vagrant@precise32:/vagrant~$ ls
vagrant@precise32:/vagrant~$ cat index.html
vagrant@precise32:/vagrant~$ sudo python -m SimpleHTTPServer 80
// sudo is needed if you want to choose port 80.
{% endhighlight %}

You have to 'sudo python -m SimpleHTTPServer 80' in /vagrant dir.

This port 80 is redirected to the localhost:8080 in the normal browser. Open your browser and go to [localhost:8080](http://localhost:8080/).

ctrl+C to quit the server. 

And to leave the VM, type exit.

{% highlight text %}
vagrant@precise32:/vagrant~$ exit
{% endhighlight %}


## The Vagrantfile

First destroy the VM in firstVM dir.

{% highlight text %}
vagrant destroy
{% endhighlight %}

The Vagrantfile must be in the root folder of the project. /firstVM/Vagrantfile and it is ruby file. Change the file type to ruby.

Guest in the file means the VM and host means the hosting computer, my computer.

You can change box to others if you need to. And this line should be uncommented.

{% highlight text %}
config.vm.box = "precise32"
{% endhighlight %}

config.vm.network can be changed to guest: 3000, host:3000 or something else. But don't forget to exit the host. Otherwise you can't use that port from other VM. 

And this line should be uncommented.

{% highlight text %}
config.vm.network :forwarded_port, guest: 80, host: 8080
{% endhighlight %}

Uncomment the following line to use the synced folder as specified. But not at the moment.

{% highlight text %}
 config.vm.synced_folder "../data", "/vagrant_data"
{% endhighlight %}


## Provisioning with Shell Scripts

Destroy the VM if it is still running and open the Vagrantfile.

{% highlight text %}
$ vagrant destroy
{% endhighlight %}

Type the following in the Vagrantfile.

{% highlight text %}
config.vm.provision :shell, inline: "echo Hello World!"
{% endhighlight %}

This will output the followings.

{% highlight text %}
...
[default] Running: inline script
stdin: is not a tty
Hello World!
{% endhighlight %}


Don't worry about the line, stdin: is not a tty.

You can rerun the provisioning without restarting the VM. Change the Vagrantfile. 

{% highlight text %}
config.vm.provision :shell, inline: "echo Hello World Aha!"
{% endhighlight %}

And in the terminal, 

{% highlight text %}
$ vagrant provision
{% endhighlight %}

Change the provision script, (this gives erros for me.)

{% highlight text %}
config.vm.provision :shell, path: './provision.sh'	
{% endhighlight %}

Create provision.sh file in the current dir.

{% highlight text %}
echo "This is our provisioning script"
apt-get install vim -y
{% endhighlight %}

-y does not ask yes or no.

Use puppet for more heavy duty tools.


## Getting Started with Puppet

[Puppet Essentials](http://example42.com/tutorials/build/deck/essentials/#slide-0)

{% highlight text %}
$ vagrant up
$ vagrant ssh
vagrant@precise32:~$ which puppet
/opt/vagrant_ruby/bin/puppet
vagrant@precise32:~$ puppet --version
{% endhighlight %}

Some VMs don't have puppet. 

{% highlight text %}
vagrant@precise32:~$ apt-get install puppet
{% endhighlight %}

## Puppet Resources

To see what are in puppet resource, vagrant up and vagrant ssh,

{% highlight text %}
vagrant@precise32:~$ puppet resource user
{% endhighlight %} 

This will show the config for user.

{% highlight text %}
vagrant@precise32:~$ puppet resource user vagrant
{% endhighlight %}

This will show the vagrant user which we are logged in as.

{% highlight text %}
user { 'vagrant':
  ensure  => 'present',
  comment => 'vagrant,,,',
  gid     => '1000',
  groups  => ['adm', 'cdrom', 'sudo', 'dip', 'plugdev', 'lpadmin', 'sambashare', 'admin'],
  home    => '/home/vagrant',
  shell   => '/bin/bash',
  uid     => '1000',
}
{% endhighlight %}

Don't forget to add , at the end as well.

Try another.

{% highlight text %}
vagrant@precise32:~$ puppet resource package vim
package { 'vim':
	ensure => 'purged',
}
// we don't have vim
vagrant@precise32:~$ which vim
// no output means not installed
{% endhighlight %}

Write a puppet script. (Puppet manifest file.)

{% highlight text %}
vagrant@precise32:~$ exit
firstVM $ vim vim.pp
// in Vim 
// :set ft=puppet
// to set the file type to puppet
{% endhighlight %}

In vim.pp

{% highlight text %}
package{ 'vim':
	name => 'vim',
	ensure => 'installed',
}
{% endhighlight %}

In stead of 'installed', you can use 'present' as well.

{% highlight text %}
firstVM $ vagrant ssh
vagrant@precise32:~$ cd /vagrant
vagrant@precise32:/vagrant~$ ls // to check if we have vim.pp file
index.html  provision.sh  Vagrantfile  vim.pp

// run the puppet manifest to install vim as a root so use sudo
vagrant@precise32:/vagrant~$ sudo puppet apply vim.pp
{% endhighlight %}


### error of fqdn

{% highlight text %}
// if you an error telling warning: Could not retrieve fact fqdn
// then add the following to Vagrantfile and destroy and up again
// it should be working with vagrant provision, not so far
config.vm.hostname = "vagrant.example.com"

// from pyrocms
# Enable Puppet
config.vm.provision :puppet do |puppet|
    puppet.facter = { "fqdn" => "local.pyrocms", "hostname" => "www" } 
    puppet.manifests_path = "puppet/manifests"
    puppet.manifest_file  = "ubuntu-apache2-pgsql-php5.pp"
    puppet.module_path  = "puppet/modules"
end
{% endhighlight %}


### not able to install vim

Add the following to Vagrantfile and $ vagrant reload in the terminal

{% highlight text %}
# Update the server
config.vm.provision :shell, :inline => "apt-get update --fix-missing"
{% endhighlight %}

This can be done in this way as well.

{% highlight text %}
config.vm.provision :shell, :path => "bootstrap.sh"
{% endhighlight %}

And write in bootstrap.sh to apt-get update

{% highlight text %}
// /vagrant/bootstrap.sh
#!/usr/bin/env bash

apt-get update
apt-get install -y apache2
rm -rf /var/www
ln -fs /vagrant /var/www
{% endhighlight %}

Use vagrant reload to quick-restart.

Since we installed Apache server and setup the default DocumentRoot of Apache to point to our /vagrant in the bootstrap.sh, we can run this as well.

{% highlight text %}
$ vagrant ssh
vagrant@vagrant:~$ wget -qO- 127.0.01
// this will display index.html which we created before.
<h1> Hi! </h1>
{% endhighlight %}

Check if vim is in.

{% highlight text %}
vagrant@vagrant:/vagrant$ which vim
/usr/bin/vim
vagrant@vagrant:/vagrant$ puppet resource package vim
package { 'vim':
  ensure => '2:7.3.429-2ubuntu2.1',
}
{% endhighlight %}


## Putting Resources in Order

Ordering puppet resource decorations.

{% highlight text %}
$ vagrant ssh
vagrant@vagrant:~$ vim files.pp  // create it in the home dir
{% endhighlight %}

In order to check that resources are not run in order,

In files.pp

{% highlight text %}
file{ 'one':
	path => '/vagrant/one',
	content => 'one',
}

file{ 'two':
	path => '/vagrant/two',
	content => 'two',
}

// save and close it with :wq

vagrant@vagrant:~$ puppet apply files.pp
notice: /Stage[main]//File[two]/ensure: defined content as '{md5}b8a9f715dbb64fd5c56e7783c6820a61'
notice: /Stage[main]//File[one]/ensure: defined content as '{md5}f97c5d29941bfb1b2fdab0874906ab82'
{% endhighlight %}

It started from two as you see above.

Remove one and two

{% highlight text %}
vagrant@vagrant:~$ rm /vagrant/one /vagrant/two
{% endhighlight %}

Using meta parameter to order of resources.

In files.pp

{% highlight text %}
file{ 'one':
	path => '/vagrant/one',
	content => 'one',
	before => File['two'],
}

file{ 'two':
	path => '/vagrant/two',
	content => 'two',
}
{% endhighlight %}

Save and close it with :wq.

{% highlight text %}
vagrant@vagrant:~$ puppet apply files.
notice: /Stage[main]//File[one]/ensure: defined content as '{md5}f97c5d29941bfb1b2fdab0874906ab82'
notice: /Stage[main]//File[two]/ensure: defined content as '{md5}b8a9f715dbb64fd5c56e7783c6820a61'
{% endhighlight %}

Notice file one is created before two.

In the following, the file two's source is the file one. But the file one is not created before the file two, therefore it will give an error. 

In files.pp

{% highlight text %}
file{ 'one':
	path => '/vagrant/one',
	content => 'one',
	# before => File['two'], # comment this out
}

file{ 'two':
	path => '/vagrant/two',
	source => '/vagrant/one',
}
{% endhighlight %}

In the terminal 

{% highlight text %}
vagrant@vagrant:~$ puppet apply files.pp
err: /Stage[main]//File[two]: Could not evaluate: Could not retrieve information from environment production source(s) file:/vagrant/one at /home/vagrant/files.pp:10
notice: /Stage[main]//File[one]/ensure:
...
{% endhighlight %}

In the following file two require one. And give no error.

{% highlight text %}
file{ 'one':
	path => '/vagrant/one',
	content => 'one',
	# before => File['two'], # comment this out
}

file{ 'two':
	path => '/vagrant/two',
	source => '/vagrant/one',
	require => File['one'],
}
{% endhighlight %}

Another way to create a hirachy with ->.

{% highlight text %}
file{ 'one':
	path => '/vagrant/one',
	content => 'one',
}

file{ 'two':
	path => '/vagrant/two',
	source => '/vagrant/one',
}

File['one'] -> File['two']
{% endhighlight %}

Or you can use the opposite direction.

{% highlight text %}
File['two'] <- File['one']
{% endhighlight %}


## Writing a Complete Puppet Manifest

Open the Vagrantfile and change the followings.

{% highlight text %}
# config.vm.provision :shell, path: './provision.sh' //comment
config.vm.provision :puppet // change to this.
{% endhighlight %}

Create dir called manifests under the root dir which is the same as Vagrantfile.

{% highlight text %}
$ mkdir manifests
$ vim manifests/default.pp
{% endhighlight %}


In Vim


{% highlight text %}	
package { 'apache2':
	ensure => 'installed',
}

file { 'site-config':
	path 		=> "/etc/apache2/sites-enabled/000-default",
	source 	=> "/vagrant/site-config",
	require => Package["apache2"],
}

file { '/vagrant/index.html':
	content => "<h1> Vagrant + Puppet </h1>",
}
{% endhighlight %}

The source "/vagrant/site-config" is in the VM.

Open the other tab in the vim.

{% highlight text %}
:tabe site-config

//Copy and pasted the following to site-config
<VirtualHost *:80>
  DocumentRoot /vagrant
  <Directory /vagrant>
    Options Indexes FollowSymLinks MultiViews
    AllowOverride None
    Order allow,deny
    allow from all
  </Directory>
</VirtualHost>
{% endhighlight %}

Save and close the both.

{% highlight text %}
firstVM $ vagrant up
{% endhighlight %}

Check it in the browser at localhost:8080. But it did not render it correctly with "Vagrant + Puppet". Because it install the apache and then site-config. We need to restart the Apache after configure it.

Add the following to manifests/default.pp. This will restart apache2 after reading the site-config.

{% highlight text %}
$ vim manifests/default.pp

service { 'apache2' :
	ensure => 'running',
	hasrestart => true,
	subscribe => File["site-config"]
}
{% endhighlight %}

You need to remove the first /etc/apache2/sites-enabled/000-default

{% highlight text %}
$ vagrant ssh
vagrant@vagrant:~$ sudo rm /etc/apache2/sites-enabled/000-default
vagrant@vagrant:~$ exit

firstVM $ vagrant provision
notice: /Stage[main]//File[site-config]/ensure: defined content as '{md5}469af2b4aee15edd883273c985b41eeb'
notice: /Stage[main]//Service[apache2]: Triggered 'refresh' from 1 events
{% endhighlight %}

This tells that it read the site-config and refreshed the apache.

Now go to localhost:8080 to see the effect.

You don't need to ssh. You can open index.html and edit it from firstVM


## Variables and Conditionals in manifest file

{% highlight text %}
$ vagrant box list
# this didn't work
# $ vagrant box add https://dl.dropbox.com/sh/9rldlpj3cmdtntc/chqwU6EYaZ/centos-63-32bit-puppet.box
// adding centos6.3
# in stead I installed this and it worked
$ vagrant box add centos64 http://puppet-vagrant-boxes.puppetlabs.com/centos-64-x64-vbox4210.box
{% endhighlight %}

In manifests/default.pp, we want to install httpd(web server) when we are operating the centos OS. We need a config path for each OS.
In manifests/default.pp

{% highlight text %}
case $operatingsystem {
	centos: {
		$apache = "httpd"
		$configfile = "/etc/httpd/conf.d/vhost.conf"
	}
	ubuntu: {
		$apache = "apache2"
		$configfile = "/etc/apache2/sites-enabled/000-default"
	}

}

package { 'apache':
	name => $apache, 
	ensure => 'installed',
}

file { 'site-config':
	path 		=> $configfile,
	source 	=> "/vagrant/site-config",
	require => Package["apache"],
}

service { 'apache' :
	name => $apache,
	ensure => 'running',
	hasrestart => true,
	subscribe => File["site-config"]
}

file { '/vagrant/index.html':
	content => "<h1> Vagrant + Puppet + ${apache} (${operatingsystem}) </h1>",
}

if $apache == "httpd" {
	service { 'iptables':
		ensure => 'stopped',
	}
}
{% endhighlight %}

Starting Precise32

{% highlight text %}
// in firstVM dir, start VM
firstVM	$ vagrant up

// after started, check a browser at localhost:8080

firstVM $ cd ..
mkdir centosVM
cd centosVM
centosVM $ vagrant init centos64
centosVM $ vim Vagrantfile
{% endhighlight %}

In Vagrantfile with vim, change :set ft=ruby and uncomment the following and change 8080 to 8081

{% highlight text %}
config.vm.box = "centos64"

config.vm.network :forwarded_port, guest: 80, host:8081

# add the folloings
config.vm.provision :puppet do |puppet|
	puppet.manifests_path = "../firstVM/manifests"
end
{% endhighlight %}

Save and close it. In the terminal copy site-config

{% highlight text %}
centosVM $ cp ../firstVM/site-config .
centosVM $ ls
Vagrantfile site-config
centosVM $ vagrant up
{% endhighlight %}

Open localhost:8081 in a browser to check Vagrant + Puppet + httpd(CentOS)


### note for my case

CentOS6.3 takes a lot of time, so I downloaded centos64 from http://puppet-vagrant-boxes.puppetlabs.com/centos-64-x64-vbox4210.box.

I also added precise64, http://files.vagrantup.com/precise64.box.

{% highlight text %}
$ vagrant box add precise64 http://files.vagrantup.com/precise64.box
{% endhighlight %}


## Facter

{% highlight text %}
cd firstVM
firstVM $ vagrant ssh
vagrant@precise32: ~$ which facter
/opt/vagrant_ruby/bin/facter
vagrant@precise32: ~$  facter // to get different properties
vagrant@precise32: ~$ facter operatingsystem
Ubuntu
vagrant@precise32: ~$ facter is_virtual
true
vagrant@precise32: ~$ exit
{% endhighlight %}

Creating your own facts with facter file. Open the Vagrantfile. Change the provision statement.

{% highlight text %}
config.vm.provision : puppet do |puppet|
	puppet.facter = {
		"key" => "value",
		"database_server" => "included"
	}
end
{% endhighlight %}


## Puppet Classes

Adding Object Oriented class to manifests/default.pp.

In firstVM,

{% highlight text %}
$ vim manifests/default.pp
{% endhighlight %}

In default.pp, create a class and tell the code to run.

{% highlight text %}
class webserver{
	case $operatingsystem {
		centos: {
			$apache = "httpd"
			$configfile = "/etc/httpd/conf.d/vhost.conf"
		}
		ubuntu: {
			$apache = "apache2"
			$configfile = "/etc/apache2/sites-enabled/000-default"
		}

	}

	package { 'apache':
		name => $apache, 
		ensure => 'installed',
	}

	file { 'site-config':
		path 		=> $configfile,
		source 	=> "/vagrant/site-config",
		require => Package["apache"],
	}

	service { 'apache' :
		name => $apache,
		ensure => 'running',
		hasrestart => true,
		subscribe => File["site-config"]
	}

	file { '/vagrant/index.html':
		content => "<h1> Vagrant + Puppet + ${apache} (${operatingsystem}) </h1>",
	}

	if $apache == "httpd" {
		service { 'iptables':
			ensure => 'stopped',
		}
	}
}	
{% endhighlight %}

Move this file to manifests/webserver.pp

{% highlight text %}
$ mv manifests/default.pp manifests/webserver.pp
{% endhighlight %}

Create a new default.pp file

{% highlight text %}
$ vim manifests/default.pp
{% endhighlight %}

In default.pp

{% highlight text %}
class{ "webserver": }
{% endhighlight %}

Save and close it. Run vagrant.

{% highlight text %}
$ vagrant up
{% endhighlight %}

To see if it runs.

Shorter default.pp

{% highlight text %}
# class{ "webserver": }
include webserver
{% endhighlight %}

Or in webserver.pp at the end of codes.

{% highlight text %}
class webserver{
	...
	...
}

class { "webserver" : }
//or
include webserver

class take parameters in webserver.pp.

class webserver($message = ""){
	
}

In default.pp

class { "webserver":
	message => "Hi there!",
}

In webserver.pp

class (){

	...
	file { '/vagrant/index.html': 
		content => "<h1> Vagrant + Puppet + ${apache} (${operatingsystem}) </h1><p>${message}"</p>,
	}
}
{% endhighlight %}

Save and close it. 

{% highlight text %}
firstVM $ vagrant provision
{% endhighlight %}

Check the browser if it has new message.


### My case

It didn't work. But I made the default.pp as followings and it worked. I deleted webserver.pp.

{% highlight text %}
class webserver{
	 case $operatingsystem {
		centos: {
			$apache = "httpd"
			$configfile = "/etc/httpd/conf.d/vhost.conf"
		}
		ubuntu: {
			$apache = "apache2"
			$configfile = "/etc/apache2/sites-enabled/000-default"
		}
	}
	package { 'apache':
		name => $apache, 
		ensure => 'installed',
	}

	file { 'site-config':
		path 		=> $configfile,
		source 	=> "/vagrant/site-config",
		require => Package["apache"],
	}

	service { 'apache' :
		name => $apache,
		ensure => 'running',
		hasrestart => true,
		subscribe => File["site-config"]
	}

	file { '/vagrant/index.html':
		content => "<h1> Vagrant + Puppet + ${apache} (${operatingsystem}) </h1>",
	}

	if $apache == "httpd" {
		service { 'iptables':
			ensure => 'stopped',
		}
	}
}

include webserver
{% endhighlight %}


## Puppet Modules

[puppetlabs/apache](https://forge.puppetlabs.com/puppetlabs/apache)

To list installed module 

{% highlight text %}
$ puppet module list
# for help
$ puppet help module
{% endhighlight %}

[Installing Modules](http://docs.puppetlabs.com/puppet/2.7/reference/modules_installing.html)

[Puppet Module Manual Page](http://docs.puppetlabs.com/man/module.html) 

{% highlight text %}
$ mkdir module-test
$ cd moduel-test
module-test $ vagrant init precise32
module-test $ vim Vagrantfile
{% endhighlight %}

In Vagrantfile, add the local path for module_path

{% highlight text %}
config.vm.network :forwarded_port, guest: 80, host: 8080

config.vm.provision :puppet do |puppet|
	puppet.module_path =""
end
{% endhighlight %}

Suspend vim by ctrl+z, to resume fg

To check path

{% highlight text %}
module-test $ puppet module install pupeetlabs/apache
# this give an error to tell about a dir. In precise64
...
Directory /home/vagrant/.puppet/modules does not exist
# to find puppet path
module-test $ puppet apply --configprint modulepath
/Users/teacher/.puppet/modules:/usr/share/puppet/modules
{% endhighlight %}

Here you can see the path in the first part
If you have warning like above then you need to make a dir. But my case I installed puppet through gem, I did not need to make it.

{% highlight text %}
$ mkdir -p ~/.puppet/modules
module-test $ puppet module install pupeetlabs/apache
{% endhighlight %}

Now we know the path, /Users/teacher/.puppet/modules. Put it in Vagrantfile.

{% highlight text %}
# going back to vim from suspend is fg.

config.vm.provision :puppet do |puppet|
	puppet.module_path ="/Users/teacher/.puppet/modules"
end
{% endhighlight %}

Save and close it.

{% highlight text %}
$ mkdir manifests
$ vim manifests/default.pp

// in default.pp
// execute the code in apache class
include apache
// also include nested class in apache class
include apache::mod::php
// this will install php as well
{% endhighlight %}

But this won't install php until apt-get installed first. Set 

{% highlight text %}
include apache

Exec {
	path =>["/usr/bin", "usr/local/bin"],
}

exec { "update":
	# you could give path here, path =>. But use Exec {} by setting parameters that passed on all child resources. 
	command => "apt-get update",

}

class { "apache::mod::php":
	require => Exec["update"]
}

# create an apache virtual host
apache::vhost{ "personal-site":
	port => 80,
	docroot => "/vagrant",
}

# something to show
file{ "/vagrant/index.php":
	content => "<?php $title = 'From the PHP' ; ?> <h1><?php echo $title; ?> </h1>",
}
{% endhighlight %}

Save it but it is not finished yet.

{% highlight text %}
$ vagrant up
{% endhighlight %}

And open it in a browser at localhost:8080. But it gives no page. Because php variables need to be escaped. Puppet thinks $ as puppet's variable $ sign.

{% highlight text %}
file{ "/vagrant/index.php":
	content => "<?php \$title = 'From the PHP' ; ?> <h1><?php echo \$title; ?> </h1>",
}
{% endhighlight %}

Save and close it and run vagrant provision.

{% highlight text %}
$ vagrant provision
{% endhighlight %}

Check it in a browser.


### My case on Mac


#### How to install puppet on Mac

Don't use dmg from puppetlab. Use rubygem by

{% highlight text %}
$ gem install puppet
{% endhighlight %}

Installation will create /Users/teacher/.puppet/modules and install modules in it.

{% highlight text %}	
$ puppet module install puppetlabs/apache
{% endhighlight %}


## Undoing Puppet Manifests

No offical way to undo puppet manifests. But there are tricks.

{% highlight text %}
$ mkdir undo
$ cd undo
$ undo $ vagrant init precise32
# create manifests dir and site-config as before
$ ls 
Vagrantfile manifests site-config
$ vim Vagrantfile
config.vm.box = "precise32"
config.vm.provision : puppet
config.vm.network :forwarded_port, guest:80, host:8080
...
{% endhighlight %}

	
