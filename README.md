ETNAir - Environment Setup
This setup creates a Docker environment for a Node.js API and PostgreSQL database, making it easier to develop, test, and deploy the ETNAir platform locally or in a containerized environment.

Prerequisites
Before starting, ensure that you have the following installed on your machine:

Docker: Download Docker
Docker Compose: Docker Compose is typically installed with Docker Desktop. Learn more
Cloning the Repository
Start by cloning the repository to your local machine:

bash
Copier le code
git clone <repository_url>
cd etnair
Setting Up the Docker Environment
Follow these steps to set up the environment with Docker and Docker Compose:

1. Docker Compose Configuration
Make sure your project contains a docker-compose.yml file for setting up the containers for the Node.js API and PostgreSQL database. Below is a simple configuration you can use:

yaml
Copier le code
version: '3.8'

services:
  backend:
    image: node:16
    container_name: etnair-backend
    working_dir: /app
    volumes:
      - ./backend:/app
    ports:
      - "5000:5000"
    environment:
      - DATABASE_URL=postgresql://<user>:<password>@db:5432/etnair
      - JWT_SECRET=<your_jwt_secret>
    depends_on:
      - db
    command: npm start

  db:
    image: postgres:13
    container_name: etnair-db
    environment:
      - POSTGRES_USER=<user>
      - POSTGRES_PASSWORD=<password>
      - POSTGRES_DB=etnair
    ports:
      - "5432:5432"
    volumes:
      - etnair-db-data:/var/lib/postgresql/data

volumes:
  etnair-db-data:
2. Environment Configuration
Create an .env file in your project’s backend directory with the following variables:

env
Copier le code
DATABASE_URL=postgresql://<user>:<password>@db:5432/etnair
JWT_SECRET=<your_jwt_secret>
Make sure to replace <user>, <password>, and <your_jwt_secret> with appropriate values.

3. Building and Running the Containers
Run the following command to build and start the Docker containers:

bash
Copier le code
docker-compose up --build
This will:

Build the backend container using the Node.js image.
Set up a PostgreSQL container to store your database.
Expose the API on port 5000 and the database on port 5432.
4. Running the Backend Server
Once the containers are running, navigate to the backend directory and run the following to ensure that your database is correctly set up:

bash
Copier le code
npx prisma migrate dev
This will apply the necessary database migrations and set up the schema defined in your Prisma models.

5. Accessing the Application
Frontend: Access the frontend at http://localhost:3000 (if running separately in a development environment).
Backend: Access the API at http://localhost:5000.
Database: You can connect to PostgreSQL using localhost:5432 with the credentials you defined in .env.
6. Stopping the Containers
To stop the Docker containers, run:

bash
Copier le code
docker-compose down
This will stop and remove all the containers.

