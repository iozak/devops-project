## Objective
Create a multi-container application that consists of a simple Python Flask web application and a Redis database. The Flask application should use Redis to store and retrieve data.
## Requirements
1. **Flask Web Application**:
    - A Flask app that has two routes:
        - `/`: Displays a welcome message.
        - `/count`: Increments and displays a visit count stored in Redis.
2. **Redis Database**:
    - Use Redis as a key-value store to keep track of the visit count.
3. **Dockerize Both Services**:
    - Create Dockerfiles for both the Flask app and Redis.
    - Use Docker Compose to manage the multi-container application.
## Bonus
1. Persistent Storage for Redis: Configure Redis to use a volume to persist its data.
2. Environment Variables: Modify the Flask application to read Redis connection details from environment variables and update the docker-compose.yml accordingly.
3. Scaling the Application: Scale the Flask service to run multiple instances and load balance between them using Docker Compose.

## Start
```
mkdir docker-challenge && cd docker-challenge
```
```
touch app.py Dockerfile docker-compose.yml
```

**app.py (The Flask application)**
Two routes added: '/' & '/count' - increment a counter stored in Redis.
Connection details are read from environment variables.
```
from flask import Flask
import redis

app = Flask(__name__)

cache = redis.Redis(host='redis', port=6379)

@app.route('/')
def welcome():
    return 'Welcome to my project built using Docker, Flask & Redis'

@app.route('/count')
def count():
    count = cache.incr('visits')
    return f'This page has been visited {count} times.'

if __name__ == '__main__':
    app.run(host='0.0.0.0', port=5000)
```

**Dockerfile**
Single image build on python:3.14.6-slim. Copies the app in, installs the flask & redis libraries,
Exposes the port and sets the start command.
```
FROM python:3.14.6-slim

WORKDIR /app

COPY . .

RUN pip install flask redis

EXPOSE 5000

CMD ["python", "app.py"]
```

**docker-compose.yml**
Runs on one private network 'web (flask)'.
```
services:
  web:
    build: .
    ports:
      - "5000:5000"
    depends_on:
      - redis

  redis:
    image: redis:latest
    ports:
      - "6379:6379"
```

Then build the container
```
docker build -t hello-redis .
```

Run the container
```
docker run -d -p 5000:5000 hello-redis
```

**Result**
Verified successful application and Redis connectivity. The root (/) page displays "Welcome to my project built using Docker, Flask & Redis", while the /count page displays "This page has been visited x times.". The visit counter increments correctly with each refresh, confirming Redis persistence and functionality.

## Bonus 1 - Persistent Storage
Persistent Storage for Redis: Configure Redis to use a volume to persist its data.

**Start**
By default, the visit counter is stored only within the Redis container, meaning it resets whenever the container is removed and recreated. To ensure the data persists beyond the container's lifecycle, a named volume was mounted to Redis's data directory (/data). This allows Redis to store its data on persistent storage rather than within the container itself.

The volume is defined once in the docker-compose.yml file and then mounted to the redisdb service. As a result, Redis can reload its saved data when the container starts, ensuring that the visit count is retained across container shutdowns, restarts, and redeployments.

docker-compose.yml
```
services:
  web:
    build: .
    ports:
      - "5000:5000"
    depends_on:
      - redis

  redis:
    image: redis:latest
    ports:
      - "6379:6379"
    volumes:
      - redis-data:/data

volumes:
  redis-data:
```

```
docker compose up
```

**Result**
The solution was successfully verified by bringing the environment down and back up using Docker Compose. The counter resumed from its previous value, confirming that the Redis data persisted correctly within the configured volume.

## Bonus 2 - Environment Variables
Modify the Flask application to read Redis connection details from environment variables and update the docker-compose.yml accordingly.

**Start**
Update the app.py (Flask Application). Replaced the Redis connection with environment variables.
```
from flask import Flask
import redis
import os

app = Flask(__name__)

cache = redis.Redis(
    host=os.getenv('REDIS_HOST', 'redis'),
    port=int(os.getenv('REDIS_PORT', 6379))
)

@app.route('/')
def welcome():
    return 'Welcome to my project built using Docker, Flask & Redis'

@app.route('/count')
def count():
    count = cache.incr('visits')
    return f'This page has been visited {count} times.'

if __name__ == '__main__':
    app.run(host='0.0.0.0', port=5000)
```
This configuration now uses:
- REDIS_HOST for the Redis hostname.
- REDIS_PORT for the Redis port.
- Default values of redis and 6379 if the variables are not defined.

Updated docker-compose.yml
Added the environment variables to the Flask service:
```
services:
  web:
    build: .
    ports:
      - "5000:5000"
    environment:
      REDIS_HOST: redis
      REDIS_PORT: 6379
    depends_on:
      - redis

  redis:
    image: redis:latest
    volumes:
      - redis-data:/data

volumes:
  redis-data:
```

Then rebuild the image and start as the flask application code changed.
```
docker compose up --build
```

**Result**
The Redis connection configuration was decoupled from the application code by replacing hardcoded host and port values with environment variables. The Flask application now retrieves these settings using `os.getenv()`, with sensible defaults provided as a fallback. Corresponding variables were added to the `web` service in `docker-compose.yml`, enabling configuration changes without modifying the source code and aligning with containerisation best practices.