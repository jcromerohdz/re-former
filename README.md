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