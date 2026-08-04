**Intro**
What are Containers?
Containers package applications and all their dependencies into portable, isolated units that run consistently across environments, while Docker provides the engine that makes containerization possible.

Benefits of Containers
Containers provide isolated, consistent, and resource efficient environments for applications, making deployments more reliable and solving compatibility issues across different systems.

What is Docker?
Docker is a platform that simplifies container management through three main components: Docker Engine (runs containers), Docker Hub (stores and shares images), and Docker Compose (manages multiple containers together).

Images and Containers
Docker images are immutable templates used to create containers, containers are the running instances of those images, and Dockerfiles provide the instructions for building the images. Together, they form the foundation of how Docker packages, deploys, and runs applications consistently across environments.
```
Dockerfile
↓
Docker Image (template)
↓
Docker Container (running application)
```

Importance in Modern Development
Docker is a game changing platform that improves software delivery by providing consistent environments, lightweight deployment, easier collaboration, and seamless automation through CI/CD pipelines.

VMs vs. Containers
Both VMs and containers provide isolation, but VMs achieve it by running separate operating systems, while containers share the host OS kernel, making them much lighter, faster, and more efficient for modern cloud native applications.

**Docker Setup & Fundamentals**
Docker Installation
https://www.docker.com/products/docker-desktop/
Verify Installation
```
docker --version
```
Very useful for debugging
```
docker info
```
Run your first container. Good for testing
```
docker run hello-world
```
Show running containers
```
docker ps
```
Show all containers
```
docker ps -a
```

**Understanding the Dockerfile**
A Dockerfile is a text file containing a series of instructions used to build a Docker image.
Docker Workflow
```
Dockerfile
↓
docker build
↓
Docker Image
↓
docker run
↓
Docker Container
```
Example Dockerfile (Node.js)
```
FROM node:14
# Uses the official Node.js 14 image.

WORKDIR /app
# Sets `/app` as the working directory.

COPY package*.json ./
# Copies dependency definition files.

RUN npm install
# Installs required Node.js packages.

COPY . .
# Copies the rest of the application code.

EXPOSE 3000
# Indicates the application listens on port 3000.

CMD ["node", "index.js"]
# Starts the Node.js application when the container runs.
```
A Dockerfile is the blueprint for building Docker images. The most important instructions are FROM, RUN, COPY, WORKDIR, and CMD, which together define the environment, dependencies, application files, and startup behaviour of a container.

**Introduction to Docker Networking**
Docker networking provides the mechanisms that allow containers to communicate efficiently and securely, whether they're running on the same host or across multiple machines.

Lists all available Docker networks.
```
docker network ls
```
Creates a new Docker network.
```
docker network create <network-name>
```
Connects a container to a network.
```
docker network connect <network-name> <container>
```

**Docker Network Types**
Bridge Network (Default)
The bridge network is Docker's default networking mode.
```
Containers on the same host can communicate.
Each container gets its own IP address.
Isolated from the host machine's network.
Provides an extra layer of security.
```
Use case: Most standard container-to-container communication on a single machine.

Host Network
In host mode, the container shares the host machine's network directly.
```
No network isolation between host and container.
Container uses the host's networking stack.
Potentially better performance.
```
Use case: Applications that need direct access to host networking.

None Network
The none network completely disables networking.
```
No network interfaces.
No inbound or outbound connectivity.
Fully isolated container.
```
Use case: Highly secure workloads or applications that should have no network access.

**Introduction to Docker Compose**
Docker Compose is a tool used to define and manage multi-container Docker applications. Instead of starting and managing containers individually, you can define all application services in a single Docker Compose YAML file and control them together.

Simplifies multi-container management. Ideal for applications that consist of multiple services, such as:
Web server, Database, & Cache.

Uses a Docker Compose YAML file, acts as a blueprint for the application.
Defines: Container images, Port mappings, Environment settings, Service relationships and interactions.

Manage the entire application stack with simple commands.
```
docker compose up
```
Starts all services defined in the YAML file.

Why Docker Compose is Important in DevOps
Docker Compose is a key DevOps tool because it simplifies managing multi-container applications, improves consistency across environments, and enhances team collaboration.

1. Simplifies Development and Testing
	1. Developers can define all required services (e.g., MySQL, Nginx, Redis) in a single Docker Compose YAML file.
	2. A complete development or testing environment can be started with a single command
	3. Eliminates the need to manually configure and start each service.
	4. Allows developers to focus on coding rather than infrastructure setup.
2. Ensures Environment Consistency
	1. Solves the common "it works on my machine" problem.
	2. Every developer, tester, and CI/CD pipeline uses the same environment definition.
	3. Ensures consistency across. Local development, Testing environments, CI/CD pipelines, & Production.
	4. Reduces bugs caused by environmental differences and increases software reliability.
3. Improves Team Collaboration
	1. Environment configurations can be stored and version-controlled alongside application code.
	2. Team members can easily share the same setup.
	3. New developers can quickly get started by:
		1. Cloning the repository
		2. Running docker-compose up
		3. Having the full environment ready within minutes
	4. Reduces onboarding time and integration issues.

