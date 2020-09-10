# 2.1
```
version: '3.5'

services:
    first-volume:
        image: "devopsdockeruh/first_volume_exercise"
        volumes:
            - ./logs.txt:/usr/app/logs.txt
        container_name: first-volume
```

# 2.2
```
version: '3.5'

services:
    ports:
        image: devopsdockeruh/ports_exercise
        ports: 
            - 8000:80

```

# 2.3
```
version: '3.5'

services:
    frontend:
        build:
            context: .
            dockerfile: Dockerfile-front
        ports: 
            - 5000:5000
    backend:
        build:
            context: .
            dockerfile: Dockerfile-back
        ports: 
            - 8000:8000
```

# 2.4
```
docker-compose up --scale compute=5
```

# 2.5
```
version: '3.5'

services:
    frontend:
        build:
            context: .
            dockerfile: Dockerfile-front
        ports: 
            - 5000:5000
        environment: 
            - API_URL=http://localhost:8000
    backend:
        build:
            context: .
            dockerfile: Dockerfile-back
        ports: 
            - 8000:8000
        environment: 
            - FRONT_URL=http://localhost:5000
            - REDIS=redis
            - REDIS_PORT=6379
    redis:
        image: redis
        ports:
            - 6379
```

# 2.6
```
version: '3.5'

services:
    frontend:
        build:
            context: .
            dockerfile: Dockerfile-front
        ports: 
            - 5000:5000
        environment: 
            - API_URL=http://localhost:8000

    backend:
        build:
            context: .
            dockerfile: Dockerfile-back
        ports: 
            - 8000:8000
        environment: 
            - FRONT_URL=http://localhost:5000
            - REDIS=data_db
            - REDIS_PORT=6379
            - DB_USERNAME=postgres
            - DB_PASSWORD=123
            - DB_NAME=msg
            - DB_HOST=msg_db
        depends_on: 
            - msg_db

    data_db:
        image: redis
        ports:
            - 6379
            
    msg_db:
        image: postgres
        restart: unless-stopped
        environment:
            - POSTGRES_USER=postgres
            - POSTGRES_PASSWORD=123
            - POSTGRES_DB=msg
        volumes:
            - database:/var/lib/postgresql/data

volumes:
    database:
```

# 2.7
```
version: '3.5'

services:
    frontend:
        build: 
            context: ./frontend
            dockerfile: Dockerfile
        ports: 
            - 3000:3000
                
    backend:
        build: 
            context: ./backend
            dockerfile: Dockerfile
        volumes: 
            - model:/src/model
        ports: 
            - 5000:5000
            
            
    training:
        build:
            context: ./training
            dockerfile: Dockerfile
        volumes: 
            - model:/src/model
            - imgs:/src/imgs

volumes: 
    model:
    imgs:
```

# 2.8
```
version: '3.5'

services:
    proxy:
        image: nginx
        volumes: 
            - ./nginx.conf:/etc/nginx/nginx.conf
        ports: 
            - 4000:80
        depends_on: 
            - frontend
            - backend

    frontend:
        build:
            context: .
            dockerfile: Dockerfile-front
        environment: 
            - API_URL=http://localhost:4000/api

    backend:
        build:
            context: .
            dockerfile: Dockerfile-back
        environment: 
            - FRONT_URL=http://localhost:4000
            - REDIS=data_db
            - REDIS_PORT=6379
            - DB_USERNAME=postgres
            - DB_PASSWORD=123
            - DB_NAME=msg
            - DB_HOST=msg_db
        depends_on: 
            - msg_db

    data_db:
        image: redis
        ports:
            - 6379
            
    msg_db:
        image: postgres
        restart: unless-stopped
        environment:
            - POSTGRES_USER=postgres
            - POSTGRES_PASSWORD=123
            - POSTGRES_DB=msg
        volumes:
            - database:/var/lib/postgresql/data
 
volumes:
    database:
```

```
events { worker_connections 1024; }

http {
    server {
        listen 80;

        location / {
            proxy_pass http://frontend:5000;
        }

        location /api/ {
            proxy_pass http://backend:8000/;
        }
    }
}
```

# 2.9
```
version: '3.5'

services:
    proxy:
        image: nginx
        volumes: 
            - ./nginx.conf:/etc/nginx/nginx.conf
        ports: 
            - 4000:80
        depends_on: 
            - frontend
            - backend

    frontend:
        build:
            context: .
            dockerfile: Dockerfile-front
        environment: 
            - API_URL=http://localhost:4000/api

    backend:
        build:
            context: .
            dockerfile: Dockerfile-back
        environment: 
            - FRONT_URL=http://localhost:4000
            - REDIS=data_db
            - REDIS_PORT=6379
            - DB_USERNAME=postgres
            - DB_PASSWORD=123
            - DB_NAME=msg
            - DB_HOST=msg_db
        depends_on: 
            - msg_db

    data_db:
        image: redis
        ports:
            - 6379
            
    msg_db:
        image: postgres
        restart: unless-stopped
        environment:
            - POSTGRES_USER=postgres
            - POSTGRES_PASSWORD=123
            - POSTGRES_DB=msg
        volumes:
            - ./database:/var/lib/postgresql/data
 
volumes:
    database:
```

# 2.10
## docker-compose.yml
```
version: '3.5'

services:
    proxy:
        image: nginx
        volumes: 
            - ./nginx.conf:/etc/nginx/nginx.conf
        ports: 
            - 4000:80
        depends_on: 
            - frontend
            - backend

    frontend:
        build:
            context: .
            dockerfile: Dockerfile-front
        environment: 
            - API_URL=http://localhost:4000/api

    backend:
        build:
            context: .
            dockerfile: Dockerfile-back
        environment: 
            - FRONT_URL=http://localhost:4000
            - REDIS=data_db
            - REDIS_PORT=6379
            - DB_USERNAME=postgres
            - DB_PASSWORD=123
            - DB_NAME=msg
            - DB_HOST=msg_db
        depends_on: 
            - msg_db

    data_db:
        image: redis
        ports:
            - 6379
            
    msg_db:
        image: postgres
        restart: unless-stopped
        environment:
            - POSTGRES_USER=postgres
            - POSTGRES_PASSWORD=123
            - POSTGRES_DB=msg
        volumes:
            - ./database:/var/lib/postgresql/data
 
volumes:
    database:
```

## Dockerfile-frontend
```
FROM node

COPY ./frontend/ /usr/myapp/frontend
WORKDIR /usr/myapp/frontend
RUN npm install
EXPOSE 5000
ENTRYPOINT [ "npm", "start" ]
```

## Dockerfile-backend
```
FROM node

COPY ./backend/ /usr/myapp/backend
WORKDIR /usr/myapp/backend
RUN npm install
EXPOSE 5000
ENTRYPOINT [ "npm", "start" ]
```