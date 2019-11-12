# README

This README would normally document whatever steps are necessary to get the
application up and running.

Things you may want to cover:

* Ruby version Feature branch

* System dependencies

* Configuration

* Database creation

* Database initialization

* How to run the test suite

* Services (job queues, cache servers, search engines, etc.)

* Deployment instructions

* ...

### Set up the Back End

1. Build a new rails app (called “re-former”).
```sh
Done!
```
2. Go into the folder and create a new git repo there. Check in and commit the initial stuff.
```sh
Done!
```

3. Modify your README file to say something you’ll remember later, like “This is part of the Forms Project in The Odin Project’s Ruby on Rails Curriculum. Find it at http://www.theodinproject.com”
```sh
Done!
```

4. Create and migrate a User model with :username, :email and :password.
```sh
rails generate model User username:string, email:string, password:string

Running via Spring preloader in process 2376
      invoke  active_record
      create    db/migrate/20191111194457_create_users.rb
      create    app/models/user.rb
      invoke    test_unit
      create      test/models/user_test.rb
      create      test/fixtures/users.yml
```

5. Add validations for presence to each field in the model.
```sh
class User < ApplicationRecord
  validates :username, presence: true
  validates :email, presence: true
  validates :password, presence: true
end
```

6. Create the :users resource in your routes file so requests actually have somewhere to go. Use the :only option to specify just the :new and :create actions.
```sh
Rails.application.routes.draw do
  # For details on the DSL available within this file, see https://guides.rubyonrails.org/routing.html
  resource :users, only: [:new, :create]
end
```

7. Build a new UsersController (either manually or via the $ rails generate controller Users generator).
```sh
rails generate controller Users

Running via Spring preloader in process 2537
      create  app/controllers/users_controller.rb
      invoke  erb
      create    app/views/users
      invoke  test_unit
      create    test/controllers/users_controller_test.rb
      invoke  helper
      create    app/helpers/users_helper.rb
      invoke    test_unit
      invoke  assets
      invoke    scss
      create      app/assets/stylesheets/users.scss
```

8. Write empty methods for #new and #create in your UsersController.
```sh
class UsersController < ApplicationController
  def new
  end

  def create
  end
end
````

9. Create your #new view in app/views/users/class UsersController < ApplicationController
```sh
<h1>Sign up</h1>
<div class="row">
  <div class="col-md-6 col-md-offset-3">
    <p>Sign up please!<p>
  </div>
</div>
```

10. Fire up a rails server in another tab.
```sh
rails db:migrate

Done!
```

11. Make sure everything works by visiting http://localhost:3000/users/new in the browser.
```sh
Page loads.
```

### HTML Form

1. Build a form for creating a new user. See the w3 docs for forms if you’ve totally forgotten how they work. Specify the method and the action attributes in your <form> tag (use $ rake routes to see which HTTP method and path are being expected based on the resource you created). Include the attribute accept-charset="UTF-8" as well, which Rails naturally adds to its forms to specify Unicode character encoding.

2. Create the proper input tags for your user’s fields (email, username and password). Use the proper password input for “password”. Be sure to specify the name attribute for these inputs. Make label tags which correspond to each field.
```sh
<form accept-charset="UTF-8" action="/users" method="post" >

  Username: <input type="text" name="username"><br>
  Email: <input type="email" name="email"><br>
  Password: <input type="password" name="password"><br>

  <input type="submit" value="Submit">
</form>
```

3. Submit your form and view the server output. Oops, we don’t have the right CSRF authenticity token (ActionController::InvalidAuthenticityToken) to protect against cross site scripting attacks and form hijacking. If you do not get an error, you used the wrong method from step 1.

4. Include your own authenticity token by adding a special hidden input and using the #form_authenticity_token method. This method actually checks the session token that Rails has stored for that user (behind the scenes) and puts it into the form so it’s able to verify that it’s actually you submitting the form. It might look like:
```sh
<form accept-charset="UTF-8" action="/users/new" method="post" >
  <input type="hidden" name="authenticity_token" value="<%= form_authenticity_token %>">

  Username: <input type="text" name="username"><br>
  Email: <input type="email" name="email"><br>
  Password: <input type="password" name="password"><br>

  <input type="submit" value="Submit">
</form>
```

5. Submit the form again. Great! Success! We got a Template is missing error instead and that’s A-OK because it means that we’ve successfully gotten through our blank #create action in the controller (and didn’t specify what should happen next, which is why Rails is looking for a app/views/users/create.html.erb view by default). Look at the server output above the error’s stack trace. It should include the parameters that were submitted, looking something like:

```sh
Started POST "/users" for ::1 at 2019-11-11 12:29:51 -0800
Processing by UsersController#create as HTML
  Parameters: {"authenticity_token"=>"M2c0d39cFIN4cAlqkx2zJwtwgfX7NNjw/Pp3xhBCVD+0LVQNIOE3pDpaWQyGJSfY9zMryuUhJxaTrys/l5/qAA==", "username"=>"christian", "email"=>"jcromerohdz@gmail.com", "password"=>"[FILTERED]"}
