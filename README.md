# docker-dev-notes
Practical Docker notes for learning and reference.


---

# 🐳 Docker Basics: Notes & Concepts

> A beginner-friendly summary of Docker concepts, differences from VMs, and why Docker is a game-changer for modern development.

---

## 📦 Docker Lifecycle

```text
Dockerfile → (build) → Docker Image → (run) → Docker Container
```

* 📝 **Dockerfile**: A text file containing instructions to build a Docker image.
* 🛠️ **Docker Image**: A snapshot/template of your application environment.
* 🚀 **Docker Container**: A running instance of a Docker image.

---

## ❓ Why Docker?

Docker offers several advantages over traditional Virtual Machines (VMs):

### 🖥️ 1. OS Support

* 🧱 **Virtual Machines**: Each VM includes a full OS → uses **a lot of space**.
* 🐳 **Docker**: Containers share the host OS → uses **less space**.

---

### ⏱️ 2. Boot-Up Time

* 🧱 **VM**: Takes **minutes** to boot due to OS initialization.
* 🐳 **Docker**: Boots up in **seconds** as it shares the host OS.

---

### ⚡ 3. Performance

* 🧱 **VM**: Running multiple VMs on a single machine can **slow down performance**.
* 🐳 **Docker**: Lightweight and faster since containers run on a **single Docker engine**.

---

### 📈 4. Scalability

* 🧱 **VM**: **Hard to scale**; each VM needs its own OS.
* 🐳 **Docker**: **Easier to scale**; containers can share the OS and start/stop quickly.

---

### 🚚 5. Portability

* 🧱 **VM**: Tightly coupled to infrastructure; **difficult to migrate**.
* 🐳 **Docker**: Runs the same container on any system with Docker → **highly portable**.

---

### 💾 6. Space Allocation

* 🧱 **VM**: **Volumes (disks) can't be shared** easily between VMs.
* 🐳 **Docker**: **Volumes can be shared** between containers for persistent storage.

---

## 🔍 VM vs Docker: Key Architectural Difference

### 🧱 In Virtual Machines:

```
Application → Guest OS → Hypervisor → Host OS → Infrastructure
```

* A **hypervisor** is required to run multiple OSes on a host machine.

### 🐳 In Docker:

```
Application → Container Engine → Host OS → Infrastructure
```

* A **container engine** (like Docker) allows multiple containers to share the host OS.

---

## 📚 Key Terminology

### 💡 What is a Hypervisor?

> A software layer that allows multiple virtual machines (with their own OS) to run on a single physical machine.

### 💡 What is a Container Engine?

> A lightweight software (like Docker) that runs multiple containers by sharing the host OS, making it fast and resource-efficient.

---

## 🧪 Benefits of Dockerized Apps

* 🚀 Faster setup
* 🔄 Easier versioning and rollbacks
* 🧩 Consistent environments across teams
* 🧼 Clean app isolation
* 📦 Simplified deployment

---

## 👩‍💻 Final Note

Docker lets you **run multiple apps in isolated environments** with less overhead, better performance, and high flexibility compared to traditional VMs.

---


---

# 🐳 Docker Commands Cheat Sheet

A quick reference to commonly used Docker CLI commands with examples and helpful tips.

---

## 📥 1. Pull an Image

```bash
docker pull <image_name>
```

🔹 Downloads an image from Docker Hub (or other registries).
🔸 **No account is needed to pull**, but you need an account to **push** images.

✅ **Example:**

```bash
docker pull nginx
```

---

## 🚀 2. Run a Container

```bash
docker run <image_name>
```

🔹 Runs a container based on the specified image.

✅ **Example:**

```bash
docker run hello-world
```

💡 Add `-d` to run in detached mode or `-p` to map ports:

```bash
docker run -d -p 8080:80 nginx
```

---

## 🛠️ 3. Build an Image from a Dockerfile

```bash
docker build -t <image_name> .
```

🔹 Builds a Docker image from a `Dockerfile` in the current directory.

✅ **Example:**

```bash
docker build -t my-node-app .
```

---

## 🖼️ 4. List All Images

```bash
docker images
```

🔹 Displays all Docker images available locally.

---

## 📋 5. List Running Containers

```bash
docker ps
```

🔹 Shows all currently running containers.

---

## 🗂️ 6. List All Containers (Including Stopped)

```bash
docker ps -a
```

🔹 Lists **all** containers, including stopped ones.

---

