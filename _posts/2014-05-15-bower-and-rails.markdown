---
layout: post
title:  "Bower and Rails"
date:   2014-05-15 12:30:14
categories: rails
author: siddhartha
---

### What is it and why do I care?

Bower is a front end package manager. 

"Okay. I’m sure it is. But I’m on rails, and I have bundler, where I manage my gems and their versions, so I doubt this Bower is going to do me any good"

So lets assume you intend to use v1.0 of a framework, ‘AbcJs’ and theres a gem, ‘AbcJs-rails’ for it. Now AbcJs has made some improvements and released a v2.0. So you'll have to wait for the author of the ‘AbcJs-rails’ to release a version of the gem corresponding to this v2.0.

Gems based on popular frameworks like jQuery, are released fast enough to keep pace with the framework. However, for lesser popular gems, this might not be the case, and thus a dependency is created.

One alternative to remove this dependency is moving from gems to manually downloading the Library/framework files and requiring them in application.js.

Bower is a cleaner, easier and more efficient way to do just this.

### Installing Bower

Install Bower via the Node Pacakage Manager using the command

    npm install -g bower

`-g` indicates that this will be a global installation, and will be accessible from other folders.

### How to integrate Bower with your existing rails app [ Rails 4 ]

Assuming you already have an existing rails project, lets move jQuery from the gem to bower. To do this, uninstall jQuery by commenting the line `gem "jquery-rails"` in your gemfile and running `bundle`. Also, comment the lines `//= require jquery` and `//= require jquery_ujs` in `application.js` by removing the `=` making it `// require jquery` and `// require jquery_ujs` and restart your server.

### Where will bower install its files?

Rails has a convention for assets, i.e, they either go into `app/assets/javascripts` or `vendor/assets/javascripts`. Bower, by default, will install all its files into a folder called `bower_components`, in the root directory of your app. To change this, we add a bower config file, `.bowerrc` at the root level, to tell bower where to install all its files. We wil install our dependencies in vendor/assets/javascripts. It will look something like this :

    {
      "directory" : "vendor/assets/javascripts"
    }

Next we initialize a `bower.json` file for your project. This file will contain all the bower dependencies of your app. Its like the `Gemfile` for bower. To create this file, run the command `bower init`. This command will require a few user inputs as follows :

    [?] name: mysampleproject
    [?] version: 0.0.1
    [?] description: This is a Sample App
    [?] main file: 
    [?] what types of modules does this package expose?[ left ]
    [?] keywords: [ "bower", "web", "rails" ]
    [?] authors: Siddharth Bhagwan <siddhartha@revealinghour.in>
    [?] license: MIT
    [?] homepage: https://github.com/siddharthbhagwan/mysampleproject
    [?] set currently installed components as dependencies? No
    [?] add commonly ignored files to ignore list? Yes
    [?] would you like to mark this package as private which prevents it from being  
        accidentally published to the registry? No

The `bower.json` file created should look something like this :

    {
      name: 'sampleapp',
      version: '0.0.1',
      homepage: 'https://github.com/siddharthbhagwan/sampleapp',
      authors: [
        'Siddharth Bhagwan <myemail@example.com>'
      ],
      description: 'Sample App',
      keywords: [
        'rails',
        'sample',
        'app'
      ],
      license: 'MIT',
      ignore: [
        '**/.*',
        'node_modules',
        'bower_components',
        'app/bower_components',
        'test',
        'tests'
      ]
    }


Next, lets add jQuery. To find the jQuery packages available, run the command

`bower search jquery`

This returns a (huge) list of packages that have the name jQuery in it, as shown below

    jquery git://github.com/jquery/jquery.git
    jquery-ui git://github.com/components/jqueryui
    jquery.cookie git://github.com/carhartl/jquery-cookie.git
    jquery-placeholder git://github.com/mathiasbynens/jquery-placeholder.git
    jquery-file-upload git://github.com/blueimp/jQuery-File-Upload.git
    jasmine-jquery git://github.com/velesin/jasmine-jquery
    jquery.ui git://github.com/jquery/jquery-ui.git
    .
    .
    .


In our case, we look for the one that has just the name ‘jquery’, that happens to be the first one.

To not break your code, you’d like to ensure that the version of jQuery you install via bower corresponds to the one you had in your gemfile. To look for the available versions, run the command.

`bower info jquery`

This returns amongs other details like the authors and the dependencies, a list of versions available for that particular package
To install a particular version, lets say 1.9.0, run the command.

`bower install jquery#1.9.0 --save`

The `--save` flag updates your bower.json file, adding the package to the dependencies of your project. The `bower.json` file will now have an entry corresponding to jquery version 1.9.0 in its dependencies.

Now, you should see a folder called jquery in `vendor/assets/javascripts`. This will contain jquery's source files, and its own `bower.json` file. Uncomment the line `// require jquery` by adding `=` making it `//= require jquery' in the `application.js` file.

Restart your app and jquery should be working fine. Repeat the process for jquery-ujs as an exercise.
