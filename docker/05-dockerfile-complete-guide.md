# Chapter 09: The Complete Guide to Dockerfile

A **Dockerfile** is a text document that contains all the commands a user could call on the command line to assemble a Docker Image.

---

## 1. Complete Dockerfile Instruction Reference

Here is a comprehensive breakdown of every major instruction you can use in a Dockerfile.

| Instruction | Description | Syntax | Best Practice |
| :--- | :--- | :--- | :--- |
| `FROM` | Sets the Base Image for subsequent instructions. | `FROM image:tag` | Always use specific tags (e.g., `alpine` or versioned labels), never `latest`. |
| `RUN` | Executes commands in a new layer and commits the results. | `RUN apt-get update && apt-get install -y curl` | Combine multiple commands into one block with `&&` and `\` to minimize layers. |
| `CMD` | Provides default arguments or commands for an executing container. | `CMD ["npm", "start"]` | Use the exec form (JSON array) rather than shell form. Can be overridden easily. |
| `ENTRYPOINT` | Configures a container that will run as an executable. | `ENTRYPOINT ["nginx", "-g", "daemon off;"]` | Use to define the main immutable executable of the container. |
| `WORKDIR` | Sets the working directory for subsequent instructions. | `WORKDIR /app` | Always use absolute paths. Avoid using `cd` inside `RUN` statements. |
| `COPY` | Copies files/directories from the build context to the container. | `COPY src/ /app/src/` | Use `COPY` instead of `ADD` for basic local file transfers. |
| `ADD` | Copies files, extracts tar archives, and downloads remote URLs. | `ADD archive.tar.gz /app/` | Use only when you specifically need to auto-extract archives or download files. |
| `ENV` | Sets environment variables. | `ENV PORT=8080` | Can be accessed at runtime and build time by subsequent steps. |
| `ARG` | Defines variables that users can pass at build-time. | `ARG VERSION=1.0` | These variables do not persist in the final image at runtime. |
| `EXPOSE` | Informs Docker that the container listens on specified network ports. | `EXPOSE 80` | Documentational only. Does not publish ports automatically. |
| `VOLUME` | Creates a mount point and marks it as holding externally mounted volumes.| `VOLUME ["/data"]` | Bypasses the UnionFS, sending data directly to the host disk. |
| `USER` | Sets the user name or UID to use when running the image. | `USER node` | Always run applications as non-root users in production. |
| `HEALTHCHECK`| Tells Docker how to test a container to check that it is still working. | `HEALTHCHECK CMD curl -f http://localhost/ || exit 1`| Highly recommended for production services. |
| `ONBUILD` | Adds a trigger instruction to be executed when the image is used as a base.| `ONBUILD COPY . /app` | Useful for builder images in shared library patterns. |

---

## 2. Shell vs Exec Form

Many instructions (`RUN`, `CMD`, `ENTRYPOINT`) can be written in two formats:

### Shell Form
```dockerfile
CMD npm start
```
* **Under the Hood**: Runs as `/bin/sh -c "npm start"`.
* **Disadvantage**: The command runs as a sub-process of the shell. It will not receive OS signals (like `SIGTERM` on `docker stop`), meaning your app cannot shut down gracefully.

### Exec Form (Recommended)
```dockerfile
CMD ["npm", "start"]
```
* **Under the Hood**: Runs the process directly without a shell wrapper.
* **Advantage**: The executable runs as PID 1, receiving system signals directly and exiting gracefully.

---

## 3. Optimizing Dockerfiles: Layer Caching & Minimizing Size

To build fast and lightweight Docker images:

1. **Leverage Build Caching**: Put instructions that change frequently (like copying source code) at the **bottom** of the Dockerfile, and stable ones (like installing dependencies) at the **top**.
2. **Minimize Layers**: Combine related shell commands inside `RUN` statements.
3. **Use `.dockerignore`**: Create a `.dockerignore` file in your project root to exclude files like `.git`, `node_modules`, and build logs from being sent to the Docker build context.

---

## 4. Multi-Stage Builds

Multi-stage builds allow you to use multiple temporary images in your build process, copying only the final build outputs (artifacts) to the slim production image. This drastically reduces image size.

```
[ Build Stage ]                 [ Production Stage ]
(Node / Compiler / SDK)         (Minimal Runtime, e.g. Alpine)
      |                               |
      |---> Builds app binary         |
      |---> Compiles assets           |
      |                               |<--- Copies ONLY the compiled binary/assets
                                      |
                                      V
                              Resulting Image: Ultra Slim (~20MB)
```

---

## 5. Production Dockerfile Examples

### Node.js (Multi-Stage)
```dockerfile
# Stage 1: Build
FROM node:18-alpine AS builder
WORKDIR /app
COPY package*.json ./
RUN npm ci
COPY . .
RUN npm run build

# Stage 2: Production
FROM node:18-alpine
WORKDIR /app
ENV NODE_ENV=production
COPY package*.json ./
RUN npm ci --only=production
COPY --from=builder /app/dist ./dist
USER node
EXPOSE 3000
CMD ["node", "dist/index.js"]
```

### React (Nginx Web Server)
```dockerfile
# Stage 1: Build React Assets
FROM node:18-alpine AS build
WORKDIR /app
COPY package*.json ./
RUN npm ci
COPY . .
RUN npm run build

# Stage 2: Serve with Nginx
FROM nginx:alpine
COPY --from=build /app/build /usr/share/nginx/html
EXPOSE 80
CMD ["nginx", "-g", "daemon off;"]
```

### Python (Slim API)
```dockerfile
FROM python:3.11-slim
WORKDIR /app
RUN groupadd -g 999 appuser && useradd -r -u 999 -g appuser appuser
COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt
COPY . .
USER appuser
EXPOSE 8000
CMD ["uvicorn", "main:app", "--host", "0.0.0.0", "--port", "8000"]
```