## 🗑️ 7. Remove a Container

```bash
docker rm <container_name_or_id>
```

🔹 Removes a **stopped** container.

✅ **Example:**

```bash
docker rm my-nginx-container
```

🛑 You must stop it first:

```bash
docker stop my-nginx-container
```

---

## 🧹 8. Remove an Image

```bash
docker rmi <image_name_or_id>
```

🔹 Deletes a Docker image.

✅ **Example:**

```bash
docker rmi nginx
```

🛑 Make sure no containers are using this image.

---

## 🧠 Bonus Tips

* 📦 To remove all stopped containers:

  ```bash
  docker container prune
  ```

* 🖼️ To remove all unused images:

  ```bash
  docker image prune
  ```

* 🔄 Restart a container:

  ```bash
  docker restart <container_name_or_id>
  ```

---


---

## 🧱 Docker Images & Containers – Layered Architecture Explained

Every Docker image is built in **layers**, and understanding how these layers work is key to understanding Docker itself.

---

### 📦 Image Layers

* 🔹 **Base Layer:**
  The foundational layer is usually a Linux distribution (like Alpine, Ubuntu, etc.).

* 🔹 **Intermediate Layers:**
  Additional layers are stacked on top of the base (e.g., installed packages, environment setup).
  🧊 These layers are **read-only** and cannot be modified.

* 🔹 **Topmost Layer – Container Layer:**
  When you run an image, Docker adds a **writable layer** on top.
  ✍️ This is where changes like file creation, app logs, or package installation are made.

---

### 🧠 Reusability & Efficiency

* ✅ If the image you're pulling shares any layers with images already on your system, **Docker won’t download those again**.
* ⚡ This layer caching makes image building and pulling very efficient.

---

### 🚀 From Image to Container

1. **Pull the Image**
   Get a pre-built image from Docker Hub or another registry.

2. **Create a Container**
   Docker uses the image as a **template** and adds a writable layer on top, turning it into a container.

3. **Run the Container**
   The container becomes a live, isolated environment that runs your application.

---

### 🧰 What Does a Container Include?

A Docker container includes everything your application needs to run:

* 📁 **File System**
  Provided by the underlying image + the container’s writable layer.

* ⚙️ **Environment Configuration**
  Variables, ports, volumes, etc., are set up per container.

---

### 🖼️ Summary

| Concept            | Description                                          |
| ------------------ | ---------------------------------------------------- |
| **Image**          | A **template** with OS, app, and dependencies.       |
| **Layers**         | Image is built in **read-only layers**.              |
| **Container**      | A **runtime instance** of an image + writable layer. |
| **Writable Layer** | Only the topmost container layer is writable.        |

---

🔄 Docker makes deployment consistent, fast, and scalable through this layered architecture.



---

## 🔧 Docker Image and Container Basics

### ✅ Docker Image Structure

* Every **Docker image** is composed of **layers**.
* The **bottom layer** is typically a base OS (like Alpine, Ubuntu, etc.).
* All intermediate layers are **read-only**.
* The **topmost layer**, called the **container layer**, is **writable**.
* When we **run an image**, Docker creates a container by adding this writable layer on top.

> ✅ Docker images are **immutable**, but containers made from them are **mutable**.

### 🛠 Image Caching

* If a pulled image layer already exists on your system, Docker **won’t re-download** it.
* This improves **efficiency** and speeds up image pulls.

---

## 📦 Example: Running Redis with Docker

### 🔽 Step 1: Pull Redis Image

```bash
docker pull redis
```

Output:

```plaintext
Using default tag: latest
...
Status: Downloaded newer image for redis:latest
```

### 📋 View Pulled Images

```bash
docker images
```

Example:

```
REPOSITORY   TAG       IMAGE ID       CREATED        SIZE
redis        latest    6671ddaed9b1   14 hours ago   150MB
```

---

## 🚀 Step 2: Run Redis Container

### ▶ Run in Attached Mode

```bash
docker run redis
```

* Runs in foreground (attached). Use `Ctrl+C` to stop.

### ▶ Run in Detached Mode

```bash
docker run -d redis
```

Check running containers:

```bash
docker ps
```

Stop container:

```bash
docker stop <CONTAINER_ID>
```

Start stopped container:

```bash
docker start <CONTAINER_ID>
```

---

## 🎯 Pull a Specific Redis Version

```bash
docker pull redis:5.0
```

List images again:

```bash
docker images
```

Output:

