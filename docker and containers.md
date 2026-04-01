
# Docker and container

## What is a container?
A container is a lightweight package that contains the application code, required libraries, dependencies, and runtime. It helps the application run the same way in any environment.

________________________________________

## What is Docker?
Docker is a tool and platform used to create, run, and manage containers.

________________________________________

## Why are containers used?
Containers are used to make application deployment easy, consistent, fast, and portable across different environments like developer system, testing, and production.

________________________________________

## What problems do containers solve?
Containers solve problems like:
•	application working in one system but not in another  
•	dependency mismatch  
•	difficult deployment  
•	inconsistent environments  
•	slow scaling of applications  

________________________________________

## What is the difference between a container and a virtual machine?
A container shares the host operating system kernel and runs only the application and its dependencies.  
A virtual machine has its own full operating system on top of a hypervisor.  
So, containers are lightweight and fast, while VMs are heavier and use more resources.

________________________________________

## What is the difference between containerization and virtualization?
Containerization means packaging and isolating the application at the OS level.  
Virtualization means creating full virtual machines with separate operating systems.  
So, containerization is lighter and faster than virtualization.

________________________________________

## How do containers work internally?
Containers work using OS-level features:
•	namespaces for isolation  
•	cgroups for resource control  
•	layered file system for image storage  
This allows multiple containers to run on the same host safely and efficiently.

________________________________________

## What is the role of the Docker Engine?
Docker Engine is the main service that runs Docker.  
It is responsible for building images, creating containers, running containers, and managing networks and volumes.

________________________________________

## What is the difference between Docker and container runtime?
Docker is a complete platform for building and managing containers.  
A container runtime is the actual component that runs the container.  
So, Docker gives the full user-friendly platform, while runtime does the low-level execution.

________________________________________

## What is a Docker container lifecycle?
The Docker container lifecycle is the sequence of states a container goes through:
•	created  
•	running  
•	stopped  
•	restarted  
•	removed  

________________________________________

## What is a Docker image?
A Docker image is a read-only template that contains the application, dependencies, and instructions needed to run a container.

________________________________________

## What is the difference between Docker image and container?
A Docker image is the blueprint.  
A container is the running instance of that image.  
Image is static. Container is active and running.

________________________________________

## What is Docker Hub?
Docker Hub is an online registry where Docker images are stored and shared.

________________________________________

## What is a container registry?
A container registry is a place used to store, manage, and distribute container images.  
Examples: Docker Hub, Amazon ECR, Azure Container Registry.

________________________________________

## What is the difference between public and private registry?
A public registry allows anyone to access and pull images.  
A private registry restricts access and is used for secure internal company images.

________________________________________

## What is OCI in containers?
OCI stands for Open Container Initiative.  
It is a standard that defines how container images and runtimes should work so that containers can be portable across different tools.

## Docker Architecture

Docker mainly has 3 major parts:
1.	Docker Client  
2.	Docker Host / Docker Engine  
3.	Docker Registry  

### 1. Docker Client
The Docker client is the command-line interface where we run docker commands  

### 2. Docker Host / Docker Engine
Docker Engine is the core part of Docker.  
It runs on the machine where Docker is installed.  

Docker Engine includes:
•	Docker Daemon  
•	REST API  
•	Container Runtime  

#### a) Docker Daemon
The Docker Daemon is a background service.  
It is responsible for:
•	building images  
•	pulling images  
•	creating containers  
•	starting and stopping containers  
•	managing networks  
•	managing volumes  
So, the daemon does the actual work.

________________________________________

#### b) REST API
The REST API allows the Docker client to communicate with the Docker daemon.  
When you run:  
docker run nginx  
the client sends this request through the API to the daemon, and the daemon performs the task.

________________________________________

#### c) Container Runtime
The container runtime is the part that actually runs the containers.  
It creates the isolated environment for the container using OS features like:
•	namespaces  
•	cgroups  
•	layered filesystem  
Examples:
•	containerd  
•	runc  

### 3. Docker Registry
A Docker Registry is the place where Docker images are stored.  
Examples:
•	Docker Hub  
•	Amazon ECR  

## Main Docker Components

Now let us understand the important Docker objects.

### 1. Docker Image
A Docker image is a read-only template used to create containers.  
It contains:
•	application code  
•	runtime  
•	libraries  
•	dependencies  
•	environment setup  
•	startup instructions  

Example:  
An image for a Python app may contain:
•	Python installed  
•	application code copied  
•	required packages installed  
•	command to start the app  
So image is the package.

________________________________________

