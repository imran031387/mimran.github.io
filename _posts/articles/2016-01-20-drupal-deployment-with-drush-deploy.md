---
layout: article
title: "Configure Drush Deploy For Deploy Drupal Painlessly"
date: 2016-01-20
modified:
categories: articles
excerpt:
tags: [TDD,Test,Drupal 7]
ads: true
image:
  feature: 
  teaser: 2016-01-20/0.png
---  

Drush deploy is a deployment framework built on Drush. It is heavily influenced by capistrano.this 
help us to deploy our code in a way that will cause minimal/no downtime, and let us to easily rollback 
when problems occurs.

#### Perquisite

* PHP 5.3 or above
* Only support for GIT
* Drush 7

#### Installation

Drush Deploy installation is pretty simple what we need to do is just clone the drush deploy frame work to .drush folder as below

```ruby
cd ~/.drush
git clone --branch 7.x-1.x http://git.drupal.org/project/drush_deploy.git
drush cc drush
```

*NOTE: after clone you need apply the following patch for drush deploy in order to get the drush deploy work with out errors.*

`https://www.drupal.org/files/issues/missing_arg3_for_drush_backend_parse_packets-2358067-1.patch`

### Setting up Drush Deploy

Here what we are going to do is we are going to deploy the code for a remote server with the GIT repository 
so for that we have to create a aliases.drushrc.php and deploy.drushrc.php file

#### aliases.drushrc.php

Typically this file is used to setup the aliases for the drush deploy as below, in this example we are creating this file 
on .drush folder you can create that files under sites folder of the drupal root as well

```ruby
<?php
$aliases['site1'] = array(
 'root' => '/home/mimran/drush-deploy',
    'remote-user' => 'mimran',
    'remote-host' => 'metoffice.zaizicloud.net',
);
?>
```

#### deploy.drushrc.php

This file used to carry the drush deploy settings as follow

```ruby
<?php
    $options['application'] = 'drush-deploy';
    $options['deploy-repository'] = 'https://imran-git:your_password@bitbucket.org/imran-git/drush-deploy.git';
    $options['branch'] = "master";
    $options['keep-releases'] = 3;
    $options['deploy-via'] = 'RemoteCache';
    $options['docroot'] = '/home/mimran/drupal-site-1';
    ?>
```

#### Initialize Drush Deploy

Once done withe drushhrc files setup we can initialize the drush deploy on remote servers by typing following command

```ruby
drush deploy-setup @site1
```

as a result you can see following folder structure on your remote server

<figure>
	<a href="#"><img src="{{ site.url }}/images/2016-01-20/1.png"></a>
	<figcaption></figcaption>
</figure>

After that you can use following command to deploy your code to server when ever you want deploy

```ruby
drush deploy @site1
```

In case you want to rollback you can use following command

```ruby
drush deploy-rollback @site1
```

#### References

`https://www.drupal.org/project/drush_deploy`

`http://www.drupalcontrib.org/api/drupal/contributions!drush!examples!example.aliases.drushrc.php/7`


