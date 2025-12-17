[![Review Assignment Due Date](https://classroom.github.com/assets/deadline-readme-button-22041afd0340ce965d47ae6ef1cefeee28c7c493a6346c4f15d667ab976d596c.svg)](https://classroom.github.com/a/GHX226XH)
[![Open in Codespaces](https://classroom.github.com/assets/launch-codespace-2972f46106e565e64193e422d61a12cf1da4916b45550586e14ef0a7c637dd04.svg)](https://classroom.github.com/open-in-codespaces?assignment_repo_id=21795136)
# Basic Concepts - Docker

Docker is a platform that allows the creation, running, and management of applications inside containers - small "packages" that include everything an application needs: code, libraries, file system, etc.

A **container** is an isolated environment that contains everything required for an application to run: source code, libraries, dependencies, and configurations.

You can think of a container as a mini virtual machine - but much lighter and faster.

Multiple containers can run different applications on the same machine without conflicts.

Docker is useful because the application is packaged with everything it needs; it can run anywhere, on any system that has Docker installed, and the environment remains identical for all developers and production servers.

Without Docker, an application might behave differently across systems (Windows, Linux, macOS) due to different library versions or configurations.

**The Three Main Components of Docker:**

- **Dockerfile** - a text file that describes how to build an image (which packages to install, which files to copy, etc.).
- **Docker Image** - a "template" created from a Dockerfile; it's a static package.
- **Docker Container** - a running instance of an image (similar to an isolated process).

**Advantages of Docker:**

- Portability
- Complete isolation
- Fast and repeatable configuration
- Scalability (easy to use with Kubernetes)

**Difference Between Docker and Virtual Machines:**

| **Feature** | **Docker** | **Virtual Machine** |
| --- | --- | --- |
| Isolation | Application-level | Operating System-level |
| Performance | Very fast | Slower (runs a full OS) |
| Size | Hundreds of MB | Tens of GB |
| Startup time | Seconds | Minutes |

**Docker Installation**

**Windows:**

- Go to the official website: <https://www.docker.com/products/docker-desktop>
- Click on **Download for Windows (WSL2)** (WSL stands for Windows Subsystem for Linux - a required component)
- Run the installer and follow the steps (**next-next-finish**).

After installation, open the **Docker Desktop** application. Then open **Command Prompt** or **PowerShell** and verify the installation with:

docker -version

**I. Create Your First Project with Docker**

We'll create a simple Python application as an example.

- Create a new folder:

mkdir docker-test

cd docker-test

- Create a file named app.py (Python):

print("Salut din Docker!")

- Create a file named Dockerfile (without any extension!):

Here is a minimal Dockerfile for running a Python app:

FROM python:3.10-slim

\# Copy your app code into the container

COPY app.py .

\# Run the app when the container starts

CMD \["python", "app.py"\]

**Explanation:**

- FROM python:3.10-slim → Uses the official lightweight Python 3.10 base image.
- COPY app.py . → Copies your app.py file from your host machine into the container's working directory (by default /).
- CMD \["python", "app.py"\] → Instructs Docker to run your Python script when the container starts.

- Build the Docker image

In the terminal, from the folder containing your files, run:

docker build -t test-python .

Regarding the command docker build -t test-python .:

- \-t test-python → gives a name (tag) to the image.
- **.** → means that the Dockerfile is located in the current folder.

5\. Run the container

After the image has been created, the following command runs the container:

docker run test-python

**What Happens**

Basically, Docker will build the image from the **Dockerfile**. Then it will start a container that runs **app.py**. You should see the following output in the terminal (as shown in the previous image):

"Salut din Docker!"

**Optional Improvements**

If your app has dependencies (such as **Flask**, **requests**, etc.), you can add a requirements.txt file and modify the Dockerfile as follows:

FROM python:3.10-slim

WORKDIR /app

COPY requirements.txt .

RUN pip install --no-cache-dir -r requirements.txt

COPY app.py .

CMD \["python", "app.py"\]

6\. Listing Containers and Images

Active containers:

docker ps

All containers (including stopped ones):

docker ps -a

Other Useful Docker Commands

| **Command** | **Explanation** |
| --- | --- |
| docker images | Lists all downloaded images |
| docker stop &lt;id&gt; | Stops a running container |
| docker rm &lt;id&gt; | Removes a container |
| docker rmi &lt;image&gt; | Removes an image |
| docker logs &lt;id&gt; | Displays the logs of a container |
| docker exec -it &lt;id&gt; bash | Opens an interactive shell inside a running container |

II. Next, a Second Example - Running Your First Container with Docker Compose

On Windows, **Docker Compose** is already included in the **Docker Desktop** application, so you don't need to install it separately.

Basically, we'll use a **docker-compose.yml** file to start a Python application (**app.py**) inside a container.

The goal is for **docker-compose** to:

- build a container from a **Dockerfile**
- run the **app.py** file
- expose the application on a port (if you want to test it locally)

The folder should contain the following three files:

- Your **app.py** file (with some simple code)
- A **Dockerfile**
- A **docker-compose.yml** file

A **.yml** file is a text file used to write structured data in a human-readable format, typically for configuration purposes.

It is commonly used in:

- Docker Compose
- GitHub Actions
- CI/CD pipelines (e.g., GitLab pipelines)
- Web application configuration
- Ansible, Kubernetes, etc.

A simple example of a .yml file:

nume: Ana

varsta: 16

materii:

\- Matematica

\- Informatica

\- Biologie

This defines:

- a name (**Ana**)
- an age (**16**)
- a list of subjects

A .yml file is used because it is:

- easy to read (compared to JSON or XML)
- clearly hierarchical (uses indentation to define structure)
- easy to write by hand

YAML files use the following extensions:

- **.yml** - the short form, commonly used
- **.yaml** - the full form, officially accepted

Both are identical in functionality.

And now, back to the **app.py** file:

print("Salut! Aplicația rulează din Docker Compose.")

In the same folder, create a file named **Dockerfile** with the following content:

FROM python:3.10-slim

COPY app.py .

CMD \["python", "app.py"\]

In the same folder, create another file named **docker-compose.yml** with the following content:

version: "3"

services:

app:

build: .

container_name: python_app_2

command: ???

The command field allows you to override the command executed inside the container (otherwise, the one specified in the Dockerfile is used - that is, CMD \["python", "app.py"\]).

CMD in Dockerfile:

This is the **default command** that Docker runs when the container starts.

**command in docker-compose.yml:**

- If used, it **overrides** the CMD instruction from the Dockerfile.
- If not used, Docker will run the command specified in the Dockerfile.

In our case, we **don't need** to include command: in the **docker-compose.yml** file, because we already have:

CMD \["python", "app.py"\]

Therefore, the final file looks like this:

Final docker-compose.yml:

version: "3"

services:

app:

build: .

container_name: python_app

Then, run the container with the command:

docker compose up -build

- \--build tells Docker to rebuild the image (if any changes were made).
- Docker will build the image from the Dockerfile.
- It will start a container that runs **app.py**.
- You should see the following in the terminal:

Salut! Aplicația rulează din Docker Compose.

**III. Flask Example (Web Application)**

If we want the container to stay **running all the time** (like a web application), we can try using a server (for example, Flask).

We'll consider the case where the container remains running, acting as a web application that constantly listens for requests.

We will create a simple web application using **Flask** (a Python micro-framework).

1\. The app.py file:

from flask import Flask

app = Flask(\__name_\_)

@app.route('/')

def home():

return "Salut! Aplicația Flask rulează în Docker Compose."

if \__name__ == '\__main_\_':

app.run(host='0.0.0.0', port=5000)

Explanation:

- It creates a minimal Flask web server.
- The / route returns a greeting message.
- The host='0.0.0.0' makes Flask accessible from outside the container.
- The app listens on port **5000**.

2\. Dockerfile installs Flask:

FROM python:3.10-slim

WORKDIR /app

COPY app.py .

RUN pip install flask

CMD \["python", "app.py"\]

3\. Modify docker-compose.yml:

version: "3"

services:

app:

build: .

container_name: python_app

ports:

\- "5000:5000"

Explanation:

- version: "3" → Uses Docker Compose v3 syntax (standard in most cases).
- build: . → Builds an image using the Dockerfile in the current directory.
- container_name: python_app → Names the container python_app.
- ports: - "5000:5000" → Maps port 5000 on your host machine to port 5000 inside the container.

4\. In the terminal, in the folder containing the files, run:

docker compose up -build

- Docker will build the image (installing Flask)
- It will start a Flask server inside the container
- In your browser, open:

<http://localhost:5000>

You should see the message:

Salut! Aplicația Flask rulează în Docker Compose.

Here's a quick overview of what's happening in your Docker environment based on the next image:

- Docker Engine: Running (bottom left says _Engine running_).
- Containers tab: Open and displaying a list of containers.
- You have 7 containers listed, mostly using the image test-python and one hello-world.
- Only one container - docker-2 - is running, indicated by the green dot and "Running (1/1)".
- Container resource usage is minimal:
  - CPU: 0.02%
  - Memory: ~20 MB of 7.18 GB

The **"Containers" view** in Docker Desktop shows all containers that are running or have run on that system. In short, it displays:

- **List of containers** - every container created locally, whether active, stopped, or in error.
- **Container status** - running, stopped, paused, etc.
- **Base image** - the image from which the container was created.
- **Quick controls** - buttons for Start, Stop, Restart, Delete, Open in Terminal, or View Logs.
- **Detailed information** (when you click on a container) - environment variables, exposed ports, mounted volumes, logs, and resource statistics (CPU, memory).

**IV. Example: Flask + MySQL Application in Docker Compose**

(This combines a web server and a database in separate containers, managed with Docker Compose.)  
This example demonstrates:

- defining multiple services in Docker Compose
- how Flask communicates with a MySQL container via environment variables
- how to build complete web applications in separate but connected containers

Project Structure

flask-mysql-app/

│

├── app.py

├── requirements.txt

├── Dockerfile

└── docker-compose.yml

- app.py (a Flask application that connects to a MySQL database and displays simple data)

from flask import Flask, jsonify

import mysql.connector

import os

app = Flask(\__name_\_)

\# Connect to the MySQL database (service name defined in docker-compose.yml)

db_config = {

'host': os.environ.get('DB_HOST', 'db'),

'user': os.environ.get('DB_USER', 'root'),

'password': os.environ.get('DB_PASSWORD', 'root'),

'database': os.environ.get('DB_NAME', 'students_db')

}

@app.route('/')

def home():

return "Salut! Aplicația Flask este conectată la MySQL."

@app.route('/students')

def get_students():

conn = mysql.connector.connect(\*\*db_config)

cursor = conn.cursor(dictionary=True)

cursor.execute("SELECT \* FROM students;")

data = cursor.fetchall()

cursor.close()

conn.close()

return jsonify(data)

if \__name__ == '\__main_\_':

app.run(host='0.0.0.0', port=5000)

- requirements.txt

flask

mysql-connector-python

- Dockerfile

FROM python:3.10-slim

WORKDIR /app

COPY requirements.txt .

RUN pip install -no-cache-dir -r requirements.txt

COPY app.py .

CMD \["python", "app.py"\]

- docker-compose.yml

version: "3"

services:

web:

build: .

container_name: flask_app

ports:

\- "5000:5000"

environment:

\- DB_HOST=db

\- DB_USER=root

\- DB_PASSWORD=root

\- DB_NAME=students_db

depends_on:

\- db

db:

image: mysql:8.0

container_name: mysql_db

restart: always

environment:

MYSQL_ROOT_PASSWORD: root

MYSQL_DATABASE: students_db

ports:

\- "3306:3306"

5\. Initialize the database

After starting the containers:

docker compose up -build

Then enter the MySQL container:

docker exec -it mysql_db mysql -u root -p

\# password: root

And run:

USE students_db;

CREATE TABLE students (

id INT AUTO_INCREMENT PRIMARY KEY,

name VARCHAR(50),

age INT

);

INSERT INTO students (name, age) VALUES ('Ana', 20), ('Mihai', 22);

6\. Testing

Open the following URLs in your browser:

- <http://localhost:5000> → welcome message
- <http://localhost:5000/students> → list of students from MySQL