### 2. Docker Container
A container is the running instance of a Docker image.  
When we start an image, Docker creates a container from it.  
So:
•	image = blueprint  
•	container = running application  
A container is isolated from other containers, but it shares the host OS kernel.

________________________________________

### 3. Dockerfile
A Dockerfile is a text file with instructions to build a Docker image.  

Example:  
FROM python:3.11  
WORKDIR /app  
COPY . .  
RUN pip install -r requirements.txt  
CMD ["python", "app.py"]  

This tells Docker:
•	base image to use  
•	working directory  
•	what files to copy  
•	what dependencies to install  
•	what command to run  
So Dockerfile is used to create an image.

________________________________________

### 4. Docker Volumes
Volumes are used to store persistent data outside the container.  
This is needed because container data is temporary by default.  

Example:  
If a database container stores data inside the container and the container is deleted, the data is lost.  
To avoid this, we use volumes.

### 5. Docker Network
Docker network allows containers to communicate with:
•	each other  
•	the host machine  
•	external systems  

Example:  
A frontend container can communicate to a backend container by adding under same network.

## How containers run

Container = process running on the host OS  

docker run nginx  
     ↓  
Docker  
     ↓  
containerd  
     ↓  
runc  
     ↓  
Linux Kernel  
     ↓  
Process starts (container)  

### 1️⃣ docker run nginx (User command)
👉 What happens:
•	You ask Docker to run nginx application  
•	Docker understands:  
o	which image → nginx  
o	what to do → run container  
👉 Output:  
Request is sent to Docker Engine  

________________________________________

### 2️⃣ Docker (Docker Engine / Daemon)
👉 What Docker does:
•	checks if nginx image exists locally  
•	if not → pulls from Docker Hub  
•	prepares:  
o	network  
o	volume (if any)  
o	container config  
👉 Output:  
Hands over work to containerd  

________________________________________

### 3️⃣ containerd (Manager)
👉 What containerd does:
•	unpacks image  
•	sets up container filesystem  
•	prepares container metadata  
•	creates container structure  
👉 Output:  
Calls runc to actually start container  

________________________________________

### 4️⃣ runc (Executor)
👉 What runc does:  
Step 1: Calls kernel for namespace  
Step 2: Calls kernel for cgroups  
Step 3: Starts process ( container)  

👉 Namespaces = isolation mechanism   They make one container not see others  
👉 cgroups = “control how much resources(cpu/memory) container can use”  

Note : If you specify limits  
docker run -m 512m --cpus=1 nginx  

Example without namespace  
If you run:  
ps aux  
You see ALL processes on system  

________________________________________

With namespace (inside container)  
ps aux  
👉 You only see:
•	your app (nginx / python)  
•	few internal processes  
👉 You CANNOT see:
•	host processes  
•	other containers  

•	🧠 Think like this  
•	👉 Linux kernel = tools provider  
👉 runc = tool user  

## What is isolation and namespaces difference
👉 Isolation = concept (goal) wherein each container is independent of each other  
👉 Namespaces = one way to achieve isolation  

## What is dangling image?
A dangling image is an image with:
•	no tag  
•	not used by any container  

## What is image tagging?
Tagging means giving a name and version to an image.  

Example:  
docker build -t myapp:v1 .  

## What is the use of tags like latest?
latest is the default tag.  
Added If no tag is specified  

## What is the difference between image size and container size?
👉 Image size = total size of all image layers  
👉 Container size = image size + writable layer (runtime changes)  

## What is the difference between base image and parent image?
•	Base image → first image in Dockerfile (no dependency on previous layers in that build)  
•	Parent image → any image used to build another image  

Example:  
FROM python:3.11  
Here:
•	python image = parent image  
•	if it is first line → also base image  

## What are Docker layers?
Docker layers are read-only steps created for each instruction in a Dockerfile.  

Notes: These are the only Dockerfile instructions that create filesystem layers in a Docker image:
•	FROM  
•	RUN  
•	COPY  
•	ADD  

Example  
FROM ubuntu        # Layer 1  
RUN apt-get update # Layer 2  
RUN apt-get install python3  # Layer 3  
COPY app.py /app/  # Layer 4  

👉 Each line = one layer  
👉 Layers are stacked to form the final image  

Key points
•	Layers are read-only  
•	Layers are cached and reused  
•	Layers are shared between images  

________________________________________

## What is image caching in Docker?
Image caching means Docker reuses previously built layers instead of rebuilding them.

________________________________________

### Example
First build  
docker build -t myapp .  
Docker builds all layers:
•	Layer 1 → built  
•	Layer 2 → built  
•	Layer 3 → built  
•	Layer 4 → built  

________________________________________

