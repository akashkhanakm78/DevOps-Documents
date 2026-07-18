# Docker Commands Quick Reference Card

A single-page cheat sheet for the most common Docker commands.

---

## 1. Container Basics
```bash
docker run -d -p 80:80 --name web nginx   # Create and start a container in detached mode
docker ps                                 # List running containers
docker ps -a                              # List all containers (running & stopped)
docker stop web                           # Gracefully stop a running container
docker start web                          # Start an existing, stopped container
docker restart web                        # Restart a container
docker kill web                           # Force-stop (SIGKILL) a container
docker rm web                             # Delete a stopped container
docker rm -f web                          # Force delete a running container
```

## 2. Diagnostics & Debugging
```bash
docker logs web                           # Print logs of a container
docker logs -f --tail 50 web              # Stream container logs, showing last 50 lines
docker exec -it web sh                    # Open an interactive shell inside a container
docker inspect web                        # Output raw configurations in JSON format
docker stats                              # Show stream of CPU, Memory, & Network usage
docker top web                            # Display running processes of a container
docker port web                           # List public mapped ports
docker diff web                           # Show modified files in container filesystem
```

## 3. Image Operations
```bash
docker images                             # List all local images
docker pull ubuntu:22.04                  # Download an image from registry
docker build -t app:v1.0 .                # Build an image from Dockerfile in current dir
docker rmi app:v1.0                       # Delete a local image
docker tag app:v1.0 repo/app:v1.0         # Tag an image for registry
docker push repo/app:v1.0                 # Upload image to registry
docker history app:v1.0                   # View build layers of an image
```

## 4. Volume & Network Commands
```bash
# Volumes
docker volume ls                          # List all persistent volumes
docker volume create db_data              # Create a named volume
docker volume inspect db_data             # Inspect storage path on host
docker volume rm db_data                  # Delete a volume

# Networks
docker network ls                         # List all networks
docker network create --driver bridge net # Create custom bridge network
docker network connect net web            # Connect container to a network
docker network disconnect net web         # Disconnect container from a network
```

## 5. System Cleanup
```bash
docker system df                          # Show Docker disk usage
docker container prune                    # Delete all stopped containers
docker image prune                        # Delete all dangling/untagged images
docker system prune -a --volumes          # WIPE OUT all unused containers, images, and volumes
```