No template found for UsersController#create, rendering head :no_content
Completed 204 No Content in 1ms (ActiveRecord: 0.0ms | Allocations: 565)
```

6. Go into your UsersController and build out the #create action to take those parameters and create a new User from them. If you successfully save the user, you should redirect back to the New User form (which will be blank) and if you don’t, it should render the :new form again (but it will still have the existing information entered in it). You should be able to use something like:
```sh
 def create
    @user = User.new(username: params[:username], email: params[:email], password: params[:password])

    if @user.save
      redirect_to new_users_path
    else
      render :new
    end
  end
```

7. Test this out – can you now create users with your form? If so, you should see an INSERT SQL command in the server log.
```sh
Started POST "/users" for ::1 at 2019-11-11 12:49:53 -0800
Processing by UsersController#create as HTML
  Parameters: {"authenticity_token"=>"xaG1MKqdX7i5zPDL1vIrEiv8IYhfFxtlT1HZKGvOgrZC69VK9SB8n/vmoK3Dyr/t17+Lt0EC5IMgBIXR7BM8iQ==", "username"=>"christian 2", "email"=>"jcromerohdz@gmail.com", "password"=>"[FILTERED]"}
   (0.0ms)  begin transaction
  ↳ app/controllers/users_controller.rb:8:in `create'
  User Create (0.2ms)  INSERT INTO "users" ("username", "email", "password", "created_at", "updated_at") VALUES (?, ?, ?, ?, ?)  [["username", "christian 2"], ["email", "jcromerohdz@gmail.com"], ["password", "12345"], ["created_at", "2019-11-11 20:49:53.210222"], ["updated_at", "2019-11-11 20:49:53.210222"]]
  ↳ app/controllers/users_controller.rb:8:in `create'
   (1113.8ms)  commit transaction
  ↳ app/controllers/users_controller.rb:8:in `create'
Redirected to http://localhost:3000/users/new
Completed 302 Found in 1126ms (ActiveRecord: 1114.4ms | Allocations: 10469)


Started GET "/users/new" for ::1 at 2019-11-11 12:49:54 -0800
Processing by UsersController#new as HTML
  Rendering users/new.html.erb within layouts/application
  Rendered users/new.html.erb within layouts/application (Duration: 0.0ms | Allocations: 20)
Completed 200 OK in 3ms (Views: 2.3ms | ActiveRecord: 0.0ms | Allocations: 3582)
```
8. We’re not done just yet… that looks too long and difficult to build a user with all those params calls. It’d be a whole lot easier if we could just use a hash of the user’s attributes so we could just say something like User.new(user_params). Let’s build it… we need our form to submit a hash of attributes that will be used to create a user, just like we would with Rails’ form_for method. Remember, that method submits a top level user field which actually points to a hash of values. This is simple to achieve, though – just change the name attribute slightly. Nest your three User fields inside the variable attribute using brackets in their names, e.g. name="user[email]".

9. Resubmit. Now your user parameters should be nested under the "user" key like:

10. You’ll get some errors because now your controller will need to change. But recall that we’re no longer allowed to just directly call params[:user] because that would return a hash and Rails’ security features prevent us from doing that without first validating it.

11. Go into your controller and comment out the line in your #create action where you instantiated a ::new User (we’ll use it later).

12. Implement a private method at the bottom called user_params which will permit and require the proper fields (see the Controllers Lesson for a refresher).
```sh
private

  def user_params
    params.require(:user).permit(:username, :email, :password)
  end
```
13. Add a new ::new User line which makes use of that new whitelisting params method.
```sh
def create
    @user = User.new(user_params)
    .
    .
    .
```
14. Submit your form now. It should work marvelously (once you debug your typos)!
```sh
Started POST "/users" for ::1 at 2019-11-11 13:11:08 -0800
Processing by UsersController#create as HTML
  Parameters: {"authenticity_token"=>"F/OJXsYO93Xwuupo/1IiKHUaG9ZIRr4Q9fI/DMe1X+uQuekkmbPUUrKQug7qarbXiVmx6VZTQfaap2P1QGjh1A==", "user"=>{"username"=>"chriss-de-blanc", "email"=>"jcromerohdz@gmail.com", "password"=>"[FILTERED]"}}
   (0.1ms)  begin transaction
  ↳ app/controllers/users_controller.rb:8:in `create'
  User Create (0.3ms)  INSERT INTO "users" ("username", "email", "password", "created_at", "updated_at") VALUES (?, ?, ?, ?, ?)  [["username", "chriss-de-blanc"], ["email", "jcromerohdz@gmail.com"], ["password", "1234"], ["created_at", "2019-11-11 21:11:08.620699"], ["updated_at", "2019-11-11 21:11:08.620699"]]
  ↳ app/controllers/users_controller.rb:8:in `create'
   (384.9ms)  commit transaction
  ↳ app/controllers/users_controller.rb:8:in `create'
