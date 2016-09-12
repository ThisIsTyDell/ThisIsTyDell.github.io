---
layout: post
title:  "Developing my first Sinatra CRUD App. "
date:   2016-09-12 21:34:46 +0000
---


I just learned how to develop using Sinatra. Sinatra is like a wrapper for Rack adding some awesome capabilities. I won't talk about them much here, but, man does it make things a little easier than doing it from scratch. You can even bring in ActiveRecord too to handle the ORM. Sinatra can be used to make different types of web apps, but this week I was tasked with creating my very own CRUD application.

CRUD. Create. Retrieve. Update. Delete. (Or my inner child likes to yell in a monster voice DESTORYYY!!!). I can't help it, you gotta have fun. If you're not having fun coding, then maybe this isn't for you, but that's another blog post for another day. So CRUD uses a set of ActiveRecord methods to associate objects together and persist them to the database. A CRUD application can be anything that implements adding, reading, updating, or removing data. Such as a to-do list, grocery list, shopping cart, and a number of other different possibilities. I chose to go with something music themed.

My Sinatra CRUD app is called Roadie. Roadie is a road managament app designed with bands in mind. Every band needs some sort of way of keeping track of all the gear that gets taken with them on the road. Users can sign up and create Trucks to represent their real life trucks or busses or whatever transportation method they use. Then after a new truck is made they can then add items or equipment to that truck. A user can have many trucks, and a truck can have many items. 

Roadie will also allow a user to edit items or trucks once already created. It's easy to move an item to another truck without needing to recreate the item. A user can also delete all items, all trucks and its items, or even delete themselves as a user if the no longer wish to use the app.

This app is open to contribution and is encouraged. I may continue refactoring and adding in more functionality as I progress through [Flatiron School's Learn.co.](http://www.learn.co) and continue learning.

Check out my Sinatra app over at GitHub. https://github.com/ThisIsTyDell/roadie-sinatra-app
