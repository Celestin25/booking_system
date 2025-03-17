# booking_system

# Creating a Django Project with Docker


# Step 1: Project Directory Setup

First, create a project directory and navigate into it:

mkdir django-docker-project

cd django-docker-project

# Step 2: Create Required Files

Create a Dockerfile




 # Create docker-compose.yml
 
Create a Docker Compose configuration file:

  
# Step 3: Create Django Project

Build the initial Docker image and create a new Django project:

docker-compose run web django-admin startproject core .

# Step 4: Configure Database

# Step 5: Run the Project

Start the Docker containers:

docker-compose up --build

Your Django application will be available at http://localhost:8000

 # Step 6: Create an App
 
In a separate terminal:

docker-compose exec web python manage.py startapp myapp

# Step 7: Run Migrations

docker-compose exec web python manage.py migrate
