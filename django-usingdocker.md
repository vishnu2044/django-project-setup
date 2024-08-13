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
version: '3.8'  # Specifies the version of the Docker Compose file format being used.

name: abc-setup  # Optional name for the Docker Compose application.

services:  # Defines the services that will be run in separate containers.

  db:  # The database service configuration.
    image: postgres:13  # Specifies the Docker image for PostgreSQL version 13.
    environment:  # Defines environment variables for the PostgreSQL container.
      POSTGRES_DB: <db_name>  
      POSTGRES_USER: <db_user>   
      POSTGRES_PASSWORD: <db_password>
    volumes:  # Mounts a volume to persist the database data.
      - postgres_data:/var/lib/postgresql/data/  # Maps the `postgres_data` volume to the PostgreSQL data directory.

  web:  # The web application service configuration.
    image: abc-image  
    container_name: abc-container  
    build: .  # Specifies that the Docker image should be built using the Dockerfile in the current directory.
    command: poetry run python manage.py runserver 0.0.0.0:8000 
    volumes:  # Mounts the current directory to the `/app` directory inside the container.
      - .:/app  # Ensures code changes are reflected inside the container without rebuilding.
    ports: 
      - "8000:8000"  # Allows access to the web application via http://localhost:8000.
    depends_on:  
      - db 
    environment:  # Defines environment variables for the web application container.
      - POSTGRES_NAME=<db_name>  
      - POSTGRES_USER=<db_user>  
      - POSTGRES_PASSWORD=<db_password>

volumes:  # Defines Docker volumes for persistent data storage.
  postgres_data:  # A named volume to persist PostgreSQL data across container restarts.
```
---------------------------------------
#### 6. Configure Django `settings.py` file for postgresql:
Open myproject/settings.py and update the DATABASES setting to use the PostgreSQL database:
```shell
import os

DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.postgresql',
        'NAME': os.environ.get('POSTGRES_NAME'),
        'USER': os.environ.get('POSTGRES_USER'),
        'PASSWORD': os.environ.get('POSTGRES_PASSWORD'),
        'HOST': 'db',   # Extracts the DB host
        'PORT': 5432,       # Extracts the DB port
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

```
```shell
docker-compose run web poetry run python manage.py migrate

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