Now change only app.py  
Second build  
docker build -t myapp .  

👉 Docker behavior:
•	Layer 1 → reused  
•	Layer 2 → reused  
•	Layer 3 → reused  
•	Layer 4 → rebuilt  

________________________________________

## Why caching is useful
•	faster builds  
•	saves time  
•	reduces resource usage  

## Important rule (very important)
👉 Docker checks layers top to bottom  
If one layer changes:
👉 all layers below it are rebuilt  

## What is a Dockerfile?
A Dockerfile is a text file with instructions used to build a Docker image.  
It defines:
•	base image  
•	dependencies  
•	application code  
•	commands to run the app  

## What are common Dockerfile instructions?

### FROM
Defines base image  
👉 starting point  
FROM python:3.11  

________________________________________

### WORKDIR
Sets working directory  
👉 where commands run  
WORKDIR /app  

________________________________________

### COPY
Copies files into image  
👉 add app code  
COPY . .  

________________________________________

### RUN
Executes commands during build  
👉 install dependencies  
RUN pip install -r requirements.txt  

________________________________________

### CMD
Default command when container starts  
👉 run application  
CMD ["python", "app.py"]  

CMD can be overridden  
docker run myimage echo hello  

________________________________________

### ENTRYPOINT
Main fixed command always runs  
👉 enforce execution  
ENTRYPOINT ["python"]  

docker run myimage app.py  
👉 Runs:  
python app.py  

ENTRYPOINT cannot be easily overridden  
👉 It always runs unless explicitly changed  

________________________________________

### EXPOSE
Defines port  
👉 documentation/networking  
EXPOSE 5000  

________________________________________

### ENV
Sets environment variables  
👉 config  
ENV APP_ENV=prod  

👉 ENV → used during container runtime (run-time)  

________________________________________

### ARG
Build-time variables  
👉 dynamic build values  
ARG VERSION=1.  
👉 ARG → used during image build (build-time)  

## COPY vs ADD (clear and to the point)

### COPY
•	Copies files from local system to container  
•	Simple and predictable  

Example  
COPY app.py /app/  

### ADD
•	Does everything COPY does  
•	Plus extra features:  
o	can extract .tar files automatically  
o	can download files from URL  
o	

ADD example for extracting tar file  

ADD project.tar.gz /app/  

What happens
•	project.tar.gz is copied into /app/  
•	Docker automatically extracts it  

ADD example for extracting tar file  
ADD https://example.com/file.txt /app/file.txt  

What happens
•	Docker downloads file.txt from the URL  
•	stores it inside /app/file.txt  

## What is multi-stage build?
Multi-stage build means using multiple FROM statements in one Dockerfile to build and optimize the final image.

________________________________________

## Why is multi-stage build used?
•	reduce image size  
•	remove unnecessary files (build tools, dependencies)  
•	keep final image clean and secure  

## What is docker build context?
Build context is the set of files available to Docker during build.  
👉 It is usually the directory you pass:  
docker build .  
OR  
docker build /home/user/project  

NOW, context = /home/user/project  
👉 Docker can access:
•	all files in that folder  
•	subfolders  

## How do you create a container?
A container is created from an image using docker create or docker run.  

Example:  
docker create nginx  

________________________________________

## How do you run a container?
Use:  
docker run nginx  
This creates and starts the container.

________________________________________

## What is detached mode in Docker?
Detached mode means the container runs in the background.  

Example:  
docker run -d nginx  

________________________________________

## What is interactive mode in Docker?
Interactive mode lets you interact with the container through terminal.  

Example:  
docker run -it ubuntu bash  

________________________________________

## How do you list running containers?
Use:  
docker ps  
To list all containers:  
docker ps -a  

________________________________________

## How do you stop a container?
Use:  
docker stop <container_id_or_name>  

Example:  
docker stop mycontainer  

________________________________________

## How do you start a stopped container?
Use:  
docker start <container_id_or_name>  

Example:  
docker start mycontainer  

________________________________________

## How do you remove a container?
Use:  
docker rm <container_id_or_name>  

Example:  
docker rm mycontainer  

________________________________________

## What is container restart policy?
Restart policy tells Docker whether the container should restart automatically after failure or reboot.  

Examples:
•	no  
•	always  
•	on-failure  
•	unless-stopped  

Example:  
docker run --restart always nginx  

________________________________________

## What is container ID?
Container ID is the unique identifier assigned to each container by Docker.  

Example:  
docker ps  
It shows something like:  
8f3a2b1c4d5e  

________________________________________

## What is container name?
Container name is the human-readable name of a container.  

Example:  
docker run --name webserver nginx  
Here webserver is the container name.

