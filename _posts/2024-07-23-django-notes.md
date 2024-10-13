---
title: Django Notes
date: 2024-07-23 11:00:00 +1000
categories: [Programming, Django]
tags: [django, backend development, mysql]
description: "In-depth notes on setting up a database and working with the Django framework, including configuring models, migrations, and the ORM for efficient data management."
---


## Database

```shell
# start database
$ sudo /usr/local/mysql/support-files/mysql.server start

# log in to MySQL shell
$ /usr/local/mysql/bin/mysql -u root -p
```

```sql
-- create new database
mysql> CREATE DATABASE money_guardian_db;

-- create new user and grant priviledges to new database
mysql> CREATE USER 'new_username'@'localhost' IDENTIFIED BY 'new_password';
mysql> GRANT ALL PRIVILEGES ON money_guardian_db.* TO 'new_username'@'localhost';
mysql> FLUSH PRIVILEGES;

-- grant privilege for test database
mysql> GRANT ALL PRIVILEGES ON *.* TO 'mg_admin'@'localhost';
mysql> FLUSH PRIVILEGES;

mysql> EXIT;
```

configure environment variable for MySQL

```shell
$ nano ~/.zshrc

# add these lines into file
# Ctrl-O + ENTER = save file
# Ctrl-X = exit
$ export PATH="/usr/local/mysql/bin:$PATH"
$ export DYLD_LIBRARY_PATH="/usr/local/mysql/lib:$DYLD_LIBRARY_PATH"
$ export LDFLAGS="-L/usr/local/mysql/lib"
$ export CPPFLAGS="-I/usr/local/mysql/include"

# source shell configuration
$ source ~/.zshrc
```

**initialise database**

```shell
# create a custom migraiton file
$ python manage.py makemigrations --empty currency --name add_initial_currency_data

# update file as python code below, then migrate
$ python manege.py migrate

# empty database
$ python manage.py flush
```

**edit migration**

```python
# edit migration file in records/migrations/ directory
def create_initial_currency_data(apps, schema_editor):
    Currency = apps.get_model('records', 'Currency')
    Currency.objects.create(code='AUD')

class Migration(migrations.Migration):
    dependencies = [...] # don't change this

    operations = [
        migrations.RunPython(create_initial_currency_data),
    ]
```

## Django Framework

Django is a high-level Python web framework that encourages rapid development and clean, pragmatic design. It follows the Model-View-Template (MVT) architectural pattern.

- **Model**: Represents the data structure and database schema (database tables)
- **View**: Handles the logic and interacts with the model to fetch data and render it to templates
- **Template**: HTML files that render the data provided by views into a format suitable for user interaction

How Django Framework Works

1. Request Handling:
   - a user makes a request to a Django-powered website
   - Django determines which view to call based on the URL configuration (`urls.py`)
2. View Processing:
   - the view processes the request, it may interact with the model to fetch data from the database
   - the view uses serializers (if it’s an API view) to convert model instances to JSON
   - the view returns a response, which could be an HTML page or a JSON object
3. Template Rendering:
   - if the view returns an HTML response, it renders a template with the context data
4. Model Interaction:
   - models define the structure of the database and interact with it
   - changes in the model are applied to the database through migrations
5. Admin Interface:
   - the `admin.py` file registers models with the Django admin site, providing a web interface to manage the data

purpose of each file in app

- `admin.py`
  - used to register models with the Django admin site, allowing you to manage your models through a web interface
- `apps.py`
  - contains the configuration for the app
  - it is used to set the name and other configuration options for the app
- `models.py`
  - this file contains the definitions of your database models
  - each model represents a table in the database
- `serializers.py`
  - this file contains the serializers, which convert model instances to and from JSON or other content types
  - serializers are essential for creating APIs
- `views.py`
  - this file contains the views for your app
  - views handle requests and return responses
  - they interact with models and serializers to fetch and process data
- `urls.py`
  - this file contains the URL routing configuration for your app
  - it maps URLs to views

```shell
$ django-admin startproject [name]
$ cd [name]
$ django-admin startapp [app_name]
```

## Others

```shell
# write dependencies
$ pip freeze > requirements.txt

# export swagger api to json
$ curl -o swagger.json http://localhost:8000/swagger.json
```