What is a docker-compose.yml file?
It acts as a blueprint or recipe for your application.
Defines all services your application requires.
Specifies: Docker images to use, Build instructions, Port mappings, Environment variables, & Service dependencies.

**Docker Registries**
A Docker Registry is a centralized repository used to store, manage, and distribute Docker images. After building Docker images locally, registries provide a place where those images can be stored and later pulled for deployment across different environments.

What is a Docker Registry?
A storage and distribution hub for Docker images.
Similar to an online library where Docker images are kept when they are not running as containers.
Makes images accessible to developers, teams, and deployment pipelines.

Public Registries - Example: Docker Hub
Accessible to everyone.
Used to share images publicly or consume community-provided images.
Common images such as MySQL are often pulled from Docker Hub.

Private Registries - Example: AWS Elastic Container Registry (ECR)
Access is restricted and secure.
Ideal for proprietary or sensitive applications.
Provides control over who can view and use images.

Why Docker Registries Are Important in DevOps
Streamline Deployments. Improve Collaboration. Ensure Consistency.

Typical Docker Registry Workflow
1. Build a Docker image locally.
2. Test the image by running a container.
3. Push the image to a registry (e.g., AWS ECR).
4. Pull the image from the registry in other environments.
5. Deploy using CI/CD pipelines.
```
Build → Test → Push → Registry → Pull → Deploy
```

**Docker Hub**
Often described as the "App Store for Docker images" because it hosts thousands of official and community contributed images.

Log In from the Command Line
```
docker login
```

Building and Tagging an Image
```
docker build -t username/repository:v1 .
```
-t = tag the image
username = Docker Hub username
repository = repository name
v1 = image version tag
. = current directory containing the Dockerfile

Pushing an Image to Docker Hub
```
docker push username/repository:v1
```

Pulling an Image from Docker Hub
```
docker pull username/repository:v1
```

**Pushing our Images to Amazon ECR**
Elastic Container Registry is AWS's fully managed Docker registry service. It allows you to: Store Docker images securely, Manage private container repositories, Pull images for deployments. & Integrate with AWS services and CI/CD pipelines.

Unlike Docker Hub, ECR is commonly used for private, production-grade workloads.

Creating an ECR Repository
1. Create and log in to an AWS account.
2. Open the AWS Management Console.
3. Search for Elastic Container Registry (ECR).
4. Create a new private repository (e.g., flask-mysql).
5. Open the repository and select View Push Commands.

AWS generates the commands needed to: Authenticate, Build images, Tag images, & Push images.

Authenticating with ECR using AWS CLI
https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html

**Building and Tagging Images**
First, build the Docker image
```
docker build -t flask-mysql .
```

Then tag the image with the ECR repository URI
Tagging associates the local image with the destination ECR repository.
```
docker tag flask-mysql:latest <ecr-repository-uri>:latest
```

Pushing Images to ECR
```
docker push <ecr-repository-uri>:latest
```

Once complete: The image is stored in ECR, Image metadata becomes visible in the AWS Console, & The image is available for deployments.

Pulling Images from ECR
```
docker pull <ecr-repository-uri>:latest
```

Running a Container from ECR
```
docker run -p 5002:5002 <ecr-repository-uri>:latest
```

**Using Docker Compose with ECR Images**

Instead of building the Flask image locally
```
web:
  build: .
```
The Docker Compose file is updated to pull the image directly from ECR
```
web:
  image: <ecr-repository-uri>
```

**Important Docker Commands to know**

List All Images
```
docker images
```
Displays all images stored locally, including: Repository name, Tag/version, Image ID, Creation date, Image size.
Useful for seeing which images are available and identifying old or unused images.

Inspect and image
```
docker inspect <image-id>
```
Provides detailed information about a specific image, including: Configuration, Environment variables, Layers, & Metadata.
Useful for troubleshooting and understanding how an image is configured.

Remove an Image
```
docker rmi <image-id>
```
Deletes a Docker image.

Remove Unused Resources
```
docker system prune
```
Removes: Stopped containers, Unused networks, Dangling images, Build cache.

List Running Containers
```
docker ps
```
Shows currently running containers, including: Container ID, Image, Status, Ports, Container name.

Stop Containers
```
docker stop <container-id>
```
Can also stop multiple with container1 container2 on same line.

Remove Containers
```
docker rm <container-id>
```

Common Docker Cleanup Workflow
```
docker ps
docker stop <container-id>
docker rm <container-id>
docker images
docker rmi <image-id>
docker system prune
```

**Making Our Image Lighter: Multistage Builds**
A Docker technique used to create smaller, more efficient images by separating the build environment from the final runtime environment.

Multi-stage builds allow a Dockerfile to use multiple FROM statements. Typically:
Build Stage
	Installs development tools and dependencies.
	Compiles or prepares the application.
Production Stage
	Starts from a clean, lightweight image.
	Copies only the files required to run the application.
	Excludes build tools and unnecessary dependencies.
This results in a much smaller final image.

