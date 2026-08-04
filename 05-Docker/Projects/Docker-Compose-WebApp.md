By using Docker Compose, we can define the Flask application, MySQL database, and their networking requirements in a single `docker-compose.yml` file. This removes the need to manually create containers and networks, enabling the complete application stack to be started, stopped, and managed with a single command.

Create docker-compose.yml file
```
services:
  web:
    build: .
    ports:
      - "5000:5000"
    depends_on:
      - mydb

  mydb:
    image: mysql:8
    environment:
      MYSQL_ROOT_PASSWORD: my-secret-pw
```

Run the docker compose stack with
```
docker compose up
```

Or in detached mode (run in background)
```
docker compose up -d
```

Stop the stack with
```
docker compose down
```
