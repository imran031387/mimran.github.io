---
layout: article
title: "Configure Phantomjs with Drupal"
date: 2015-09-10
modified:
categories: articles
excerpt:
tags: [TDD,Test,Drupal 7]
ads: true
image:
  feature: 
  teaser: 2015-09-10/0.png
---  

Phantomjs describes itself as “a headless WEBKIT” with a javascript API which can run on terminal,
 and able to do everything that a typical browser can do further we can write code to tell it exactly
  what to do without the manual interaction.


#### Installation

Installing Phantomjs is very easy using "brew"

```ruby
brew update
brew install phantomjs
```
To make sure phantomjs is installed properly just following command on terminal "phantomjs --version" 
if you can see a version number that means phantomjs installed properly

### Examples

#### Screen Capture
Using phantomjs you can write a script as follow to capture the screen

* Create a js file with the name capture.js
* Copy the following command to the file

```ruby
var page = require('webpage').create();
page.open('http://www.google.com/', function() {
    page.render('screen/google.png');
    phantom.exit();
});
```

* Run the following command to execute the phantomjs file

```ruby
phantomjs capture.js
```

As a result you can see following snap

<figure>
	<a href="#"><img src="{{ site.url }}/images/2015-09-10/1.png"></a>
	<figcaption></figcaption>
</figure>


#### Measure page loading speed

you can measure the page loading speed as follow
* Create a file with the name of loadingspeed.js
* Copy the following command to the file

```ruby

var page = require('webpage').create(),
    system = require('system'),
    t, address;
 
if (system.args.length === 1) {
    console.log('Usage: loadspeed.js <some URL>');
    phantom.exit(1);
} else {
    t = Date.now();
    address = system.args[1];
    page.open(address, function (status) {
        if (status !== 'success') {
            console.log('FAIL to load the address');
        } else {
            t = Date.now() - t;
            console.log('Page title is ' + page.evaluate(function () {
                return document.title;
            }));
            console.log('Loading time ' + t + ' msec');
        }
        phantom.exit();
    });
}
```

Run the phantomjs script as follow

```ruby 
phantomjs loadingspeed.js http://www.google.com
```

As a result you can see following

<figure>
	<a href="#"><img src="{{ site.url }}/images/2015-09-10/2.png"></a>
	<figcaption></figcaption>
</figure>

#### Intergrating Phantomjs With Behat

Typically Behat is running using selanium web-driver so what we are going to do here is we are going to 
integrate the headless phantomjs WEBKIT(Known as a Gohst Driver) to behat so for that we are going to do 
following steps

##### Start the Ghost Driver
* We have to start the Ghost driver on a specific port and we are going to integrate that port to behat 
using "behat.yml" file to running the tests on top of phantom js

```ruby
phantomjs --webdriver=4445
```
<figure>
	<a href="#"><img src="{{ site.url }}/images/2015-09-10/3.png"></a>
	<figcaption></figcaption>
</figure>

* Create a behat profile to intergrate the phantomjs to behat as follow

```ruby
phantomjs:
  suites:
    default:
      contexts:
        - FeatureContext
        - Drupal\DrupalExtension\Context\DrupalContext
        - Drupal\DrupalExtension\Context\MinkContext
        - Drupal\DrupalExtension\Context\MessageContext
        - Drupal\DrupalExtension\Context\DrushContext
 
  extensions:
    Behat\MinkExtension:
      goutte: ~
      selenium2:
        # Intergrate the phantomjs to behat by adding following setting
        wd_host: http://localhost:4445/wd/hub
      base_url: http://www.behat-test.local.com
    Drupal\DrupalExtension:
      blackbox: ~
      region_map:
              footer: "#footer"
      api_driver: 'drupal'
      drupal:
         drupal_root: /Users/mimran/Projects/behat-test
      drush:
         root: /Users/mimran/Projects/behat-test
 
      region_map:
        right sidebar: "#aside .region-sidebar-second"
        content: "#content"
        # Header regions
        left header: "#header-left"
        top header: "#nav-header"
        right header: "#header-right"
        bottom header: "#nav-masthead"
        # frontpage content regions
        top middle content: "#sites-with-drupal"
        top right content: "#develop-with-drupal"
        bottom right content: "#community-updates"
        middle content: "#front-drupal-stats"
        # Footer region
        footer: "#footer"
        # Pager region
        pager: ".pager"
```

* To run the behat tests on top of phantomjs you can use following command

```ruby
behat -p phantomjs
```


