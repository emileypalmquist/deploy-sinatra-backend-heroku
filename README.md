# Prerequisites
- install postgesql
- install the heroku cli

# Change database from sqlite3 to postgresql
in the gemfile remove: 
```ruby
gem "sqlite3"
```
in the terminal run:
```
bundle add pg
```
next we will need to change the db.yml file to look something like:
```
default: &default
  adapter: postgresql
  encoding: unicode
  pool: <%= ENV.fetch("RAILS_MAX_THREADS") { 5 } %>
  timeout: 5000

development:
  <<: *default
  database: your_project_name_development

test:
  <<: *default
  database: your_project_name_test

production:
  <<: *default
  database: your_project_name_production
```
# Run your app to make sure it is still running after the database change
```
rake db:create db:migrate db:seed
rake server
```

# Deploying to heroku
First run the following command to add additional platforms to your Gemfile.lock
```
 bundle lock --add-platform x86_64-linux
```
next we need to commit the changes
```
git add .
git commit -m "switched db from sqlite3 to posgresql"
```
after we have commited our changes we can create a new heroku app with the heroku cli
```
heroku create
```
Next we need to push the code to the heroku repository that was created with the above command.
First run the command
```
git branch --show-current
```
if it printed out main
```
git push heroku main
```
if it printed out master
```
git push heroku master
```
Next we need to migrate and seed the database on the server
```
heroku run rake db:migrate db:seed
```
finally run:
```
heroku open
```
