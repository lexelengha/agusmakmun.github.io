---
layout: post
title:  "Ruby on rails M-V-C architecture"
date:   2021-07-01 17:00:00 +0200
categories: [ruby, rails]
---

# Rails
Ruby on rails is an open source web framework written in the Ruby programming language, and all the applications in Rails are written in Ruby. It is very popular framework for web development due to its amazing features. This framework has built-in solutions to many common problems that developer face during web development.

Ruby on Rails uses the Model-View-Controller `(MVC)` architectural pattern. `MVC` is a pattern for the architecture of a software application. It separates an application into the following components:

* Models for handling data and business logic.
* Controllers for handling the user interface and application.
* Views for handling graphical user interface objects and presentation.

This separation results in user requests being processed as follows:

  1.`The browser sends a request for a page to the controller on the server.`
  2.`The controller retrieves the data it needs from the model in order to respond to the request.`
  3.`The controller gives the retrieved data to the view.`
  4.`The view is rendered and sent back to the client for the browser to display.`

Each component of the model-view-controller architecture has its place within the app directory—the models, views, and controllers sub directories respectively.

# ActiveRecord
`ActiveRecord` is the module for handling business logic and database communication. It plays the role of model in our `MVC` architecture.

`ActiveRecord` is designed to handle all of an application's tasks that relate to the database, including:

  * establishing a connection to the database server
  * retrieving data from a table
  * storing new data in the database.

# Controllers
The `ActionController` is a module which handles the application logic of your program, acting as glue between the application’s data, the presentation layer, and the web browser.

# Views
The principle of `MVC` is that a view should contain presentation logic only. This principle holds that the code in a view should only perform actions that relate to displaying pages in the application; none of the code in a view should perform any complicated application logic, nor store or retrieve any data from the database. In Rails, everything that is sent to the web browser is handled by a view.

A view need not actually contain any `Ruby` code at all — it may be the case that one of your views is a simple `HTML` file; however, it’s more likely that your views will contain a combination of `HTML` and `Ruby` code, making the page more dynamic. The `Ruby` code is embedded in `HTML` using embedded `Ruby` syntax.