---
layout: post
title:  "V for Vagrant"
date:   2015-03-25 12:30:14
categories: rhc
author: sagar
---

If you are a web developer, a operations engineer or a designer then ```vagrant``` is the best tool for you.


###What is vagrant?
You wrote code that works on local but doesn't work on production may be because you are using different version of database or may be your local machine have different operating system than production or there may be lots of scenarios.

Vagrant allows you to create consistent development and production environment.  
Vagrant is a very useful tool that helps you to work in your favourite operating sytem but runs code in the production server's operating system. It also helps to run instance on AWS or rackspace and can stop after your testing is done in single command.

###Installation
You will need [Virtualbox](https://www.virtualbox.org/) and then [Vagrant](http://www.vagrantup.com/downloads).

Choose the box from [ATLAS](https://atlas.hashicorp.com/boxes/search) as per your production server's OS. This box is JOUS(just enough operating system).  
Now, goto your project directory and enter ```$ vagrant init ubuntu/trusty64```. Here we have selected ubuntu64. This will just create vagrant file which is configuration file.
You can automate a process of installation of all softwares which is required for your project.
Below is a shell script to install postgresql project_dependancies.sh  

<pre><code>#!/usr/bin/env bash  
apt-get install -y postgresql-9.1  
</code></pre>
Specify above shell script in vagrantfile like this.

<pre><code>Vagrant.configure(2) do |config|  
  config.vm.box = "ubuntu/trusty64"  
  config.vm.provision :shell, path: "project_dependancies.sh"  
end</code></pre>

Now enter ```$ vagrant up --provider virtualbox```  
This command will take time if you are doing it for the first time. It will download the base image of OS and then will install softwares that are specified in project_dependancies.sh file and then it will boot OS.
Now your guest machine is booted up and you can play with it.
You can ssh using ```$ vagrant ssh``` into this machine same as you do ssh into your production server.

###Synced Folders
This is very nice feature of vagrant. You have ssh'ed into guest machine. just goto  ```cd /vagrant/``` and ```ls```
You can see that you have access to entire project directory that you have working with on host machine. This means you can edit from guest machine. Really awesome feature.

###Networking
Suppose you are running rails project on guest machine on port 3000. And We have to check on our host machine's broweser. So we have to forward port of rails application on some port of host.  
e.g. ```config.vm.network "forwarded_port", guest: 3000, host: 4567```.
Now we can goto ```http://localhost:4567/``` on host machine and it will connect to guest machine's port 3000. Thats so cool.

### Vagrant Share
You can show your work to your client using vagrant.
First you have to create [ATLAS](http://vagrantcloud.com/) account and then  
```$ vagrant login``` enter username and password and ```$ vagrant share```. This will give you URL and you can share it with client which will share your vagrant environment.
