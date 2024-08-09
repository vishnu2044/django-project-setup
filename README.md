
# django-project-setup

This is the file for guidance on creating a Django setup.

# Steps to work with the repo

- Create Django Application
```shell
  django-admin startproject <Project name>

```
- Setup poetry

Install poetry package manager (https://python-poetry.org/docs/#installation)
```shell
poetry init

```
Ensure that the database is created and the user has the required permissions.

- To install dependencies:
```shell
    poetry install
```

- To activate virtual environment:
```shell
    poetry shell
```

- To Run Migrations:

```shell
  python manage.py makemigrations
  python manage.py migrate

```

- To run the server
```shell
  python manage.py runserver
```

- If you want to add any dependencies run:
```shell
  poetry add <package>
```

- To created super user:
```shell
  python manage.py createsuperuser
``` 


 - Before starting the project, run these commands to import necessary data:
```shell
  python manage.py import_qualifications
  python manage.py import_state_and_countries
```