Redirected to http://localhost:3000/users/new
Completed 302 Found in 396ms (ActiveRecord: 385.8ms | Allocations: 8376)


Started GET "/users/new" for ::1 at 2019-11-11 13:11:09 -0800
Processing by UsersController#new as HTML
  Rendering users/new.html.erb within layouts/application
  Rendered users/new.html.erb within layouts/application (Duration: 0.1ms | Allocations: 20)
Completed 200 OK in 3ms (Views: 2.7ms | ActiveRecord: 0.0ms | Allocations: 3584)
```

### Railsy-er Forms with #form_for

1. Modify your #new action in the controller to instantiate a blank User object and store it in an instance variable called @user.
```sh
class UsersController < ApplicationController
  def new
    @user = User.new
  end

  def create
    @user = User.new(user_params)

    if @user.save
      redirect_to new_users_path
    else
      render :new
    end
  end

  private

  def user_params
    params.permit(:username, :email, :password)
  end
end
```

2. Comment out your #form_tag form in the app/views/users/new.html.erb view (so now you should have TWO commented out form examples).

3. Rebuild the form using #form_for and the @user from your controller.
```sh
<h1> Over here </h1>
<%= form_tag('/users') do %>
 <%= label_tag(:username, 'Enter your username:') %>
 <%= text_field_tag(:username) %><br>
 <%= label_tag(:email, 'Enter your email:') %>
 <%= text_field_tag(:email) %><br>
 <%= label_tag(:password, 'Enter your password:') %>
 <%= password_field_tag(:password) %><br>
 <%= submit_tag('Submit') %>
<% end %>
```

4. Play with the #input method options – add a default placeholder (like “example@example.com” for the email field), make it generate a different label than the default one (like “Your user name here”), and try starting with a value already populated. Some of these things you may need to Google for, but check out the #form_for Rails API docs
```shi
<%= form_tag('/users') do %>
 <%= label_tag(:username, 'Enter your username:') %>
 <%= text_field_tag(:username, nil, placeholder: 'Your user name here' ) %><br>
 <%= label_tag(:email, 'Enter your email:') %>
 <%= text_field_tag(:email, nil, placeholder: 'example@example.com') %><br>
 <%= label_tag(:password, 'Enter your password:') %>
 <%= password_field_tag(:password) %><br>
 <%= submit_tag('Submit') %>
<% end %>
```

5. Test it out. You’ll need to switch your controller’s #create method again to accept the nested :user hash from params.
```sh
It works!!
```

# Railsy-er Forms with #form_for
#form_tag probably didn’t feel that useful – it’s about the same amount of work as using <form>, though it does take care of the authenticity token stuff for you. Now we’ll convert that into #form_for, which will make use of our model objects to build the form.

1. Modify your #new action in the controller to instantiate a blank User object and store it in an instance variable called @user.
```sh
class UsersController < ApplicationController
  def new
    @user = User.new
  end
  .
  .
  .
```
2. Comment out your #form_tag form in the app/views/users/new.html.erb view (so now you should have TWO commented out form examples).

3. Rebuild the form using #form_for and the @user from your controller.

4. Play with the #input method options – add a default placeholder (like “example@example.com” for the email field), make it generate a different label than the default one (like “Your user name here”), and try starting with a value already populated. Some of these things you may need to Google for, but check out the #form_for Rails API docs
```sh
<h1>Sign up</h1>
  <%= form_for(@user) do |f| %>        
        <%= f.label :username %>
        <%= f.text_field :username, placeholder: 'Your user name here', class: 'form-control' %>
        <br>
        <%= f.label :email %>
        <%= f.email_field :email, placeholder: 'Your email@here.com', class: 'form-control' %>
        <br>
        <%= f.label :password %>
        <%= f.password_field :password, class: 'form-control' %>
        <br>
        <%= f.submit "Submit", class: "btn btn-outline-primary" %>
  <% end %>
```

5. Test it out. You’ll need to switch your controller’s #create method again to accept the nested :user hash from params.
```sh
It work!
```

### Editing

1. Update your routes and controller to handle editing an existing user. You’ll need your controller to find a user based on the submitted params ID.
```sh
Rails.application.routes.draw do
  # For details on the DSL available within this file, see https://guides.rubyonrails.org/routing.html
  resources :users, :only => [:new, :create, :edit, :update]
end

