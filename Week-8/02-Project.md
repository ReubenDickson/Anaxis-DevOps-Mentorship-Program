WEEK 8 REAL-WORLD PROJECT
“Containerized Multi-Service Application with Persistence”

Scenario: Your company is building a simple web application:

Frontend: Nginx

Cache: Redis

- Data must persist

- Configuration must be externalized

You are responsible for containerizing and composing the system.


Project Tasks

1. Create project structure
``` bash
week8-project/
├── docker-compose.yml
├── html/
│   └── index.html
└── README.md
```

2. Web page
<h1>Docker Compose App</h1>
<p>This app runs multiple services.</p>

3. docker-compose.yml
``` bash
version: "3.8"

services:
  web:
    image: nginx
    ports:
      - "8080:80"
    volumes:
      - ./html:/usr/share/nginx/html
    environment:
      - APP_ENV=production

  redis:
    image: redis
    volumes:
      - redis_data:/data

volumes:
  redis_data:
```

4. Run the system
``` bash
docker-compose up -d
```

Verify:
``` bash
docker ps
curl localhost:8080
docker volume ls
docker volume inspect week8-project_redis_data
```

5. Test persistence
``` bash
docker-compose down
docker-compose up -d
```

Confirm:

i. web page still loads

ii. redis volume still exists
