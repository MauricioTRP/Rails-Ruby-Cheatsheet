# Rails-Ruby-Cheatsheet
A self made cheatsheet of usefull commands in Rails 7 and Ruby 3.
please feel free to make proposals

## Basic security
### UUID (Universal User Identifier)
We'll use a UUID instead of the standard incremental integer for the ID of the postgres record to make the (id encrypted)[https://betterprogramming.pub/empowering-a-rails-application-with-uuid-as-default-primary-key-44cd740828e8].


1. You have to run the follow migration `rails g migration enable_uuid`
``` 
      invoke  active_record
      create    db/migrate/20220927142009_enable_uuid.rb
```
then add this line to the migration file `enable_extension 'pgcrypto'`
```
# frozen_string_literal: true

class EnableUuid < ActiveRecord::Migration[7.0]
  def change
    enable_extension 'pgcrypto'
  end
end
```
2. Implement the initializer
We have to tell our generator that we're going to use uuid instead of incremental integer. So you have to create the file: `config/initializers/generators.rb` 
``` 
# frozen_string_literal: true

Rails.application.config.generators do |g|
  g.orm :active_record, primary_key_type: :uuid
end
```
 Now when you run `rails db:create db:migrate` it will be configured to automatically use UUID as primary_key.
 
 
 3. Devise 
 
 Just follow the documentation to install. 
 Add `gem 'devise'` to your `Gemfile` and run bundle install.
 
 then run `rails devise:install` and follow the recomendations that appears in the console.
 
 create the controller `turbo_devise_controller.rb` 
 
 ```
 # app/controllers/turbo_devise_controller.rb
 
 class TurboDeviseController < ApplicationController
  class Responder < ActionController::Responder
    def to_turbo_stream
      controller.render(options.merge(formats: :html))
    rescue ActionView::MissingTemplate => error
      if get?
        raise error
      elsif has_errors? && default_action
        render rendering_options.merge(formats: :html, status: :unprocessable_entity)
      else
        redirect_to navigation_location
      end
    end
  end

  self.responder = Responder
  respond_to :html, :turbo_stream
end
 ```


## Test Driven Development setup

In order to make unitary test in our app, we have to install and setup several gems to make the tests

### 

## Database commands

### List database objects (tables) using rails console
```
ActiveRecord::Base.connection.tables 
=> # ["schema_migrations", "ar_internal_metadata", tables_of_your_project ]
```

## Generators
Here are some "most used" (for me) commands while using generators `rails generate <something>` like commands

### Models
```
rails generate model <model_name_in_singular> attribute_1:<tipe_of_data>:<some_constraint> ...
```

#### Examples

Here it will generate a model and a migration of a "degrees" table with a "degree" field wich will be in our database after use `rails db:migrate`. We will constrint the degree field to have uniques records and string data type.
```
rails generate model Degree degree:string:uniq
```
##### output

```
  invoke  active_record
  create    db/migrate/[date_stamp]_create_degrees.rb
  create    app/models/degree.rb
```

This other simple example will create a "users" table with fiels name, email, comments, hobbies, description, hobs. 
```
rails generate model User name:string email:string comments:text hobbies:string description:text jobs:string

```
#### Join Table
So now, a user can have multiple degrees, wich can be generated in a join table as:

```
rails generate migration UsersDegreeJoinTable users_degrees users_degree:uniq

```
