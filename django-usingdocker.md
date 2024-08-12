# Django Project Setup

This file provides guidance on setting up a Django project using Poetry.

## Steps to Work with the Repository

#### 1. Setup Poetry for dependancy Management
Initialize poetry by running:
```shell
poetry init
```
---------------------------------------
#### 2. Add Django and PostgreSQL dependenciesn
Add dependencies that you want:
```shell
poetry add django psycopg2-binary
```
---------------------------------------
#### 3. Create a Django Application
Initialize your Django project by running:
```shell
poetry run django-admin startproject <project_name> .
```
---------------------------------------
#### 3. Create `Dockerfile`
n your project root, create a Dockerfile with the following content:
```shell
# Use an official Python runtime as a parent image
FROM python:3.10-slim

# Set the working directory in the container
WORKDIR /app

# Copy the project files to the working directory
COPY . /app/

# Install Poetry
RUN pip install poetry

# Install the dependencies from Poetry
RUN poetry config virtualenvs.create false
RUN poetry install --no-root

# Expose the port Django runs on
EXPOSE 8000

# Run the Django development server
CMD ["poetry", "run", "python", "manage.py", "runserver", "0.0.0.0:8000"]

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


