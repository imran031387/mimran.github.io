---
layout: article
title: "Drupal 8 Theming Part 1"
date: 2017-06-24
modified:
categories: articles
excerpt:
tags: [theme development,drupal8]
ads: true
image:
  feature: 
  teaser: 2017-06-24/0.png
---  
As continious serious of blog posts today i'm gonna write about how to setup drupal 8  development environment for 
Themeing 

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
   
Then enable debug and auto reload of the twig and disable cache as the figure:1 below

NOTE: You can enable these on the default.service.yml file but best practice to have it on the 
development.services.yml file as follow

Clear the cache for take effect and inspect the web page and see for theme suggestions as the figure:2

<figure class="half">
	<a href="#"><img src="{{ site.url }}/images/2017-06-24/1.png"></a>
	<a href="#"><img src="{{ site.url }}/images/2017-06-24/2.png"></a>
</figure>

#### Enable SAAS minify & uglify and watch and live reaload.

Create a package.json file to install npm dependencies

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

Install the live reload chrome plugin to see the changes realtime in the browser

and then type

`npm install`

`gulp watch`








   