________________________________________

## How do you view container logs?
Use:  
docker logs <container_id_or_name>  

Example:  
docker logs webserver  

________________________________________

## How do you execute commands inside a container?
Use:  
docker exec -it <container_name> bash  

Example:  
docker exec -it webserver bash  

________________________________________

## What is docker exec?
docker exec is used to run a new command inside a running container.  

Example:  
docker exec -it webserver ls /app  

________________________________________

## What is docker attach?
docker attach connects your terminal to the main running process of the container.  

Example:  
docker attach webserver  

________________________________________

## docker exec vs docker attach
•	docker exec → runs a new command inside container  
•	docker attach → connects to the main process already running  

________________________________________

## What is Docker volume?
A Docker volume is a persistent storage managed by Docker used to store container data outside the container.

________________________________________

## Why do we use volumes in Docker?
•	to persist data even if container is deleted  
•	to share data between containers  
•	to separate data from container lifecycle  

________________________________________

## What is bind mount?
A bind mount links a host machine directory directly to a container directory.

________________________________________

## What is the difference between volume and bind mount?

Volume	Bind Mount  
managed by Docker	managed by user (host path)  
stored in Docker storage	stored in host filesystem  
portable and safer	depends on host path  
preferred in production	used in development  

________________________________________

## What is data persistence in Docker?
Data persistence means data remains safe even if the container stops or is deleted.

________________________________________

## What happens to data when container is deleted?
•	without volume → data is lost  
•	with volume → data is preserved  

________________________________________

## How do you create a volume?
docker volume create myvolume  

________________________________________

## How do you mount a volume to a container?
docker run -v myvolume:/app nginx  

________________________________________

## What is read-only volume?
A read-only volume allows the container to read data but not modify it.  

Example:  
docker run -v myvolume:/app:ro nginx  

________________________________________

## What is Docker Compose?
Docker Compose is a tool used to run and manage multiple containers together using a single configuration file.

________________________________________

## Why is Docker Compose used?
•	to manage multi-container apps  
•	to avoid running multiple docker run commands  
•	to define everything in one file  

________________________________________

## What is docker-compose.yml file?
It is a configuration file where we define:
•	services (containers)  
•	images  
•	ports  
•	volumes  
•	environment variables  

________________________________________

## What is service in Docker Compose?
A service represents one container (or group of containers).  

Example:
•	frontend service  
•	backend service  
•	database service  

________________________________________

## How do you start services using Docker Compose?
docker-compose up  

Detached mode:  
docker-compose up -d  

________________________________________

## How do you stop services in Docker Compose?
docker-compose down  

________________________________________

## What is multi-container application?
An application that uses multiple containers working together.  

Example:
•	frontend + backend + database  

________________________________________

## How do containers communicate in Docker Compose?
Containers communicate using:  
👉 service names as hostnames  

Example:
•	backend connects to database using db (service name)  

________________________________________

## What is depends_on in Docker Compose?
depends_on defines startup order of services  

Example:  
depends_on:  
  - db  

👉 backend starts after db  

________________________________________

## What is environment variable in Compose?
Used to pass configuration values to containers.  

Example:  
environment:  
  - DB_HOST=db  
  - PORT=5000  

## What is docker pull?
Used to download an image from a registry (Docker Hub) to local system.  
docker pull nginx  

________________________________________

## What is docker push?
Used to upload a local image to a registry.  
docker push myusername/myimage:v1  

________________________________________

## How do you tag an image?
Used to give name and version to an image.  
docker tag myimage myusername/myimage:v1  

________________________________________

## How do you push image to Docker Hub?
Step 1: Login  
docker login  

Step 2: Tag image  
docker tag myimage myusername/myimage:v1  

Step 3: Push  
docker push myusername/myimage:v1  

________________________________________

## What is docker login?
Used to authenticate with Docker Hub or registry.  
docker login  

________________________________________

## What is docker logout?
Used to log out from Docker registry.  
docker logout  

________________________________________

## What is image versioning?
Image versioning means using tags to manage different versions of images.  

Example:  
myapp:v1  
myapp:v2  
myapp:latest  

________________________________________

## What is container cleanup?
Removing unused:
•	containers  
•	images  
•	volumes  
•	networks  
to free space.

________________________________________

## What is docker prune?
Used to clean up unused Docker resources.  

Examples:  
docker container prune   # remove stopped containers  
docker image prune       # remove unused images  
docker system prune      # remove all unused resources  

## Why is container security important?
Container security is important because insecure containers can expose the application, host system, and sensitive data to attacks.

________________________________________

