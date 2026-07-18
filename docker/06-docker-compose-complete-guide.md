# Chapter 10: Docker Compose Complete Guide

**Docker Compose** is a tool for defining and running multi-container Docker applications. With Compose, you use a YAML file to configure your application’s services, networks, and volumes, then start them all with a single command.

---

## 1. Core Concepts of Docker Compose

Instead of running separate terminal sessions to spin up multiple containers, Docker Compose aggregates container commands into a single structured configuration.

* **Services**: Defines the containers to run.
* **Networks**: Defines custom isolated networks for container communication.
* **Volumes**: Defines persistent storage mounts shared between host and containers.

---

## 2. Docker Compose YAML Schema Reference

Here is a comprehensive overview of the core YAML parameters used in a `docker-compose.yml` file:

```yaml
version: '3.8' # Represents the Docker Compose file format version

services:
  web:
    image: nginx:alpine # Specify image to pull
    container_name: web-server # Optional custom container name
    ports:
      - "80:80" # Map host port 80 to container port 80
    networks:
      - app-network
    depends_on:
      - api # Ensure the 'api' container starts before 'web'

  api:
    build:
      context: ./api # Directory containing a Dockerfile
      dockerfile: Dockerfile
    environment:
      - NODE_ENV=production
      - DB_HOST=db
    env_file:
      - .env # Load environment variables from a file
    networks:
      - app-network
    depends_on:
      - db

  db:
    image: postgres:15-alpine
    volumes:
      - db_data:/var/lib/postgresql/data # Persist database storage
    environment:
      POSTGRES_DB: appdb
      POSTGRES_USER: admin
      POSTGRES_PASSWORD_FILE: /run/secrets/db_password # Using Docker secrets
    secrets:
      - db_password
    networks:
      - app-network

secrets:
  db_password:
    file: ./secrets/db_password.txt # Define secret files

volumes:
  db_data: # Define the persistent volume

networks:
  app-network:
    driver: bridge # Define the isolated network bridge
```

---

## 3. Controlling Service Startup Order

In multi-tier configurations, certain containers must start before others (e.g., APIs must wait for databases).
* `depends_on`: Instructs Docker Compose to start dependency services first.
* **Limitation**: `depends_on` only waits until the dependency container starts, *not* until the application inside is fully ready.
* **Solution**: Use `healthcheck` configurations in the dependency service, and specify condition matching in the dependent service:
  ```yaml
  api:
    depends_on:
      db:
        condition: service_healthy
  ```

---

## 4. Docker Compose Commands Reference

Here are the essential commands used to manage multi-container applications:

* **Start Stack**: `docker compose up -d`
  * Compiles, creates, connects, and starts all services in detached mode.
* **Stop Stack**: `docker compose down`
  * Stops containers and removes containers, networks, and volumes created by `up`.
  * Use `docker compose down -v` to delete persistent volumes too.
* **List Running Services**: `docker compose ps`
* **Stream Stack Logs**: `docker compose logs -f`
* **Restart Services**: `docker compose restart`
* **Validate YAML Configuration**: `docker compose config`
* **Rebuild Custom Images**: `docker compose build`
* **Run One-Off Command**: `docker compose run <service> <command>`
  * Useful for running migrations: `docker compose run api npm run db:migrate`.
