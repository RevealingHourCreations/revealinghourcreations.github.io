---
layout: post
title:  "Writing your own rails engine"
date:   2014-08-09 09:30:00
categories: rails
author: siddhartha
---

### What are we building here?
The idea is to release a system that would popup a random quote every 20 minutes in the dev environment. It could be an entertainer, reminder, refresher, motivator or whatever you want it to be. 

Whether what we are building sits on top of rails, or needs access to core rails functionality and rests just below the skin of an app is an important differenciation to be made i.e, [do we require to build a gem or a rails engine?](http://stackoverflow.com/questions/5521224/difference-between-plugins-and-ruby-gems). For our project, which we'll be calling 'Devmate', we need to build an engine.

### Getting started
To get started, we let rails generate the structure with some boilerplate code for us by running -

    rails plugin new devmate --mountable

This gives us a very railsy structure to play around with, so we have a general idea about what would go where. Engines can be classified as full or mountable, [the basic difference between them](http://stackoverflow.com/questions/6118905/rails-3-1-engine-vs-mountable-app) being the namespacing i.e, `--mountable` will create the engine in an isolated namespace, whereas `--full` will create an engine that shares the namespace of the main app. There are rare cases when we'd want to create a full engine thats not namespaced thus creating two tightly coupled apps.

Our engine's specs reisde in `devmate.gemspec`. The file's contents are pretty much self explanatory.

    $:.push File.expand_path("../lib", __FILE__)

    # Maintain your gem's version:
    require "devmate/version"

    # Describe your gem and declare its dependencies:
    Gem::Specification.new do |s|
      s.name        = "devmate"
      s.version     = Devmate::VERSION
      s.authors     = ["siddharth bhagwan"]
      s.email       = ["sid_sss@hotmail.com"]
      s.homepage    = "https://github.com/siddharthbhagwan/devmate"
      s.summary     = "Devmate pops up random quotes on the screen as you work"

      s.files = Dir["{app,config,db,lib}/**/*"] + ["MIT-LICENSE", "Rakefile", "README.md"]
      s.test_files = Dir["test/**/*"]

      s.add_dependency "rails", ">= 3.0.0"
      s.add_dependency "jquery-rails", ">= 3.0.0"
      s.add_dependency "json"
    end

__Note:__ If we lookup the gemfile, we notice that the `Gemfile` contains a line - `gemspec`. This means that it'll refer to the required gems from the gemspec, and those don't need to be repeated in the gemfile.

### Basic Functionality
The engine intends to display a quote every 20 minutes - lets put that data i.e, a few quotes in `config/data` in json format, so it'll be easy to retrieve.

    #quotes.json

    { 
      "1" : "Quote 1",
      "2" : "Quote 2",
      .
      .
    } 

All we have to do is retrieve this file once, and  then periodically display a quote. For displaying the quote, we'll be using [noty](ned.im/noty). The api is super simple to use, and looks really neat. Since our engine's functionality consists of really basic stuff, we don't quite need a model or a controller, and our javascript can go directly into the `application.js` itself.

On a page refresh, the data is fetched once, and using setTimeout a quote is picked randomly to be displayed using noty.

__Note:__ Noty's files have been added in `app/assets/javascript`, and not included via a gem.

    # application.js

    // Fetch the quotes file
    var quotes = undefined;
    $.ajax({
        url: '/fetchData.json',
        success: function(msg) {
          quotes = msg;
        },
        error: function(jqXHR, textStatus, errorThrown){
        },
        complete: function(){
          setTimeout(setPrompt, 1000);
        }
      });

    setPrompt = function() {
      // Generate a random number of the quote to be picked
      random_index = ((Math.floor((Math.random() * 100))% quotes.data_count) + 1)
      noty({
        text: quotes.data[random_index],
        layout: quotes.position,
        maxVisible: 5,
        timeout: quotes.timeout,
        type: 'default'
      });

      setTimeout(setPrompt, quotes.call_timeout);
    };

  Lets set up that controller action that is being ajax-ed next.

    # application_controller.rb

    def fetchData
      path = File.join(Devmate::Engine.root, 'config', 'data', 'quotes.json')
      if File.exists?(path)
        quotes_file = File.read(path)
        quotes_hash = JSON.parse(quotes_file)
        test_data = Hash[ 
          :position => Devmate.position,
          :timeout => Devmate.timeout,
          :call_timeout => Devmate.call_timeout,
          :maxVisible => Devmate.maxVisible,
          :close_with => Devmate.close_with,
          :data_count => quotes_hash.length.to_s,
          :data => quotes_hash
        ]
        respond_to do |format|
          format.json {
            render json: test_data.to_json
          }
        end
      end
    end

And make a corresponding entry to `routes.rb`

    Devmate::Engine.routes.draw do
      get 'fetchData' => 'application#fetchData'
    end


### Adding Configuration
We now have a minimal functional engine. It is often required to give the user the option to configure the fucntionality. In our case, those configurable properties are the interval at which the quotes popup, the location i.e, where on the screen does it popup, etc. as can be seen above in `application_controller.rb`. Enabling the user to provide config values means we're going to have to define some default values first. These are the ones the engine will refer to in the absence of user provided values. This is done in `lib/devmate.rb` via a meta class.

    # lib/devmate.rb

    class << self
      mattr_accessor :position, :timeout, :maxVisible, :close_with, :call_timeout
      self.position = 'topRight'
      self.timeout = '60000'
      self.maxVisible = '7'
      self.close_with = 'click'
      self.call_timeout = '1200000'
    end

    # Map variables from your app into your engine
    def self.setup(&block)
      yield self that resides in `lib/generators`

    class DevmateGenerator < Rails::Generators::Base
      desc 'This generator creates an initializer file at config/initializers for devmate'

      def create_initializer_file
        create_file 'config/initializers/devmate.rb', 
          "Devmate.setup do |config|

            # Where would you like the notification to appear?
            # [top, topLeft, topCenter, topRight, centerLeft, center,
            centerRight, bottomleft, bottomCenter, bottomRight, bottom]
            config.position = 'topRight'

            # How long before it is dismissed
            config.timeout = 5000

            # Number of notification visible simultaneously
            config.maxVisible = 1

            # Call every n seconds
            config.call_timeout = 8000

            # close with
            config.close_with = 'click'
          end
        end "
      end
    end

Running `rails g` in the host app will now give `devmate` as one of the available generators and runnning `rails g devmate` will create `config/initializers/devmate.rb` in the host app. These values will now override the default ones in the engine.

### Mounting the enginge in your application
Our egnine is ready! Let us use it locally within an app. All we need to do is require it in the gemfile, mount it, and include its assets. Once tested, the gem can be pushed to [rubygems](rubygems.org), and the gemfile can fetch it from there.

~~~   
# Hosting App's Gemfile

group :development do
  # path to the local codebase
  gem 'devmate', path: '../devmate'
end
~~~

~~~
# Hosting apps config.routes

mount Devmate::Engine, at: "/"
~~~

~~~
# Hosting App's applicaiton.html

<%= javascript_include_tag "devmate/application" %>
~~~

Restart the hosting app, and a quote should popup on the screen!

The complete source code for devmate is available [here](https://github.com/siddharthbhagwan/devmate).

