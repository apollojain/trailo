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

Step 2: Setting up the home page
===============================
Next, we want to generate the controller for our application. We want to run the following line in terminal: 
```
$ rails g controller home
```
What this does is actually create a controller for our Homepage. This basically allows the ruby backend to communicate with the Home view. We want to do two things now. First, go to
```
.../trailo/app/views/home/
```
and create a file called index.html.erb. In this file, we can add a single header: 
```html
<h1>Welcome to Trailo!</h1>
```
After this, we want to go to
```
.../trailo/config/routes.rb
```
and add the following line:
```
root :to => "home#index"
```

After this, we want to go to the file
```
.../trailo/app/controllers/home_controller.rb
```
and change the code to 
```
class HomeController < ApplicationController
	def index
		if user_signed_in?
			redirect_to :controller => "todos", :action => "index"
		end
	end
end
```
So, what does this do exactly? Well, basically, we set the root of our entire app to be the index of the home controller, or in other words, app/views/home/index.html.erb. What the controller is saying is that if our user is signed in, we'll redirect to the page app/views/todos/index.html.erb, which we will create soon. 

Step 3: Creating all of the To Do Stuff
===============================
The first thing we want to do is create our to do controller: 
```
$ rails g controller todos
```
After this, we want to go to
```
.../trailo/config/routes.rb
```
and add the following line:
```
resources:todos
```
Now, let's generate a model for our todo tasks. Enter the following in terminal:
```
$ rails g model todo name:string done:boolean
```

Step 4: Setting up the To Do Controller
======================================
Now, let's set up the To Do Controller. To do this, let's go to 
```
.../trailo/controllers/todos_controller.rb
```
We are now going to add a few functions. First, let us add an index method: 
```	
	def index
		@todos = Todo.where(done: false)
		@done = Todo.where(done: true)
	end
```
What this will do is filter out @todos as Todo items where done is false, and @done as ths ame, but done is true. 

Next, we will add yet another function
```	
	def new
		@todo = Todo.new
	end
```	
This creates a new Todo object for us. 

We will also create some Todo parameters, which basically defines what sorts of things the Todo object takes when you do an HTTP GET Request: 
```	
	def todo_params 
		params.require(:todo).permit(:name, :done)
	end
```	
Next, we will do Create, Update, and Destroy functions for our Todo Controller. All of these functions are pretty self-explanatory by their names. 
```	
	@todo = Todo.new(todo_params)

		if @todo.save
			redirect_to todos_path, :notice => "Your to do item was created!"
		else
			render "new"
		end
	end

	def update
		@todo = Todo.find(params[:id])

		if @todo.update_attribute(:done, true)
			redirect_to todos_path, :notice => "Your to do item was marked as done!"
		else
			redirect_to todos_path, :notice => "Your to do item was unable to be marked as done!"
		end
	end

	def destroy
		@todo = Todo.find(params[:id])
		@todo.destroy

		redirect_to todos_path, :notice => "Your to do item was deleted!"
	end
```	
Type in the following into terminal
```	
$ rake routes
```	
to see what all of the paths are. 


Step 5: Setting up the To Do View
==================================
Now, go to
```
.../trailo/app/views/todos/
```
and create a file called index.html.erb. In this file, we can add a single header: 
```html
<h1>Trailo items</h1>

<h2>To do:</h2>
<% @todos.each do |t| %>
	<p>
		<strong><%= t.name %></strong>
		<small><%= link_to "Mark as Done", todo_path(t), :method => :put %></small>
	</p>
<%end %>

<h2>Complete:</h2>
<% @done.each do |t| %>
	<p>
		<%= t.name %>
		<small><%= link_to "Remove", t, :confirm => "is it ok to remove this from the list?", :method => :delete %></small>
	</p>
<% end %>

<p><%= link_to "Add an item to the list", new_todo_path %></p>
```
Basically, the above code has embedded Ruby that loops through all of your incomplete and complete todo objects and injects HTML into your view. 

Next, go to
```
.../trailo/app/views/todos/
```
and create a file called new.html.erb. In this file, we can add a single header: 
```html
<h1>Add new item to your Trailo To do list!</h1>
<%= form_for @todo, :url => todos_path(@todo) do |f| %>
	<%= f.label :name %>: <%= f.text_field :name %>
	<%= f.hidden_field :done, :value => false %>
	<%= f.submit "Add to todo list" %>
<%end %>
```

Lastly, we want to change the file 
```
.../trailo/app/views/layouts/application.html.erb
```
in order to allow for users to log in and log out. 
```
<!DOCTYPE html>
<html>
<head>
  <title>Trailo</title>
  <%= stylesheet_link_tag    'application', media: 'all', 'data-turbolinks-track' => true %>
  <%= javascript_include_tag 'application', 'data-turbolinks-track' => true %>
  <%= csrf_meta_tags %>
</head>
<body>

<% if user_signed_in? %>
	You are currently signed in as <%= current_user.email %>, not you? 
	<%= link_to "Sign out", destroy_user_session_path, :method => :delete %>
<%else %>
	<%= link_to "Sign up", new_user_registration_path %> or <%= link_to "Sign in", new_user_session_path %>.
<%end %>

<% flash.each do |name, message| %>
	<%= content_tag :div, message, :id => "flash_{name}" %>
<%end%>

<%= yield %>

</body>
</html>
```
