---
layout: post
title:  "Creating a Single Page App Rails API for AngularJS"
date:   2017-01-24 19:02:40 +0000
---


When I set out to create my first single page application using AngularJS and Rails, my first thought was, "How in the world do I connect these two together?" I watched several YouTube videos, read the documentation, reviewed all of the Angular material on the Learn.co platform. To my surprise, it was simpler than I expected and I was overthinking the entire process. While not necessarily easy, it was as simple as setting up an API using Rails to serve as the backend and draw out all of my views and controllers using Angular.

This post will explore the steps needed to create the backend for AngularJS frontend.

If you do not know what Rails and AngularJS are, here is a brief description of the two.

"Ruby on Rails, or simply Rails, is a server-side web application framework written in Ruby under the MIT License. Rails is a model–view–controller (MVC) framework, providing default structures for a database, a web service, and web pages." - Wikipedia

"AngularJS (commonly referred to as "Angular" or "Angular.js") is a complete JavaScript-based open-source front-end web application framework mainly maintained by Google and by a community of individuals and corporations to address many of the challenges encountered in developing single-page applications." - Wikipedia

When we combine the two, we can use Rails to serve the API and connection to the database, and Angular to handle routing, displaying, and manipulation of data without needing to refresh the page. 

My app design is to provide a dropbox style service to send audio files to an audio engineer, who then in return will send back the mixed and mastered audio files in exchange for payment. 

The first step I took in developing this app was actually to grab a sheet of paper and sketch out how I wanted things to look and feel in respects to the database models and the views. Starting with this approach made it easier when it came time to create the database and also the overall flow of the application. It does not have to be elaborate, but it serves as an outline and frees up mental resource you need for the trek ahead.

Next, it is time to get the terminal open and start with the `rails new name_of_app` command to instantiate a new rails application. This command builds out everything you need to begin building your Rails API.

I planned on the app being able to handle many musicians and engineers; I needed some authentication for logging in to keep user data separate from other users. I chose to go with Devise as we have been studying Devise here at Learn.co. 

In my application, I wanted to remove turbolinks from the gemfile and any reference to it in the other files. Turbolinks cause issues when combined with JavaScript and jQuery. After removing turbolinks, I added `bower-rails, devise, angular-rails-templates, active-model-serializer, and bootstrap-sass` to the gemfile.

Bower is an optional package manager. However, I recommend it as it makes installing dependencies easier. Devise handles user authentication and authorization, Angular-rails-templates allows up to place our HTML template files inside of the assets/javascript directory. Active-model-serializer converts our models into JSON for use in the API. Lastly, Bootstrap makes it a smoother experience to style your views with CSS.

After deciding which gems you want to use, you'll need to install them, by running 'bundle install'.

Next, we create the database by running 'rake db:create'.

Now, we'll generate the bower.json file for adding dependencies by running `rails g bower_rails:initialize json`. In this file you'll want to insert the following packages under vendor, dependencies: `“angular”: “v1.5.8”,  “angular-ui-router”: “latest”, “angular-devise”:  “latest”` After adding these to the json file, you'll want to install them by running 'rake bower:install'.

The next step is to install devise with `rails g devise:install`, which generates config/initializers/devise.rb, user resources, user model, and a user migration. After installation of devise, you'll want to run the migration created by devise, by running `rake db:migrate`.

Then we need to make sure our data is in JSON format when being requested. So run `rails g serializer user` to generate a serializer on the user model. So now when a request for data is made, the response will contain JSON instead of the HTML template as Rails defaults to. Inside of the new serializer, we can make different attributes available in the API by adding them here.

You'll want to repeat creating migrations, models, and serializers for each model in your domain.

Next, we need to make sure Devise will respond with JSON by adding the following in config/application.rb directly under `class Application < Rails::Application:
config.to_prepare do
 DeviseController.respond_to :html, :json
end`

Now we need to configure a route which Angular can run on, by adding the following to the routes.rb file: `root ‘application#index’`.

Next, inside application_controller.rb, we'll need to tell rails to respond with JSON: `respond_to :json`, to trust devise without auth tokens: `skip_before_action :verify_authenticity_token`, and to render only one route.

Now, inside the users_controller.rb file, we need to make the user data available to the API in json.
`class UsersController < ApplicationController
def show
    user = User.find(params[:id])
    render json: user
  end 
end`

Next in the application.html.erb file, you'll want to remove the `<%= yield %>`, and replace with `<ui-view></ui-view>`. Rails uses yield to display templates, and Angular uses ui-view to display templates. After that, in the body tag add `ng-app = "myApp"`

Last thing we need to do in order to prep the rails backend to be used as an API is to set up the asset pipeline. Inside your application.js file, require the following: `//= require jquery //= require jquery_ujs //= require angular //= require angular-ui-router //= require angular-devise //= require angular-rails-templates //= require bootstrap-sprockets //= require_tree .`

This may seem like a long and tedious process, but it will get easier overtime with more practice. Now we have created a rails backend that will be able to talk to Angular, both serving and receiving data. To access the data in Angular, you'll want to make `$http` requests your rails server using custom services and using resolves in your router.

The goal is to learn something every day and hopefully you learned something from this post. Stay tuned in the future for a post on how to implement Angular.