## What is root user in container?
Root user in a container is the highest-privileged user, similar to root user in Linux.

________________________________________

## Why should containers not run as root?
Containers should not run as root because if the container is compromised, the attacker may get higher privileges on the host or misuse container resources.

________________________________________

## What is image scanning?
Image scanning is the process of checking Docker images for known security vulnerabilities, outdated packages, and unsafe dependencies.

________________________________________

## What are vulnerabilities in Docker images?
Vulnerabilities are security weaknesses in image packages, libraries, OS components, or application dependencies.

________________________________________

## What is least privilege in containers?
Least privilege means giving the container only the minimum permissions it needs to run.

________________________________________

## What is secrets management in containers?
Secrets management means securely storing and providing sensitive data like passwords, API keys, and tokens to containers.

________________________________________

## Why should secrets not be stored in images?
Secrets should not be stored in images because images can be shared, pushed to registries, and accessed by others, which can expose sensitive data.

________________________________________

## How can you reduce Docker image size?
•	use minimal base images  
•	use multi-stage builds  
•	remove unnecessary packages and files  
•	combine commands where possible  
•	copy only required files  

________________________________________

## Why should we use minimal base images?
Minimal base images reduce image size, improve security, and reduce unnecessary packages.

________________________________________

## What is caching in Docker build?
Caching means Docker reuses previously built layers if they have not changed, which makes builds faster.

________________________________________

## Why should we avoid unnecessary layers?
Unnecessary layers increase image size and make builds less efficient.

________________________________________

## What is .dockerignore file?
.dockerignore is a file used to exclude unnecessary files and folders from the Docker build context.

________________________________________

## Why is .dockerignore important?
It is important because it reduces build context size, speeds up builds, and prevents unwanted files from going into the image.

________________________________________

## What are best practices for writing Dockerfile?
•	use small base images  
•	use multi-stage builds  
•	copy only required files  
•	avoid unnecessary layers  
•	run containers as non-root  
•	use .dockerignore  
•	keep Dockerfile simple and clean  

________________________________________

## Why should containers be stateless?
Containers should be stateless so they can be easily replaced, scaled, restarted, or redeployed without depending on local container data.

________________________________________

## What is immutable infrastructure in context of containers?
Immutable infrastructure means we do not modify running containers. If changes are needed, we build a new image and redeploy a new container.

________________________________________

## Why are containers important in DevOps?
Containers are important in DevOps because they make applications portable, consistent, easy to deploy, and easy to scale.

________________________________________

## How do containers help in CI/CD?
Containers help in CI/CD by providing the same runtime environment for build, test, and deployment, which reduces environment issues.

________________________________________

## Practical / scenario-based questions”doc

________________________________________

## If your container is not starting
“First I check container logs using docker logs to see the error. Then I check container status using docker ps -a. I verify the CMD or ENTRYPOINT and image configuration. Based on the error, I fix the issue and restart the container.”

________________________________________

## If your container crashes immediately
“First I check logs to identify the error. Then I verify the startup command, dependencies, and environment variables. Usually crashes happen due to wrong command or missing dependencies, so I fix and rerun.”

________________________________________

## If port is not accessible
“First I check if the container is running. Then I verify port mapping using docker ps. I check if the application inside the container is listening on the correct port, and also verify network or firewall issues.”

________________________________________

## If you want to persist data
“I use Docker volumes. I create a volume and mount it to the container so that data is stored outside the container and not lost when the container is removed.”

________________________________________

## If image size is too large
“I use a minimal base image, apply multi-stage build, remove unnecessary files, and optimize layers to reduce image size.”

________________________________________

## If multiple containers need to run together
“I use Docker Compose, where I define all services in a YAML file and start them together using a single command.”

________________________________________

## If you want to update container without downtime
“I follow a rolling update approach. I start a new container with the updated image, shift traffic to it, and then stop the old container.”

________________________________________

## If container is consuming high memory
“I check resource usage using docker stats, analyze logs and application behavior, and then apply memory limits or optimize the application.”

________________________________________

## If you want to debug inside container
“I use docker exec -it <container> bash to enter the container and debug the application.”

________________________________________

## If containers cannot communicate
“I check if both containers are in the same network, verify service/container names, and ensure the target service is running and accessible.”

________________________________________

## If you want environment-specific configs
“I use environment variables, either through Dockerfile, runtime flags, or Docker Compose .env files to manage different configurations.”

________________________________________

## 🔥 Final strong interview pattern
Always speak like this:  
👉 “First I check logs → then configuration → then environment/network → then fix”

________________________________________

## ⚡ One-line memory
👉 Debug = logs → config → network → resources

