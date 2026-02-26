WEEK 8 - Docker Deep Dive: Volumes, Environment Variables & Docker Compose
From Single Containers to Real Applications

Week 8 Purpose (Read This First)

In Week 7, you learned how to:

- run containers

- build images

- deploy a simple app

That’s good - but real applications are never that simple.

In the real world:

- applications need data that must not disappear

- applications need configuration

- applications are made of multiple services

- restarting one container should not destroy everything

Week 8 teaches you how DevOps engineers solve these problems.



1. The First Real-World Container Problem: Data Loss

What Beginners Usually Don’t Expect: When a container stops or is deleted, all data inside it is lost

Example:
``` bash
docker run -it ubuntu bash
touch test.txt
exit
```

Restart container → test.txt is gone.

This is not a bug.
This is how containers are designed.

Containers are meant to be stateless.

So how do real apps keep data?



2. Docker Volumes - Making Containers Stateful

A Docker volume stores data outside the container, on the host.

Create a volume
``` bash
docker volume create app_data
```

Use volume in a container
``` bash
docker run -d \
  -p 8080:80 \
  -v app_data:/usr/share/nginx/html \
  nginx
```

Now container can be destroyed and yet data remains

This is how databases, uploads and logs are handled in production.


DevOps Thinking

Containers are disposable.
Data is not.



3. Environment Variables — Configuring Containers Properly

- Hardcoding values is dangerous.

- Environment variables allow:

i. configuration without changing code

ii. different settings per environment (dev, staging, prod)

Example
``` bash
docker run -e APP_ENV=production -e APP_PORT=3000 my-app
```

Inside container:
``` bash
echo $APP_ENV
```

Why DevOps Engineers Care:

- secrets

- ports

- database URLs

- API keys

All are injected at runtime.



4. The Next Real-World Problem: Multiple Containers

Real applications consist of:

i. frontend

ii. backend

iii. database

iv. cache (for quicker retrieval of data)

Running them manually:

i. is error-prone

ii. does not scale

iii. is not repeatable

This is why Docker Compose exists.



5. Docker Compose — Defining Systems, Not Containers

Docker Compose allows you to define an entire application stack in one file.

Install Docker Compose (Ubuntu)
``` bash
sudo apt install docker-compose -y
```


6. Your First docker-compose.yml
``` bash
sudo nano docker-compose.yml
```
Add the content below to the file;
``` bash
version: "3.8"

services:
  web:
    image: nginx
    ports:
      - "8080:80"

  redis:
    image: redis

```

Start everything:
``` bash
docker-compose up -d
```

Stop everything:
``` bash
docker-compose down
```

This is infrastructure as code at a small scale.