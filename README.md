# Rails-Ruby-Cheatsheet
A self made cheatsheet of usefull commands in Rails 7 and Ruby 3.
please feel free to make proposals

## Database commands

### List database objects (tables) using rails console
```
ActiveRecord::Base.connection.tables 
=> # ["schema_migrations", "ar_internal_metadata", tables_of_your_project ]
```

## Generators
Here are some "most used" (for me) commands while using generators `rails generate <something>` like commands

### Models
Create a model
```
rails generate model <model_name_in_singular> attribute_1:<tipe_of_data>:<some_constraint> ...
```
Deletes a model

```
rails d model .... # deletes

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