```

2. Create the Edit view at app/views/users/edit.html.erb and copy/paste your form from the New view. Your HTML and #form_tag forms (which should still be commented out) will not work – they will submit the form as a POST request when you need it to be a PATCH (PUT) request (remember your $ rake routes?). It’s an easy fix, which you should be able to see if you attempt to edit a user with the #form_for form (which is smart enough to know if you’re trying to edit a user or creating a new one).
```sh
.
.
.
  def edit
    @user = User.find(params[:id])
  end

  def update
    @user = User.find(params[:id])
    if 
      @user.update(user_params)
      redirect_to edit_user_path(@user)
    else
      render :edit
   end
.
.
.
```

3. Do a “view source” on the form generated by #form_for in your Edit view, paying particular attention to the hidden fields at the top nested inside the <div>. See it?
```sh
<form class="edit_user" id="edit_user_2" action="/users/2" accept-charset="UTF-8" method="post"><input type="hidden" name="_method" value="patch"><input type="hidden" name="authenticity_token" value="/glXj9ZWM3S1IiSXnA0ETclwT8oPZM5jCRy8vLk09xKEUQ90Ol3DIbbNWt/CuRJc2LoFWCYn1nEdVV6RwNW5lg==">        
  <label for="user_username">Username</label>
  <input placeholder="Your username" class="form-control" type="text" value="zamzam" name="user[username]" id="user_username">
  <br>
  <label for="user_email">Email</label>
  <input placeholder="Your email" class="form-control" type="email" value="zam@email.com" name="user[email]" id="user_email">
  <br>
  <label for="user_password">Password</label>
  <input class="form-control" type="password" name="user[password]" id="user_password">
  <br>
  <input type="submit" name="commit" value="Submit" class="btn btn-outline-primary" data-disable-with="Submit">
</form>
```

4. Try submitting an edit that you know will fail your validations. #form_for also automatically wraps your form in some formatting (e.g. a red border on the input field) if it detects errors with a given field.
```sh
Started PATCH "/users/2" for ::1 at 2019-11-13 04:15:43 +0800
Processing by UsersController#update as HTML
  Parameters: {"authenticity_token"=>"yYOk+HPaQsWHk/8YlH5giMM31yqXi4qlpWI2yQIyQq6z2/wDn9GykIR8gVDKynaZ0v2duL7IkrexK9Tke9MMKg==", "user"=>{"username"=>"zamzam", "email"=>"zam@email.com", "password"=>"[FILTERED]"}, "commit"=>"Submit", "id"=>"2"}
  User Load (1.1ms)  SELECT "users".* FROM "users" WHERE "users"."id" = ? LIMIT ?  [["id", 2], ["LIMIT", 1]]
  ↳ app/controllers/users_controller.rb:21:in `update'
   (0.7ms)  begin transaction
  ↳ app/controllers/users_controller.rb:23:in `update'
  User Update (304.8ms)  UPDATE "users" SET "password" = ?, "updated_at" = ? WHERE "users"."id" = ?  [["password", "password"], ["updated_at", "2019-11-12 20:16:21.043999"], ["id", 2]]
  ↳ app/controllers/users_controller.rb:23:in `update'
   (99.8ms)  commit transaction
  ↳ app/controllers/users_controller.rb:23:in `update'
Redirected to http://localhost:3000/users/2/edit
Completed 302 Found in 561ms (ActiveRecord: 406.3ms | Allocations: 5608)


Started GET "/users/2/edit" for ::1 at 2019-11-13 04:16:21 +0800
Processing by UsersController#edit as HTML
  Parameters: {"id"=>"2"}
  User Load (1.0ms)  SELECT "users".* FROM "users" WHERE "users"."id" = ? LIMIT ?  [["id", 2], ["LIMIT", 1]]
  ↳ app/controllers/users_controller.rb:17:in `edit'
  Rendering users/edit.html.erb within layouts/application
  Rendered users/edit.html.erb within layouts/application (Duration: 61.8ms | Allocations: 887)
Completed 200 OK in 150ms (Views: 129.5ms | ActiveRecord: 1.0ms | Allocations: 4790)
```

5. Save this project to Git and upload to Github.
```sh
Done!
```
# Extra Credit
1. Modify your form view to display a list of the error messages that are attached to your failed model object if you fail validations. Recall the #errors and #full_messages methods. Start by displaying them at the top and then modify
```sh
<h1>Sign up</h1>
<%= form_for(@user) do |f| %>
  <ul>
    <% @user.errors.full_messages.each do |error| %>
    <li style="color:red"><%= error %></li>
    <% end %>
  </ul>
  .
  .
  .

Then for Edit
<h1>Edit User</h1>
<%= form_for(@user, url: user_path) do |f| %> 
  <ul>
    <% @user.errors.full_messages.each do |error| %>
    <li style="color:red"><%= error %></li>
    <% end %>
  </ul>     
```
  

