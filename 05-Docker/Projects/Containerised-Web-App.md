**Creating a Web App**
Create simple Python Flask web application that will later be packaged into a Docker container.
```
1. Install Python.
2. Install Flask.
3. Use simple web application.
4. Run it locally.
```
Create app.py and add flask code
```
from flask import Flask

app = Flask(__name__)

@app.route('/')
def hello_world():
    return 'Hello, world!'

if  __name__ == '__main__':
    app.run(host='0.0.0.0', port=5000)
```
How the Flask Application Works
```
Imports the Flask framework.
Creates a Flask application instance.
Defines a route (/).
Returns "Hello World" when someone visits the homepage.
Starts the web server.
```
Run the app with
```
python3 app.py
```

**Containerise our Web Application**
Now the process of containerising the Flask web application by creating a Dockerfile and building a Docker image.
Note - The file name must be Dockerfile (capital D). It should have no file extension.
```
# Create a fie in the same directory as our web app named
Dockerfile
```

Populated the Dockerfile with the instructions
```
FROM python:3.14.6-slim
# 3.14.6-slim provides a lightweight Python environment.

WORKDIR /app
# Creates and sets /app as the working directory. Future commands run from this location.

COPY . .
# Copies all files from the current project directory into the container.

RUN pip install flask
# Installs Flask inside the container.

EXPOSE 5000
# Indicates the application will use port 5000.

CMD ["python", "app.py"]
# Runs the Flask application when the container starts.
```

Build the Docker Image
```
docker build -t hello-flask .
```
docker build → Starts the image build process.
-t hello-flask → Tags the image with the name hello-flask.
. → Tells Docker to look for the Dockerfile in the current directory.

Running the Container
```
docker run -d -p 5000:5000 hello-flask
```
docker run → Creates and starts a container.
-d → Runs the container in detached mode (background).
-p 5000:5000 → Maps port 5000 on the host machine to port 5000 inside the container.
hello-flask → The Docker image being used.

When executed, Docker returns a container ID, indicating the container is running.

Checking Running Containers
```
docker ps
```

Stopping a Container
```
docker stop <container-id>
```

**Linking Containers together**
Now to take it up a level and move from a simple standalone Flask application to a more realistic setup where the Flask app connects to a MySQL database and retrieves information from it.
```
Flask Application Container
↓
MySQL Container
```

We will update the flask app to now also import the MySQL client library, establish a connection to the MySQL database, execute a SQL query, and display MySQL version alongside the hello world message.
```
from flask import Flask
import MySQLdb

app = Flask(__name__)

@app.route('/')
def hello_world():
    # Connect to the MySQL database
    db = MySQLdb.connect(
        host="mydb",    # Hostname of the MySQL container
        user="root",    # Username to connect to MySQL
        passwd="my-secret-pw",  # Password for the MySQL user
        db="mysql"      # Name of the database to connect to
    )
    cur = db.cursor()
    cur.execute("SELECT VERSION()")
    version = cur.fetchone()
    return f'Hello, World! MySQL version: {version[0]}'

if __name__ == '__main__':
    app.run(host='0.0.0.0', port=5000)

```

Updating the Dockerfile
```
RUN pip install flask mysqlclient
```
Now includes mysqlclient alongside flask

While using the python-slim image, the build failed due to missing system dependencies required by mysqlclient. The Dockerfile was updated to install the additional packages needed to complete the build successfully.
```
FROM python:3.14.6-slim

WORKDIR /app

COPY . .

RUN apt-get update && apt-get install -y \
    gcc \
    python3-dev \
    libmariadb-dev \
    pkg-config

RUN pip install flask mysqlclient

EXPOSE 5000

CMD ["python", "app.py"]
```

Creating a Custom Docker Network
```
docker network create my-custom-network
```
A key concept is that Docker containers can communicate using container names rather than IP addresses.  This will allow to discover each other by name, Communicate without needing hardcoded IPs, and operate in an isolate environment.

Running the MySQL Container
```
docker run -d \
--name mydb \
--network my-custom-network \
-e MYSQL_ROOT_PASSWORD=my-secret-pw \
mysql:8
```

Rebuilding the Flask Image
```
docker build -t hello-flask-mysql .
```

Running the Flask Container
```
docker run -d \
--name myapp \
--network my-custom-network \
-p 5000:5000 \
hello-flask-mysql
```