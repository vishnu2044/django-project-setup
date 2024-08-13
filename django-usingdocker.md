# Django Project Setup

This file provides guidance on setting up a Django project using Docker, postgresql and poetry.

## Steps to Work with the Repository

#### 1. Setup Poetry for dependancy Management
Poetry is a powerful dependency management tool that simplifies Python project setup. Initialize it by running:
```shell
poetry init
```
---------------------------------------
#### 2. Add Django and PostgreSQL dependenciesn
Django is the core framework, and psycopg2-binary is the PostgreSQL adapter. Add them using:
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
#### 4. Create `Dockerfile`
n your project root, create a Dockerfile with the following content:
```shell
# Use an official Python runtime as a parent image
FROM python:3.12-slim

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
#### 5. Create a `docker-compose.yml` file:
In the project root, create a docker-compose.yml file with the following content:
```shell
version: '3.8'

name: docker-setup-name

services:
  web:
    image: docker-setup-image
    container_name: docker-setup-container-name
    build: .
    command: poetry run python manage.py runserver 0.0.0.0:8000
    volumes:
      - .:/app
    ports:
      - "8000:8000"
    depends_on:
      - db
    environment:
      - DATABASE_URL=postgres://<db_user>:<db_password>@db:5432/<db_name>

  db:
    image: postgres:13
    environment:
      POSTGRES_DB: <db_name>
      POSTGRES_USER: <db_user>
      POSTGRES_PASSWORD: <db_password>
    volumes:
      - postgres_data:/var/lib/postgresql/data/

volumes:
  postgres_data:
```
---------------------------------------
#### 6. Configure Django `settings.py` file for postgresql:
Open myproject/settings.py and update the DATABASES setting to use the PostgreSQL database:
```shell
import os
from urllib.parse import urlparse

# Parse the DATABASE_URL environment variable
result = urlparse(os.getenv("DATABASE_URL"))

DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.postgresql',
        'NAME': result.path[1:],  # Extracts the DB name
        'USER': result.username,   # Extracts the DB username
        'PASSWORD': result.password, # Extracts the DB password
        'HOST': result.hostname,   # Extracts the DB host
        'PORT': result.port,       # Extracts the DB port
    }
}


```
---------------------------------------
#### 7. Run the Docker Containers:
Build and start the containers
```shell
docker-compose up --build

```
---------------------------------------
#### 8. Run migrations:
Build and start the containers
```shell
docker-compose run web poetry run python manage.py makemigrations
docker-compose run web poetry run python manage.py migrate

```
---------------------------------------
#### 9. Create superuser:
Build and start the containers
```shell
docker-compose run web poetry run python manage.py createsuperuser

```
---------------------------------------
#### 9. Create superuser(Optional):
Build and start the containers
```shell
docker-compose run web poetry run python manage.py createsuperuser
```
---------------------------------------
#### 10. Runserver:
Build and start the containers
```shell
docker-compose run web poetry run python manage.py runserver
```
---------------------------------------
####  To access the database:
If you want to access the database from inside the PostgreSQL container itself
```shell
docker-compose exec db psql -U <db_user> -d <db_name>

```
---------------------------------------
#### 2. Add Essential Files
- Add a gitignore file
- add README file

---------------------------------------

