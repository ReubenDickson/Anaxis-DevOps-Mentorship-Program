WEEK 7 REAL-WORLD PROJECT

“Containerize and Run a Simple Web Application”

Scenario: Your team wants to deploy a simple web app.
They want it:

- portable

- reproducible

- easy to redeploy

You are asked to containerize it using Docker.


Project Tasks

1. Create a simple web page
``` bash
mkdir docker-project
cd docker-project
nano index.html
```

Content:

<h1>Hello from Docker!</h1>
<p>This app runs inside a container.</p>


2. Create a Dockerfile
``` bash
sudo nano Dockerfile
```

Add the following content to the Dockerfile
``` bash
FROM nginx:latest
COPY index.html /usr/share/nginx/html/index.html
```

3. Build Docker image
``` bash
docker build -t my-web-app .
```


4. Run the container
``` bash
docker run -d -p 8080:80 my-web-app
```

5. Verify
``` bash
curl localhost:8080
```