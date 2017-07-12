---
layout: article
title: "Getting Started Drupal Development With ZAIZI Drupal Stack(Vagrant Box)"
date: 2017-07-12
modified:
categories: articles
excerpt:
tags: [development,vagrant,drupal]
ads: true
image:
  feature: 2017-07-12/1.png
  teaser: 2017-07-12/0.png
--- 

Setting up development environment is always a nightmare for developers and will have to spend considerable amount 
of time as well, So in this post, I'm going to elaborate how to setup LAMP & LEMP using vagrant in a quick time frame,
so that developers concentrate on development rather setting up environments.


#### What is Vagrant?

Vagrant allows developers to build, manage and share virtual development environments. Vagrant uses VirtualBox for 
its virtual machines and you can use Puppet or Chef as a provisioning tool. The goal of Vagrant is to offer developers 
the ability to build and work in a consistent virtualized environment (mirror of a production server) without wasting 
time setting up a local version of Apache, PHP and MySQL....etc.

#### What is Zaizi Drupal Stack (ZDS)?

Zaizi Drupal Stack is a preconfigured Vagrant Box with a full array of LAMP and LEMP Stack features to get you up and 
running with Vagrant to setup drupal devolopment envirment in no time.Basically we used scotch/box as our base vagrant 
box since it's already have LAMP stack features in it so on top of that we added Nginx, PHP-FPM, Drush, PHP Codesniffer 
and Composer.


#### How to Create a Drupal Devolopment Envirment With ZDS?

1. First you need to download the Zaizi Drupal Stack on the following link.
```ruby
    https://www.mediafire.com/?bugv2e0jrf73su9
```
2. Install Vagrant (You can use the vagrant.dmg file which is in the zaizi drupal stack folder).
3. Install VirtualBox (You can use the virtualbox.dmg file which is in the zaizi drupal stack folder).
4. Create a project folder as follow

```ruby
    # Create the project folder
    cd <your desired place>
    mkdir zaizi-drupal-stack
    cd zaizi-drupal-stack
```

5. Copy the zaizi-drupal-stack.box in to the project folder
6. Then add zaizi-drupal-stack.box to vagrant as follow

```ruby
    vagrant box add zaizi-drupal-stack zaizi-drupal-stack.box
```

7.  Then Type following Command on terminal

```ruby
    vagrant init zaizi-drupal-stack
```
it will create a Vagrant file in the project root directory.

8. Then replace the vagrant file content with following code

```ruby
    # -*- mode: ruby -*-    
    # vi: set ft=ruby :
    Vagrant.configure("2") do |config|
    config.vm.box = "zaizi-drupal-stack"
    config.vm.network "private_network", ip: "192.168.33.10"
    config.vm.hostname = "zaizi-drupal-stack"
    config.vm.synced_folder ".", "/var/www", :mount_options => ["dmode=777", "fmode=666"]
    end
```

9. Then Type Following command

```ruby
    vagrant up
```

10. To make sure our development environment successfully setup ,we need ssh in to the system

```ruby
    # SSH in to the server by typing following command
    vagrant ssh
```
if you can see the following screen that means your development environment is setup successfully

<figure>
	<a href="#"><img src="{{ site.url }}/images/2017-07-12/2.png"></a>
	<figcaption></figcaption>
</figure>


#### Setup Web Server Root

To create a web server root just we need to copy the public folder(which is in the zaizi drupal stack folder) 
to the project folder ,once done with that you can access the root folder via http://192.168.33.10/

if all went well you can see the following page with all the system details.

<figure>
	<a href="#"><img src="{{ site.url }}/images/2017-07-12/3.png"></a>
	<figcaption></figcaption>
</figure>


