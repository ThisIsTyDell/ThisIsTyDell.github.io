---
layout: post
title:  "Introducing CutsNCurls, a Ruby on Rails Application"
date:   2016-10-24 23:59:39 +0000
---

> ***TL,DR*** *"CutsNCurls is an application for hair salons and barbershops to manage clients, and allow clients to schedule appointments. YouTube Video below, or link right here"
> *

I was tasked with creating a Ruby on Rails application that manages related data through complex forms and RESTful routes. The goal of the application is to build a Content Management System. Before creating the app I needed to figure out what I was going to make and HOW!? So I went to social media and to my family. Asking random people what type of app they wish they had.

I got a bunch of different answers. From creating a movie collection management site to even someone asking me to recreate Bookface.... Ahem.... Woah Not Yet. Eventually I asked my grandmother, who has an iPhone with just the default apps on it with nothing else. I knew her answer would be awesome, because she has no reference of other apps to compare it to. She was a beautician most of her life and wished there was an app to manage her clients. So I began to ask her about the "Hair World" and started the terminal... "Rails new CutsNCurls"

She had previously told me that a hair stylist has many clients, and usually the clients go to the same stylist upon each visit. This provided me with a starting point to build out my models and associate them to eachother.

In the app there are multiple models:
* User - self join model with clients (user) and stylist
 * A stylist has many clients
 * A client belongs to a stylist
 * has many appointments
 * has many time_slots through appointments
 * has many appointment_services through appointments
 * has many services through appointment_services
* Appointment
 * belongs to a user
 * belongs to a time_slot
 * has many appointment_services
 * has many services through appointment_services
* Category
 * has many services
* Service
 * belongs to a category
 * has many appointment_services
* TimeSlot
 * has many appointments
 * has many users through appointments

> While all of these associations may not be needed for the views for interaction through a web browser, they may come in handy if an Admin would like to use the application from the Terminal.

I inlisted Devise to handle user authentication and OmniAuth to allow login from a 3rd party provider. In this app I've only extended the ability to login through Facebook or by email. However it would be simple to add additional providers in the future, such as Google or Twitter. When a user signs up for an account they are automatically created as a normal user, redirected to the profile page, and given the option to provide their name and select a stylist.

The user model uses Pundit to handle authorization policy and allow different roles. Normal, Employee, and Admin. Each role provides different access levels to the application. For example, A user can view categories and services, create and cancle appointments, but can not create new services or categories. An employee can create and edit services and categories. Lastly, an Admin can view a list of all users and all appointments, CRUD any model, upgrade a normal user to an employee or even create other admins. The admin also has the ability to see who has paid and who has not paid for their appointment.

> The first inital admin can only be created through the terminal or other database management software.

Speaking of paid. This app takes advantage of the Stripe API to handle payments and all of the security issues that may go along. When a user visits their appointment page they are presented with a list of services they selected, with a total amount shown at the bottom. This amount is then passed on to Stripe to initiate payment. Upon successful payment the view refreshes to thank the customer and let them know they have paid.

This app has room to grow, improvement, and more features may be added in the future. Watch this video below for a brief walkthrough of the application.

<iframe width="560" height="315" src="https://www.youtube.com/embed/LUV3sodtUaQ" frameborder="0" allowfullscreen></iframe>


