---
layout: article
title: "Drupal 7 Testing With Behat"
date: 2015-08-08
modified:
categories: articles
excerpt:
tags: [TDD,Test,Drupal 7]
ads: true
image:
  feature: 
  teaser: 2015-08-08/0.png
---  

#### Introduction

Behat is a tool that makes behavior driven development (BDD) possible. With BDD, we can write human-readable 
stories that describe the behavior of your application. These stories can then be auto-tested against our 
applications

#### What does the Drupal Extension add?

* Set up test data with Drush or the Drupal API
* Define theme regions and test data appears within them
* Clear the cache, log out, and other useful steps
* Detect and discover steps provided by contributed modules and theme

#### Prerequisite

* PHP (It must be higher than 5.3.5!)
* JAVA
* Drush
* Selanium

#### System wide Installation

A system-wide installation allows you to maintain a single copy of the testing tool set and use it for multiple test environments.

#### Install Composer

```ruby
   curl -sS https://getcomposer.org/installer |
   php mv composer.phar /usr/local/bin/composer
```

#### Install the Drupal Extension

1. Make Directory drupalextention

```ruby
cd /opt/
sudo mkdir drupalextension
cd drupalextension/
```

2. Create a file called composer.json with the following code syntax

```ruby
{
  "require": {
    "drupal/drupal-extension": "~3.0"
},
  "config": {
    "bin-dir": "bin/"
  }
}
```

3. Run the install command

```ruby
sudo composer install
```

4. Check whether behat installed successfully  

```ruby
bin/behat --help
```

If you were successful, you’ll see the help output.

5. Create a symlink this will make the binary available system-wide so you can use behat only instead of bin/behat

```ruby
ln -s /opt/drupalextension/bin/behat /usr/local/bin/behat
```

#### Set up tests

1. Create a folder to resides the test scenarios (There is no specific constraints for folder location but best practice is keep the test scenarios with in the same repository)

```ruby 
cd drupal_root
mkadir sites/default/behat-tests
```

2. Create a behat.yml file with in the folder with the following code basically this is the file we are using for setting the configurations for behat test

```ruby 
default:
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
      selenium2: ~
      base_url: base_url of your drupal site (Ex :http://www.behat-test.local.com)
    Drupal\DrupalExtension:
      blackbox: ~
      api_driver: 'drupal'
      drupal:
         drupal_root: /Users/mimran/Projects/behat-test
      drush:
         root: your drupal root folder (Ex :/Users/mimran/Projects/behat-test)
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

3. Initialize behat. This creates the features folder with some basic things to get you started:

```ruby
bin/behat --init
```

This will generate a FeatureContext.php file where we can write our own gherkin code snippets.

```ruby
 <?php
 
use Behat\Behat\Tester\Exception\PendingException;
use Drupal\DrupalExtension\Context\RawDrupalContext;
use Behat\Behat\Context\SnippetAcceptingContext;
use Behat\Gherkin\Node\PyStringNode;
use Behat\Gherkin\Node\TableNode;
 
/**
 * Defines application features from the specific context.
 */
class FeatureContext extends RawDrupalContext implements SnippetAcceptingContext {
 
