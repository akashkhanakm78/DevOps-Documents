# Chapter 08: The Ultimate Docker Commands Guide (100+ Commands)

This reference contains over 100 essential Docker commands, organized by their lifecycle, object type, troubleshooting utility, and usage.

---

## 1. Container Lifecycle Commands

| Command | Syntax | Flags & Options | Example |
| :--- | :--- | :--- | :--- |
| **Run Container** | `docker run [options] image [cmd]` | `-d` (Detached), `-p` (Ports), `--name` (Name), `-v` (Volume) | `docker run -d -p 80:80 --name web nginx` |
| **Create Container** | `docker create [options] image` | Same as run | `docker create -name app node:18` |
| **Start Container** | `docker start [options] container` | `-i` (Interactive), `-a` (Attach stdout) | `docker start -i app` |
| **Stop Container** | `docker stop [options] container` | `-t` (Grace period in seconds) | `docker stop -t 5 app` |
| **Restart Container** | `docker restart [options] container` | `-t` (Grace period) | `docker restart app` |
| **Kill Container** | `docker kill [options] container` | `-s` (Signal default: SIGKILL) | `docker kill -s SIGINT app` |
| **Pause Container** | `docker pause container` | None | `docker pause web` |
| **Unpause Container** | `docker unpause container` | None | `docker unpause web` |
| **Remove Container** | `docker rm [options] container` | `-f` (Force remove running), `-v` (Remove volumes) | `docker rm -fv web` |
| **Prune Containers** | `docker container prune` | `-f` (Force bypass confirmation) | `docker container prune -f` |

---

## 2. Image Management Commands

| Command | Syntax | Description / Common Flags | Example |
| :--- | :--- | :--- | :--- |
| **Build Image** | `docker build [options] path` | `-t` (Tag name), `-f` (Dockerfile path) | `docker build -t app:v1 .` |
| **Pull Image** | `docker pull image:tag` | Fetch image from registry | `docker pull redis:alpine` |
| **Push Image** | `docker push image:tag` | Upload image to registry | `docker push username/app:v1` |
| **List Images** | `docker images` or `docker image ls` | List local images | `docker images -a` |
| **Remove Image** | `docker rmi image` or `docker image rm` | Remove local image | `docker rmi node:18-alpine` |
| **Tag Image** | `docker tag source_image target_image` | Create an alias tag | `docker tag app:v1 username/app:v1` |
| **Inspect Image** | `docker image inspect image` | Returns detailed JSON configuration | `docker image inspect nginx` |
| **Save Image** | `docker save -o tarfile.tar image` | Save image to a tar archive | `docker save -o app.tar app:v1` |
| **Load Image** | `docker load -i tarfile.tar` | Load image from a tar archive | `docker load -i app.tar` |
| **Prune Images** | `docker image prune` | Remove unused/dangling images | `docker image prune -a` |
| **Image History** | `docker history image` | Show layers and build history | `docker history nginx` |

---

## 3. Network Management Commands

| Command | Syntax | Description | Example |
| :--- | :--- | :--- | :--- |
| **Create Network** | `docker network create [options] name` | Create a virtual network (default bridge) | `docker network create --driver bridge my-net` |
| **List Networks** | `docker network ls` | List all local Docker networks | `docker network ls` |
| **Inspect Network**| `docker network inspect name` | Get JSON details of network configurations | `docker network inspect my-net` |
| **Connect Network**| `docker network connect net cont` | Connect a container to a network | `docker network connect my-net web` |
| **Disconnect** | `docker network disconnect net cont` | Disconnect container from network | `docker network disconnect my-net web` |
| **Remove Network** | `docker network rm name` | Delete custom network | `docker network rm my-net` |
| **Prune Networks** | `docker network prune` | Remove all unused networks | `docker network prune -f` |

---

## 4. Volume Management Commands

| Command | Syntax | Description | Example |
| :--- | :--- | :--- | :--- |
| **Create Volume** | `docker volume create name` | Create persistent named storage volume | `docker volume create db_data` |
| **List Volumes** | `docker volume ls` | List all volumes on the host | `docker volume ls` |
| **Inspect Volume** | `docker volume inspect name` | Inspect physical mount location | `docker volume inspect db_data` |
| **Remove Volume** | `docker volume rm name` | Delete a volume (fails if in use) | `docker volume rm db_data` |
| **Prune Volumes** | `docker volume prune` | Remove all unused local volumes | `docker volume prune -f` |

---

## 5. Monitoring & Troubleshooting Commands

| Command | Syntax | Common Flags | Example |
| :--- | :--- | :--- | :--- |
| **List Containers**| `docker ps` | `-a` (All), `-q` (Only IDs), `-s` (Sizes) | `docker ps -a` |
| **Container Logs** | `docker logs container` | `-f` (Follow/stream), `--tail n` (Last n lines) | `docker logs -f --tail 100 web` |
| **Inspect Object** | `docker inspect object` | Inspect metadata of container/image | `docker inspect web` |
| **Execute Process**| `docker exec -it container command`| Run a process inside a running container | `docker exec -it web bash` |
| **Container Stats**| `docker stats [container]` | Real-time CPU, memory, network usage | `docker stats web` |
| **Processes (top)**| `docker top container` | List running processes in a container | `docker top web` |
| **Diff Changes** | `docker diff container` | Inspect changes on container file system | `docker diff web` |
| **Copy Files** | `docker cp source target` | Copy files between host and container | `docker cp ./nginx.conf web:/etc/nginx/` |
| **Port Mapping** | `docker port container` | View public mapped ports of container | `docker port web` |
| **System DF** | `docker system df` | View disk usage by containers/images/volumes | `docker system df` |
| **System Events**  | `docker system events` | Stream system events in real-time | `docker system events` |
| **System Prune**  | `docker system prune` | Clear unused containers, networks, and images | `docker system prune -a --volumes -f` |

---

## Advanced Commands (Docker Registry & Swarm)

* `docker login` - Login to a Docker registry.
* `docker logout` - Logout from a Docker registry.
* `docker search <term>` - Search Docker Hub for images.
* `docker swarm init` - Initialize a Swarm cluster on the current host.
* `docker swarm join` - Join a Swarm cluster as a worker or manager node.
* `docker service create` - Deploy a service to a Swarm cluster.
* `docker stack deploy` - Deploy a multi-service stack to Swarm using a Compose file.
