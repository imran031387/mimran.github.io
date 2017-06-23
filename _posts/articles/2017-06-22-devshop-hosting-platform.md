---
layout: article
title: "Devshop As a Cloud Hosting Platform"
date: 2017-06-22
modified:
categories: articles
excerpt:
tags: [hosting,drupal]
ads: true
image:
  feature: 
  teaser: 2017-06-22/0.png
---  

### What is Devshop
DevShop is a "cloud hosting" system for Drupal. DevShop makes it easy to host, develop, 
test and update drupal sites. It provides a front-end built in Drupal (Devmaster) and a 
back-end built with drush, Symfony, and Ansible. DevShop deploys the sites using git, and 
allows us to create unlimited environments for each site. DevShop makes it very easy to deploy
any branch or tag to each environment

The code is deployed on push to your git repo automatically. Deploy any branch or tag 
to any environment. Data (the database and files) can be deployed between environments. 
Run the built-in hooks whenever code or data is deployed, or write your own.

This document will elaborate how to install "DEVSHOP" on a ubuntu environment and major features 
of it, for the demonstration purpose I'm going to use a ubuntu vagrant box, you can download a ubuntu
vagrant box from following link,

`
https://atlas.hashicorp.com/ubuntu/boxes/trusty64
`

## Installing Devshop
Once you download the vagrant ubuntu box, as a next thing we need to do is add that ubuntu 
box to vagrant as follow

```ruby
vagrant box add devshop.example.com ubuntu-14.04-amd64.box
```

Then type the following command to initiate the ubuntu vagrant box

```ruby
vagrant init ubuntu-14.04-amd64
```

Then we need to replace the Vagrant file with the following code

```ruby
# -*- mode: ruby -*-
# vi: set ft=ruby :
 
 
Vagrant.configure(2) do |config|
  config.vm.box = "ubuntu-14.04-amd64"
  config.vm.network "private_network", ip: "192.168.33.11"
  config.vm.hostname = "devshop.example.com"
  config.vm.synced_folder ".", "/var/www", :mount_options => ["dmode=777", "fmode=666"]
end
```

Then we can run the ubuntu server instance by the following command

```ruby
   vagrant ssh
```

Before installing Devshop we need to setup FQDN to the server as follow

```ruby
vi /etc/hostname
#Type a FQDN Eg: devshop.example.com
```
Then we need to reboot the server to get effect

Then we need to run the following command to install Devshop

```ruby
wget https://raw.githubusercontent.com/opendevshop/devshop/0.x/install.sh
bash install.sh
```
End of a successful installation you will get a one-time login to Devshop as the following screen 

<figure>
	<a href="#"><img src="{{ site.url }}/images/2017-06-22/1.png"></a>
	<figcaption></figcaption>
</figure>

By using the one-time login you can set up the password for your Devshop instance 

Following video will show the installation in action

<iframe width="560" height="315" src="//www.youtube.com/embed/3RsRSgbXoaw" frameborder="0"> </iframe>

### Devshop Features

###### Devshop Home Page
The OpenDevShop Home page shows you a bird's eye view of all of your projects. The name, 
install profile, git URL, Drupal Version, and a list of all the environments are visible 
at a glance.Each environment indicators updated in real time. You can see the status of the 
latest task for every site in your system.

<figure>
	<a href="#"><img src="{{ site.url }}/images/2017-06-22/2.png"></a>
	<figcaption></figcaption>
</figure>

###### DevShop Project Dashboard

The project dashboard shows you all the information you need about your website. Git URL, 
the list of branches and tags, links to GitHub, links to the live environment, Drush aliases, 
and most importantly: your project's environments. Each block is a running copy of your website. 
Name them whatever you want. Each one shows you the drupal version, the current git branch or tag, 
the URLs that are available, the last commit time, a files browser, and a backup system.

<figure>
	<a href="#"><img src="{{ site.url }}/images/2017-06-22/3.png"></a>
	<figcaption></figcaption>
</figure>

###### Environments Dashboard

Under the "Environment Settings" button is a list of possible tasks:

* `Download Modules` allows you to add drupal modules and themes to your project, automatically committing them to git.
* `Clone Environment` creates an exact copy of your environment with a new name.
* `Fork Environment` runs a clone, then creates a new branch with a name of your choice!
* `Disable & Destroy Environment.` A setting prevents environments from being destroyed in two clicks.
* `Flush all caches, Rebuild Registry, Run Cron,` etc. These tasks are not really needed if you use Deploy hooks! Cron is always enabled, and caches can be cleared on every code deployment.
* `Backup / Restore,` as you would expect.
* `Run Tests` allows you to manually trigger test runs.

<figure>
	<a href="#"><img src="{{ site.url }}/images/2017-06-22/4.png"></a>
	<figcaption></figcaption>
</figure>

###### Deploy

* The `Deploy Code` control allow you to easily change what branch or tag an environment is tracking.
* `Deploy Data` allows you to deliver new database and files to your environment.
* `Deploy Stack` allows you to move your environment's services (like database and files) from one server to another.

<figure>
	<a href="#"><img src="{{ site.url }}/images/2017-06-22/5.png"></a>
	<figcaption></figcaption>
</figure>

###### Tasks

At the bottom of each environment block, there is a status indicator for the last task that
was run on the environment.You can click any task to view the detailed logs of any task.

<figure>
	<a href="#"><img src="{{ site.url }}/images/2017-06-22/6.png"></a>
	<figcaption></figcaption>
</figure>

###### Logs

Using logs section, the devshop administrator can see all the activities that were taken place within devshop sytem so in case of an issue administrator can easily drill down and find the root cause.
 
<figure>
	<a href="#"><img src="{{ site.url }}/images/2017-06-22/7.png"></a>
	<figcaption></figcaption>
</figure>

So as a conclution why waste money when you have a free open source option to host and maintain 
multiple sites from a single place.

my talk on "London Drupal Meetup" about "How to deploy and manage multiple Drupal sites effectively using “OpenDevshop”"

<iframe width="560" height="315" src="//www.youtube.com/embed/f6W4m7FQ-K4" frameborder="0"> </iframe>







 