```
REPOSITORY   TAG       IMAGE ID       CREATED        SIZE
redis        latest    6671ddaed9b1   14 hours ago   150MB
redis        5.0       91a41e726017   2 years ago    104MB
```

---

## 🧪 Run Multiple Redis Containers

```bash
docker run -d redis
docker run -d redis:5.0
```

Use `docker ps` to confirm both are running.

> ❗ Containers use the same internal port (6379), but Docker allows this because they run in **isolated environments**.

---

## 🔌 Port Binding Explained

> Containers are isolated and **don’t expose ports to the host** by default.

### 📤 Bind container port to host port:

```bash
docker run -d -p <host_port>:<container_port> <image>
```

### 🧪 Example:

```bash
docker run -d -p6379:6379 redis
docker run -d -p5001:6379 redis:5.0
```

Now Redis is accessible on:

* `localhost:6379` (latest version)
* `localhost:5001` (v5.0)

Check running containers:

```bash
docker ps
```

Output:

```
CONTAINER ID   IMAGE       PORTS
...            redis       0.0.0.0:6379->6379/tcp
...            redis:5.0   0.0.0.0:5001->6379/tcp
```

---

## 🛑 Stop the Containers

```bash
docker stop <container_id_1> <container_id_2>
```

Example:

```bash
docker stop 2683eeb34390 59fe95d7f96f
```

---




---

## 🔍 Debugging Docker Containers

We can debug Docker containers using two main methods:

### 1. **Analyzing Logs**

* View logs for a specific container:

  ```bash
  docker logs <container-id>
  docker logs <container-name>
  ```

* View logs in real-time (`-f` for "follow"):

  ```bash
  docker logs -f <container-id>
  docker logs -f <container-name>
  ```

---

### 2. **Accessing the Container Terminal**

Use the `docker exec` command to log in to a running container interactively:

```bash
docker exec -it <container-name or container-id> /bin/bash
```

> Note: Some images (like Alpine) may not have `/bin/bash`; use `/bin/sh` instead.

Example:

```bash
docker exec -it redis-latest-container /bin/bash
```

Once inside, you can run commands like `ls`, `env`, or check application-specific logs/configs.

To exit the container terminal:

```bash
exit
```

---

## 🧱 Assigning a Constant Name to a Container

When you run a container without a name, Docker assigns a random name (e.g., `optimistic_liskov`).

To assign a constant name:

```bash
docker run -d -p <host-port>:<container-port> --name <container-name> <image-name>
```

Example:

```bash
docker run -d -p 6379:6379 --name redis-latest-container redis
```

---

## 🛑 Stopping a Container

To stop a container:

```bash
docker stop <container-id or container-name>
```

Stopping a container keeps it in the history (`docker ps -a`) until it is removed.

---

## 🧹 Removing Stopped Containers

You cannot reuse a container name unless the old container is removed. Use `docker container prune` to delete **all stopped containers**:

```bash
docker container prune
```

> ⚠️ This deletes *all* stopped containers. Use with caution.

---

## 🧾 Useful Docker Commands Recap

| Command                               | Description                             |
| ------------------------------------- | --------------------------------------- |
| `docker ps`                           | List running containers                 |
| `docker ps -a`                        | List all containers (including stopped) |
| `docker logs <id/name>`               | View logs of a container                |
| `docker logs -f <id/name>`            | Follow live logs                        |
| `docker exec -it <id/name> /bin/bash` | Login to container terminal             |
| `docker stop <id/name>`               | Stop a running container                |
| `docker container prune`              | Delete all stopped containers           |

---


---

## ✅ Step-by-Step: Convert an Existing App to Docker Image

---

### 1️⃣ Create a `Dockerfile`

Inside your Node.js app root (where `package.json` is), create a file named `Dockerfile`:

```Dockerfile
# syntax=docker/dockerfile:1

FROM node:lts-alpine            # Use lightweight base image
WORKDIR /app                    # Set working directory
COPY . .                        # Copy app code into the image
RUN yarn install --production   # Install production dependencies
CMD ["node", "src/index.js"]    # Start the app
EXPOSE 3000                     # Declare the port app uses
```

> 🔍 Use `npm install --production` if you use npm instead of yarn.

---

### 2️⃣ Build the Docker Image

From the project directory (where Dockerfile is located):

```bash
docker build -t my-app-name .
```

* `-t my-app-name`: Tags the image with a name you choose.
* `.`: Refers to current directory (build context).

