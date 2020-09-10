# 1.1 Getting started

```
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS                      PORTS               NAMES
a093a6e6afa7        nginx               "/docker-entrypoint.…"   10 seconds ago      Up 9 seconds                80/tcp              dazzling_mclean
93e61e4d8956        nginx               "/docker-entrypoint.…"   6 minutes ago       Exited (0) 44 seconds ago                       vigorous_hertz
0538daf22e49        nginx               "/docker-entrypoint.…"   9 minutes ago       Exited (0) 9 minutes ago                        crazy_easley
```

# 1.2 Cleanup
```
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES

REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
```

# 1.3 Hello Docker Hub
```
Give me the password: basics
You found the correct password. Secret message is:
"This is the secret message"
```

# 1.4
```
docker exec -it angry_payne bash
root@af2cbbf7c403:/usr/app# tail -f ./logs.txt
Tue, 01 Sep 2020 14:19:00 GMT
Tue, 01 Sep 2020 14:19:03 GMT
Tue, 01 Sep 2020 14:19:06 GMT
Tue, 01 Sep 2020 14:19:09 GMT
Secret message is:
"Docker is easy"
```

# 1.5
```
docker run -it --rm ubuntu sh -c \
   'apt-get update;
   apt install curl;
   echo "Input website:";
   read website;
   echo "Searching..";
   sleep 1;
   curl http://$website;'
```

# 1.6
Dockerfile:
```
FROM devopsdockeruh/overwrite_cmd_exercise

CMD ["--clock", "10"]
```

command:
```
docker build -t docker-clock .
docker run -it docker-clock
```

# 1.7
Dockerfile:
```
FROM ubuntu:16.04

WORKDIR /usr/myapp

RUN apt-get update && apt-get install -y curl
COPY commands.sh commands.sh
RUN chmod +x commands.sh

CMD ["./commands.sh"]
```


command:
```
docker build -t curler .
docker run -it curler
```

# 1.8
```
docker run -d -v $(pwd)/logs.txt:/usr/app/logs.txt devopsdockeruh/first_volume_exercise
Wed, 02 Sep 2020 07:53:41 GMT
Wed, 02 Sep 2020 07:53:44 GMT
Wed, 02 Sep 2020 07:53:47 GMT
Wed, 02 Sep 2020 07:53:50 GMT
Secret message is:
"Volume bind mount is easy"
```

# 1.9
```
docker run -p 1234:80 devopsdockeruh/ports_exercise
```

# 1.10
Dockerfile:
```
FROM node

COPY ./frontend/ /usr/myapp/
WORKDIR /usr/myapp/frontend/
RUN npm install
EXPOSE 5000

ENTRYPOINT [ "npm", "start" ]
```

# 1.11
Dockerfile:
```
FROM node

COPY ./backend/ /usr/myapp/backend
WORKDIR /usr/myapp/backend
RUN npm install
EXPOSE 8000

ENTRYPOINT [ "npm", "start" ]
```

command:
```
docker build -t backend .
docker run -it -v $(pwd)/logs.txt:/usr/myapp/backend/logs.txt -p 8000:8000 backend
```

# 1.12
## frontend
```
FROM node
COPY ./frontend/ /usr/myapp/frontend
WORKDIR /usr/myapp/frontend
RUN npm install
EXPOSE 5000
ENV API_URL=http://localhost:8000
ENTRYPOINT [ "npm", "start" ]
```

```
docker build -t frontend .
docker run -d -p 5000:5000 frontend
```

## backend
```
FROM node

COPY ./backend/ /usr/myapp/backend
WORKDIR /usr/myapp/backend
RUN npm install
EXPOSE 8000
ENV FRONT_URL=http://localhost:5000
ENTRYPOINT [ "npm", "start" ]
```

```
docker build -t backend .
docker run -d -p 8000:8000 backend
```

# 1.13
```
FROM openjdk:8

COPY ./spring/ /usr/myapp/spring
WORKDIR /usr/myapp/spring
RUN ./mvnw package
EXPOSE 8080

ENTRYPOINT [ "java", "-jar", "./target/docker-example-1.1.3.jar" ]
```

# 1.14
```
FROM ruby:2.6.0

COPY sources.list-debian /etc/apt/sources.list
RUN apt-get update && apt-get install -y nodejs 

COPY ./ruby/ /usr/myapp/ruby
WORKDIR /usr/myapp/ruby
RUN bundle config mirror.https://rubygems.org https://gems.ruby-china.com
RUN bundle install
RUN rails db:migrate
EXPOSE 3000

ENTRYPOINT [ "rails", "s" ]
```

# 1.15
```
https://hub.docker.com/repository/docker/reezexue/simpleapp
```

# 1.16
```
https://simpleappxue.herokuapp.com/presses/new
```

# 1.17
```
https://hub.docker.com/repository/docker/reezexue/py-np
```