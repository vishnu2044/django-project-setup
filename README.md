# Django Project Setup

This file provides guidance on setting up a Django project using Poetry.

## Steps to Work with the Repository

#### 1. Create a Django Application
Initialize your Django project by running:
```shell
django-admin startproject <project_name>
```
---------------------------------------
#### 2. Add Essential Files
- Add a gitignore file
- add README file

---------------------------------------
#### 3. Setup poetry

  Install poetry package manager (https://python-poetry.org/docs/#installation)
```shell
poetry init
```
---------------------------------------

#### 4. Add dependencies

  You can add dependencies based on your project needs.
```shell
# Sample 
  poetry add Django djangorestframework django-filter
```
---------------------------------------
#### 5. Create your .env file

  Based on the project, you can set up the `.env` file and make the necessary changes in the `project_name/settings.py` file also.
```shell
# Sample env file
  DB_NAME=db_name
  DB_USER=db_username
  DB_PASSWORD=password
  DB_HOST=localhost
  DB_PORT=5432
  SECRET_KEY=yoursecretkey

```
---------------------------------------
#### 6. To activate virtual environment:
```shell
    poetry shell
```
---------------------------------------
#### 7. To Run Migrations:
```shell
  python manage.py makemigrations
  python manage.py migrate

```
---------------------------------------
#### 8. To run the server
```shell
  python manage.py runserver
```
---------------------------------------
#### Add Additional Dependencies:

```shell
  poetry add <package>
```
---------------------------------------
#### To created super user:
```shell
  python manage.py createsuperuser
``` 