> 🧠 This installs dependencies and builds an image snapshot of your app.

---

### 3️⃣ Run the Container

```bash
docker run -d -p 3000:3000 my-app-name
```

* `-d`: Detached mode (runs in background)
* `-p 3000:3000`: Maps host port 3000 to container port 3000

✅ Visit your app at: [http://localhost:3000](http://localhost:3000)

---

### 4️⃣ (Optional) View Running Containers

```bash
docker ps
```

---

## 🔁 Common Enhancements

---

### 🧩 Need to run multiple scripts?

**Option 1: Use a shell script**

```sh
#!/bin/sh
node file1.js
node file2.js
```

Update Dockerfile:

```Dockerfile
COPY start.sh /app/start.sh
RUN chmod +x /app/start.sh
CMD ["/app/start.sh"]
```

**Option 2: Use `sh -c`**

```Dockerfile
CMD ["sh", "-c", "node file1.js && node file2.js"]
```

---

### 🛠 Need dev dependencies?

Remove `--production`:

```Dockerfile
RUN yarn install
```

---

### 📦 Want to use `npm` instead of `yarn`?

Replace:

```Dockerfile
RUN yarn install --production
```

with

```Dockerfile
RUN npm install --production
```

---

## 🧼 Clean-up Tips

* To stop a container:

  ```bash
  docker stop <container-id>
  ```

* To remove unused images:

  ```bash
  docker image prune -a
  ```

---

## 🔗 Resources

* [Dockerfile Reference](https://docs.docker.com/reference/dockerfile/)
* [Docker Build Docs](https://docs.docker.com/engine/reference/commandline/build/)

---



---

# 🚀 Running a PHP-MySQL CRUD App with Multiple Docker Containers

## 📦 Overview

In a **full-stack PHP-MySQL CRUD app**, we need:

* One container for the **backend** (PHP + Apache)
* Another for the **database** (MySQL)

To run them together:

* The PHP app (CRUD interface) listens on **Port 80** inside the container, mapped to **Port 8081** on the host
* MySQL listens on **Port 3306**

## 🐳 Step-by-Step: Using Docker CLI

### 1. Create a Docker Network

```bash
docker network create app-network
```

### 2. Run the MySQL Container

```bash
docker run -d -p 3306:3306 \
  --name=db \
  --network=app-network \
  -e MYSQL_ROOT_PASSWORD='Pass@123' \
  techtvm/mysql_techtvm \
  mysqld --default-authentication-plugin=mysql_native_password
```

🔍 **Explanation:**

* `--name=db`: Names the container for easier reference
* `--network=app-network`: Allows containers to talk to each other
* `-e`: Sets environment variables (like MySQL root password)
* `mysqld ...`: Starts the MySQL server with legacy password plugin support

### 3. Run the PHP CRUD App Container

```bash
docker run -d -p 8081:80 \
  --name=crud-app \
  --network=app-network \
  -e MYSQL_DBHOST='db:3306' \
  -e MYSQL_DBUSER='root' \
  -e MYSQL_DBPASS='Pass@123' \
  -e MYSQL_DBNAME=php_crud \
  techtvm/php-crud-app
```

🧠 **Note:**

* `MYSQL_DBHOST='db:3306'`: The hostname `db` matches the container name of MySQL
* All containers need to be in the same network (`app-network`)

## ⚠️ Problem with This Approach

Managing multiple containers separately can get tedious. A better solution: **Docker Compose**.

---

## 🧩 Docker Compose: Simplify Multi-Container Management

With Docker Compose, you define all containers in one file: `docker-compose.yaml`.

### 📄 Sample `docker-compose.yaml`

```yaml
version: '3.8'

services:
  db:
    image: techtvm/mysql_techtvm
    container_name: db
    environment:
      MYSQL_ROOT_PASSWORD: 'Pass@123'
    command: mysqld --default-authentication-plugin=mysql_native_password
    ports:
      - "3306:3306"
    networks:
      - app-network

  crud-app:
    image: techtvm/php-crud-app
    container_name: crud-app
    environment:
      MYSQL_DBHOST: 'db:3306'
      MYSQL_DBUSER: 'root'
      MYSQL_DBPASS: 'Pass@123'
      MYSQL_DBNAME: php_crud
    ports:
      - "8081:80"
    networks:
      - app-network
    depends_on:
      - db

networks:
  app-network:
    driver: bridge
```

---

## ▶️ Running the App

### Start the containers:

```bash
docker-compose up -d
```

### If your `docker-compose.yaml` is in a different path:

```bash
docker-compose -f /path/to/docker-compose.yaml up -d
```

### Stop and remove all containers, networks, and volumes:

```bash
docker-compose down
```

---

## 📝 Key Concepts Recap

| Concept                | Description                                              |
| ---------------------- | -------------------------------------------------------- |
| `--network`            | Allows containers to talk to each other by name          |
| `-e`                   | Sets environment variables for containers                |
| `depends_on`           | Ensures service order (e.g., backend waits for database) |
| `command`              | Overrides default startup command                        |
| `ports`                | Maps internal container ports to host machine ports      |
| `docker-compose up -d` | Starts all services in the background                    |
| `docker-compose down`  | Stops and removes containers, networks                   |

---



---

# 🐳 Docker for Multi-Container Fullstack App

## Scenario: PHP - MySQL CRUD App

### ✅ App Overview

* **Backend (MySQL)** runs on **port 3306**
* **Frontend (PHP + Apache)** runs on **port 80**
* The frontend and backend run in **separate containers**
* Both containers must be **connected via the same Docker network**

---

## 🔹 Step-by-Step with `docker run`

### 1️⃣ Run MySQL Container

```bash
docker run -d -p 3306:3306 \
  --name=db \
  --network=app-network \
  -e MYSQL_ROOT_PASSWORD='Pass@123' \
  techtvm/mysql_techtvm \
  mysqld --default-authentication-plugin=mysql_native_password
```

**Explanation:**

* `--name=db`: Assigns a name to the container
* `--network=app-network`: Joins the container to a custom Docker network
* `-e MYSQL_ROOT_PASSWORD`: Sets the MySQL root password
* `techtvm/mysql_techtvm`: Custom image used
* `mysqld --default-authentication-plugin=mysql_native_password`: Ensures compatibility with older MySQL clients

---

### 2️⃣ Run PHP CRUD App Container

```bash
docker run -d -p 8081:80 \
  --name=crud-app \
  --network=app-network \
  -e MYSQL_DBHOST='db:3306' \
  -e MYSQL_DBUSER='root' \
  -e MYSQL_DBPASS='Pass@123' \
  -e MYSQL_DBNAME=php_crud \
  techtvm/php-crud-app
```

---

## ❗ Limitations

You need to **manually manage** containers: start, stop, network configuration, etc.

---

## ✅ Solution: Docker Compose

### 🔧 `docker-compose.yaml`

```yaml
version: '3.8'

services:
  db:
    image: techtvm/mysql_techtvm
    container_name: db
    environment:
      MYSQL_ROOT_PASSWORD: 'Pass@123'
    command: mysqld --default-authentication-plugin=mysql_native_password
    ports:
      - "3306:3306"
    volumes:
      - mysql_data:/var/lib/mysql/data
    networks:
      - app-network

  crud-app:
    image: techtvm/php-crud-app
    container_name: crud-app
    environment:
      MYSQL_DBHOST: 'db:3306'
      MYSQL_DBUSER: 'root'
      MYSQL_DBPASS: 'Pass@123'
      MYSQL_DBNAME: php_crud
    ports:
      - "8081:80"
    networks:
      - app-network
    depends_on:
      - db

networks:
  app-network:
    driver: bridge

volumes:
  mysql_data:
```

### ▶️ To Run:

```bash
docker-compose up -d
```

### 🛑 To Stop:

```bash
docker-compose down
```

* Use `-v` to also remove volumes:

  ```bash
  docker-compose down -v
  ```

---

# 💾 Docker Volumes

### ❓ Why Use Volumes?

* Data **inside containers is lost** when the container is removed (`docker rm`)
* Volumes ensure **persistent data storage**

### 🔍 Data Loss Scenarios

| Command                  | Data Lost?                   |
| ------------------------ | ---------------------------- |
| `docker stop`            | ❌ No                         |
| `docker rm`              | ✅ Yes (unless volume used)   |
| `docker-compose down`    | ✅ Yes (if no volume defined) |
| `docker-compose down -v` | ✅ Yes (removes volume)       |

---

## 📦 Types of Docker Volumes

### 1. **Host Volume**

```bash
docker run -v /home/sanju/data:/var/lib/mysql/data ...
```

* Mounts a specific host path to container path

---

### 2. **Anonymous Volume**

```bash
docker run -v /var/lib/mysql/data ...
```

* Host path is automatically generated by Docker

---

### 3. **Named Volume**

```bash
docker run -v mysql_data:/var/lib/mysql/data ...
```

* Volume can be referred to by name

---

## 🔍 Inspect Docker Volumes

List all volumes:

```bash
docker volume ls
```

Inspect a specific volume:

```bash
docker volume inspect mysql_data
```

---


---

## 🐳 Docker Volumes

### 🔸 What happens when containers are stopped/removed?

| Action                   | Is Data Lost?                              |
| ------------------------ | ------------------------------------------ |
| `docker stop`            | ❌ No – Container stops, data is preserved. |
| `docker rm`              | ✅ Yes – Removes container and data.        |
| `docker-compose down`    | ✅ Yes – If no named volumes are used.      |
| `docker-compose down -v` | ✅ Yes – Volumes are removed too.           |

> 💡 `docker stop` only halts the container — the writable layer and data remain.
> 💥 Data is lost **only if** the container is removed and no volume is used.

---

### 📂 How Data Works in Docker Containers

* Docker images are **read-only**, composed of multiple layers.
* Running a container adds a **read/write container layer** on top.
* This layer contains a **virtual file system**.
* All operations (writes/changes) happen in this top layer.
* **Data is ephemeral** if stored only inside this layer (i.e., not persisted).

---

### 📦 Why Use Volumes?

* To persist data **outside** of the container lifecycle.
* Volumes bridge the container’s virtual file system with the host’s physical file system.

---

### 📁 Volume Types

#### 1. **Host Volume**

Mounts a specific path from your host to the container.

```bash
docker run -v /home/sanju/data:/var/lib/mysql/data mysql
```

#### 2. **Anonymous Volume**

You don’t specify the host path; Docker manages it internally.

```bash
docker run -v /var/lib/mysql/data mysql
```

> Docker stores it in: `/var/lib/docker/volumes/<random_id>/_data`

#### 3. **Named Volume**

You assign a name to the volume, and Docker manages the path.

```bash
docker run -v mysql_data:/var/lib/mysql/data mysql
```

> You can reuse `mysql_data` across containers and reattach it if needed.

---

### 📘 Example: `docker-compose.yml` with Named Volume

```yaml
version: '3.8'

services:
  db:
    image: techtvm/mysql_techtvm
    container_name: db
    environment:
      MYSQL_ROOT_PASSWORD: 'Pass@123'
    command: mysqld --default-authentication-plugin=mysql_native_password
    ports:
      - "3306:3306"
    volumes:
      - mysql_data:/var/lib/mysql/data
    networks:
      - app-network

volumes:
  mysql_data:

networks:
  app-network:
    driver: bridge
```

### 🔍 Useful Volume Commands

```bash
docker volume ls          # List all volumes
docker volume inspect <name>  # Get full path & details
```

---

## 🚀 Pushing Docker Images to a Private Repository

### 🔐 What is a Private Repository?

* **Public images** on Docker Hub can be pulled without authentication.
* **Private images** require authentication.
* Platforms that support private registries:

  * Docker Hub
  * AWS ECR
  * Azure, Google Cloud
  * GitHub Container Registry

---

### 🧱 Steps to Push a Custom Image to Docker Hub

#### 1. Create a Docker Hub account

> Free accounts can create 1 private repo

#### 2. Create a private repository

Example: `sanju9645/test_private_repo`

---

#### 3. Build the Docker Image

```bash
cd getting-started-app
docker build -t my-first-custom-image .
```

#### 4. Authenticate with Docker Hub

```bash
docker login
```

> To remove credentials:
> `rm ~/.docker/config.json`

---

#### 5. Tag the Image for Push

```bash
docker tag my-first-custom-image:latest sanju9645/test_private_repo:1.0.0
```

#### 6. Push the Image

```bash
docker push sanju9645/test_private_repo:1.0.0
```

---

### 🛑 Delete an Image Locally

```bash
docker rmi my-first-custom-image:latest
docker rmi sanju9645/test_private_repo:1.0.0
```

---

### 📥 Pulling Images from a Private Repo

> Must be logged in (`docker login`)
> Ensure correct tag is used when pulling:

```bash
docker pull sanju9645/test_private_repo:1.0.0
```

---

### 📚 Useful Commands

```bash
docker images         # List all images
docker volume ls      # List volumes
docker network ls     # List networks
docker network prune  # Remove unused networks
```

---

