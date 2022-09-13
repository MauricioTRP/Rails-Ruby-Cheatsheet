# Rails-Ruby-Cheatsheet
A self made cheatsheet of usefull commands in Rails

## Database commands

### List database objects (tables) using rails console
```
ActiveRecord::Base.connection.tables 
=> # ["schema_migrations", "ar_internal_metadata", tables_of_your_project ]
```
