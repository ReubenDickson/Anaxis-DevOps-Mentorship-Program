WEEK 7 — Containers & Docker Fundamentals
Why Containers Exist and How DevOps Engineers Use Them

Week 7 Purpose (Read This First)

Before containers existed, deploying applications was painful.

Typical problems:

- “It works on my laptop but not on the server”, a common statment made by application developers.

- Conflicting software versions

- Manual setup taking hours or days

- Difficult rollbacks

- Inconsistent environments

Docker was created to solve these problems.


This week teaches students:

i. why containers exist

ii. what Docker really is

iii. how DevOps engineers use it in the real world

iv. how to containerize an application

By the end of this week, students will deploy their first containerized application.



1. The Problem Docker Solves (Big Picture First)

Traditional Deployment (Old Way)

To run an app, you had to:

- Install Operating System (OS) dependencies

- Install runtime (Node, Python, Java, etc.)

- Configure environment variables

- Match exact versions

- And then hope nothing breaks

This caused:

- Deployment failures

- Configuration drift

- Inconsistent environments

- Containerized Deployment (Modern Way)

With Docker:

- The app + dependencies are packaged together

- Runs the same everywhere

- Starts in seconds

- Easy to move, scale, and destroy

Docker makes applications portable and predictable.



2. What Is a Container?

A container is a lightweight, isolated environment that runs an application with everything it needs to work.

Containers share the host OS kernel but remain isolated.

Important clarification:

i. A container is NOT a virtual machine

ii. Containers are much lighter and faster



3. Container vs Virtual Machine

Virtual Machine                 	Container
Full OS                         	Shares host OS
Heavy	                            Lightweight
Slow startup	                    Starts in seconds
Resource-intensive	                Efficient

DevOps Engineers prefer containers because:

i. they scale faster

ii. they are easier to automate

iii. they work well in CI/CD



4. What Is Docker?

Docker is the most popular platform for:

- building containers

- running containers

- managing container images

Docker consists of:

i. Docker Engine (runs containers)

ii. Docker Image (blueprint)

iii. Docker Container (running instance)



5. Core Docker Concepts (Very Important)

i. Docker Image

- A template for creating containers

- Built from a Dockerfile

- Immutable (does not change)

ii. Docker Container

- A running instance of an image

- Can be started, stopped, destroyed

iii. Dockerfile

- A text file with instructions

- Describes how to build an image



6. Installing Docker and Buildx from Official Docker Script

This method guarantees that:

- Docker Engine is modern

- Docker CLI sees plugins

- Buildx is installed correctly

It works even if APT repos or keys are broken.

Step 1: Remove old Docker if existing
``` bash
sudo apt remove docker docker.io docker-ce docker-ce-cli docker-buildx-plugin containerd runc -y
```

Step 2: Clean any leftover binaries
``` bash
sudo rm -f /usr/bin/docker
sudo rm -f /usr/local/bin/docker
sudo rm -rf /etc/apt/sources.list.d/docker.list
sudo rm -rf /etc/apt/keyrings/docker.gpg
```

Step 3: Install Docker officially
``` bash
curl -fsSL https://get.docker.com | sudo sh
```

Step 4: Enable Buildx plugin (comes bundled with Docker 20.10+)
``` bash
sudo apt install docker-buildx-plugin -y
```

Step 5: Verify Docker & Buildx
``` bash
docker version
docker buildx version
```

Expected Result
``` bash
docker version → 28.2.2+ ✅

docker buildx version → shows a valid version ✅
```
You can now run:

docker build -t my-web-app .


without legacy builder errors.



7. Running Your First Container

Run a test container:
``` bash
sudo docker run hello-world
```

If successful:

Docker is working

Congratulations, you just ran your first container.



8. Understanding a Real Container Run

Let's run an Nginx container. But first of all, what is Nginx?

Nginx is a web server and reverse proxy.

As a web server, it serves web content - HTML pages, images, files - to clients (browsers).

As a reverse proxy, it receives requests from users and forwards them to one or more backend servers, which helps with load balancing and security.

In practice:

When you type a website URL, Nginx can either serve the website itself or forward the request to an application server (like Node.js, Python, etc.).

It’s lightweight, fast, and widely used in production for high-traffic sites.

In one sentence:

Nginx is a fast, efficient server that delivers websites and forwards requests to backend services when needed.

Now let's run our Nginx container.

``` bash
sudo docker run -d -p 8080:80 nginx
```

Explanation:

-d → detached (runs in background)

-p 8080:80 → map host port 8080 to container port 80

nginx → image name

Test:

curl localhost:8080



9. Managing Containers

List running containers:
``` bash
docker ps
```

Stop container:
``` bash
docker stop <container_id>
```

Remove container:
``` bash
docker rm <container_id>
```

List images:
``` bash
docker images
```