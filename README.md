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

HTML Form
The first form you build will be mostly HTML (remember that stuff at all?). Build it in your New view at app/views/users/new.html.erb. The goal is to build a form that is almost identical to what you’d get by using a Rails helper so you can see how it’s done behind the scenes.

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