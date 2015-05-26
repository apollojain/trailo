Introduction
======================
Welcome to the Ruby on Rails tutorial!

This is a guide that will help you through your first Ruby on Rails application. We'll be building Trailo©, a lightweight, simpler version of Trello.

After you finish this, you'll have:

* Created a Rails App
* Familiarized yourself with the basics of MVC conventions
* Used a web form to send information to a web server

Acknowledgements
----------------------
* Thanks to Mike Hibbert for the inspiration to create the tutorial

Step 0: Before you begin
=========================
Prior to starting this tutorial, make sure you have Ruby on Rails set up on your computer. Keep in mind that Ruby on Rails is exponentially easier to set up on Unix-based systems than it is on Windows-based systems; still, there are ways to set it up on Windows-based computers as well. 

Here are some guides on how to set up Rails on <a href="https://s3.amazonaws.com/railsinstaller/Windows/railsinstaller-3.1.0.exe">Windows</a>, <a href="https://gorails.com/setup/osx/10.10-yosemite">Max OS X</a>, and <a href="https://gorails.com/setup/ubuntu/14.04">Ubuntu</a>. Please set up Rails BEFORE the workshop. The setup is honestly a bit time consuming, and takes about 30-45 minutes. We will unfortunately not be stopping if you need help. If you are unable to set up Rails, StackOverflow honestly will be able to help with just about any problem relating to the setup. 

So now, a little bit on what Rails is and what the Model View Controller(MVC) convention is. Rails is essentially an open source web application framework that allows you to easily set up a web application, as it provides default structures for static pages, database integration, packages, and plugins. This makes it ridiculously easy to get started and to do rapid prototyping. 

In order to understand what an MVC is, let us examine a Facebook Post. A <b>Controller</b> basically sends commands to the model to update its state. So, when you hit the "post" button on a status update, a controller is basically changing the content of a model. A <b>Model</b>, which stores data that is retrieved by the controller and displayed by the view. So, the model for a Status would consist of a String and a Text Block, where the String is the name of the poster, and the Text Block is the contents of the post. The <b>View</b> is how the Model is actually displayed. So, when you log into Facebook, and you see people posting their Statuses on their walls, this is the View. 

Step 1: Creating the app and setting up dependancies. 
=========================
So, the first thing you want to do is actually create the application. In order to do this, you want to enter the following line in a directory of your choice: 
```
$ rails new trailo
```
What this will do is actually create a new Rails application called trailo. Now, go into your new trailo directory and open a file called "Gemfile". At the bottom of the file, include the following line: 
```
gem 'devise'
```
This line will place a package that allows for user authenficiation inside of your Rails application. Next, cd into your "trailo" directory, or in other words into the directory 
```
.../trailo/
```
and run the following commands:
```
$ bundle install
$ bin/rails g devise:install
$ bin/rails g devise user
$ bin/rake db:migrate
```
These lines install the devise package and then sets up a user model for us to use later on. 