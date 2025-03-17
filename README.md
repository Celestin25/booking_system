# booking_system

#Creating a Django Project with Docker
Here's how to set up a Django project using Docker:

# Step 1: Project Directory Setup

First, create a project directory and navigate into it:
bashCopymkdir django-docker-project
cd django-docker-project

# Step 2: Create Required Files

Create a Dockerfile
Create a file named Dockerfile:
bashCopytouch Dockerfile
Add the following content to the Dockerfile:
dockerfileCopyFROM python:3.11

WORKDIR /app

COPY requirements.txt .

RUN pip install --no-cache-dir -r requirements.txt

COPY . .

EXPOSE 8000

CMD ["python", "manage.py", "runserver", "0.0.0.0:8000"]
Create requirements.txt
Create a file for Python dependencies:
bashCopytouch requirements.txt
Add these dependencies:
CopyDjango>=4.2.0,<5.0.0
gunicorn>=21.0.0
Create docker-compose.yml
Create a Docker Compose configuration file:
bashCopytouch docker-compose.yml
Add this content:
yamlCopyversion: '3'

services:
  web:
    build: .
    command: python manage.py runserver 0.0.0.0:8000
    volumes:
      - .:/app
    ports:
      - "8000:8000"
    depends_on:
      - db
    environment:
      - DEBUG=1
      - DATABASE_URL=postgres://postgres:postgres@db:5432/postgres

  db:
    image: postgres:15
    volumes:
      - postgres_data:/var/lib/postgresql/data/
    environment:
      - POSTGRES_PASSWORD=postgres
      - POSTGRES_USER=postgres
      - POSTGRES_DB=postgres
    ports:
      - "5432:5432"

volumes:
  postgres_data:
  
# Step 3: Create Django Project

Build the initial Docker image and create a new Django project:
bashCopydocker-compose run web django-admin startproject core .

# Step 4: Configure Database

Update core/settings.py to use PostgreSQL:
pythonCopy# Replace the DATABASES section with:
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.postgresql',
        'NAME': 'postgres',
        'USER': 'postgres',
        'PASSWORD': 'postgres',
        'HOST': 'db',
        'PORT': 5432,
    }
}
Also add the postgres driver to requirements.txt:
Copypsycopg2-binary>=2.9.6

# Step 5: Run the Project

Start the Docker containers:
bashCopydocker-compose up --build
Your Django application will be available at http://localhost:8000

 # Step 6: Create an App
 
In a separate terminal:
bashCopydocker-compose exec web python manage.py startapp myapp

# Step 7: Run Migrations

bashCopydocker-compose exec web python manage.py migrate
