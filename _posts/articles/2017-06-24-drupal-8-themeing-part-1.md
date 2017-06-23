---
layout: article
title: "Drupal 8 Theming Part 1"
date: 2017-06-23
modified:
categories: articles
excerpt:
tags: [theme development,drupal8]
ads: true
image:
  feature: 
  teaser: 2017-06-24/0.png
---  

One of major functionalities of Drupal 7 is  database abstraction layer this letting us to create complex 
static and dynamic queries easy and  more readable manner by using query builders but unfortunately there 
are some significant performance differences among the query builders.

#### Enable TWIG Debug mode and disable Drupal cache.

Copy the example.settings.local.php file in to default folder and rename it to settings.local.php

NOTE: check whether the settings.local.php file is get enabled in the local development services

```ruby
   $settings['container_yamls'][] = DRUPAL_ROOT . '/sites/development.services.yml’;
```
Then make Drupal to read that file configurations as below

1. Go to settings.php file
2. And un-comment following lines of code
   ```ruby
   if (file_exists($app_root . '/' . $site_path . '/settings.local.php')) {
       include $app_root . '/' . $site_path . '/settings.local.php’;
   }
   ```
   
Then enable debug and auto reload of the twig and disable cache as follow

NOTE: You can enable these on the default.service.yml file but best paractice to have it on the 
development.services.yml file as follow

<figure>
	<a href="#"><img src="{{ site.url }}/images/2017-06-24/1.png"></a>
	<figcaption></figcaption>
</figure>

Clear the cache for take effect and inspect the web page and see for theme suggestions as follow

<figure>
	<a href="#"><img src="{{ site.url }}/images/2017-06-24/2.png"></a>
	<figcaption></figcaption>
</figure>

   