  /**
   * Initializes context.
   *
   * Every scenario gets its own context instance.
   * You can also pass arbitrary arguments to the
   * context constructor through behat.yml.
   */
  public function __construct() {
  }
}
```

4. To ensure everything is set up appropriately, type:

```ruby
behat -dl
```

if all went well you can see a result like follow (smile) (These are predefined behat gherkin snippets that we can use for create test suits)

```ruby 
default | Given I am an anonymous user
default | Given I am not logged in
default | Given I am logged in as a user with the :role role(s)
default | Given I am logged in as :name
```

### Environment specific settings

Some of the settings in behat.yml are environment specific. For example the base URL may 
be http://behat-test.local.com on your local development environment so we cant put them in 
to behat.yml in such case we have to put them as envirment variable as follow 

```ruby 
{
    "extensions": {
        "Behat\\MinkExtension": {
            "base_url": "http://myproject.localhost"
        },
        "Drupal\\DrupalExtension": {
            "drupal": {
                "drupal_root": "/var/www/myproject"
            }
        }
    }
}
```

*NOTE:* To export this into the BEHAT_PARAMS environment variable, squash the JSON object into a single line and surround with single quotes: like following syntax

```ruby 
export BEHAT_PARAMS='{"extensions": {"Behat\\MinkExtension": {"base_url": "http://myproject.localhost"},"Drupal\\DrupalExtension": {"drupal": {"drupal_root":"/var/www/myproject"}}}}'
```

*HINT:* for automatically create JSON object with the environment variable we can use drush-bde-env













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
   
Then enable debug and auto reload of the twig and disable cache as the figure:1 below

NOTE: You can enable these on the default.service.yml file but best practice to have it on the 
development.services.yml file as follow

Clear the cache for take effect and inspect the web page and see for theme suggestions as the figure:2

<figure class="half">
	<a href="#"><img src="{{ site.url }}/images/2017-06-24/1.png"></a>
	<a href="#"><img src="{{ site.url }}/images/2017-06-24/2.png"></a>
</figure>

#### Enable SAAS minify & uglify and watch and live reaload.

Create a package.json file to install npm dependencies or as to get a kick start clone my repo from the 
below link

`https://github.com/imran031387/Drupal-8-Theme-Starter`

```ruby
{  
  "name": "cybertron-gulp",
  "version": "1.0.0",
  "description": "Gulp dependencies for cybertron theme",
  "main": "gulpfile.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "repository": {
    "type": "git",
    "url": "https://github.com/imran031387/Drupal-8-Theme-Starter.git"
  },
  "author": "mimran",
  "license": "ISC",
  "devDependencies": {
    "gulp": "^3.9.1",
    "gulp-autoprefixer": "^3.1.1",
    "gulp-imagemin": "^2.4.0",
    "gulp-livereload": "^3.8.1",
    "gulp-sass": "^2.3.2",
    "gulp-sourcemaps": "^1.12.0",
    "imagemin-pngquant": "^4.2.2",
    "gulp-uglify": "^3.0.0",
    "gulp-concat": "^2.6.1"
  }
}
```

Create the gulpfile.js tasks as follow

```ruby
var gulp = require('gulp');
var livereload = require('gulp-livereload');
var sass = require('gulp-sass');
var autoprefixer = require('gulp-autoprefixer');
var sourcemaps = require('gulp-sourcemaps');
var imagemin = require('gulp-imagemin');
var pngquant = require('imagemin-pngquant');
var concat = require('gulp-concat');
var rename = require('gulp-rename');
var uglify = require('gulp-uglify');




gulp.task('imagemin', function () {
    return gulp.src('./images/*')
        .pipe(imagemin({
            progressive: true,
            svgoPlugins: [{removeViewBox: false}],
            use: [pngquant()]
        }))
        .pipe(gulp.dest('./images'));
});


gulp.task('sass', function () {
  gulp.src('./sass/**/*.scss')
    .pipe(sourcemaps.init())
        .pipe(sass({outputStyle: 'compressed'}).on('error', sass.logError))
        .pipe(autoprefixer('last 2 version', 'safari 5', 'ie 7', 'ie 8', 'ie 9', 'opera 12.1', 'ios 6', 'android 4'))
    .pipe(sourcemaps.write('./'))
    .pipe(gulp.dest('./css'));
});


gulp.task('uglify', function() {
    return gulp.src('./lib/*.js')
        .pipe(concat('scripts.js'))
        .pipe(gulp.dest('./js'))
        .pipe(rename('main.js'))
        .pipe(uglify())
        .pipe(gulp.dest('./js'));
});

gulp.task('watch', function(){
    livereload.listen();
    gulp.watch('./sass/**/*.scss', ['sass']);
    gulp.watch('./lib/*.js', ['uglify']);
    gulp.watch(['./css/style.css', './**/*.twig', './js/*.js'], function (files){
        livereload.changed(files)
    });
});
```

Install the live reload chrome plugin to see the changes real time in the browser

and then type

`npm install`

`gulp watch`

and that's it you have setup your drupal 8 themeing environment will talk next about "twig"
templating engine...;)








   
