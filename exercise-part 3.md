# 3.1

## frontend

### old version: 594 MB
```
FROM ubuntu:16.04


COPY source.list.ubuntu /etc/apt/sources.list
RUN apt-get update
RUN apt install -y curl
RUN curl -sL https://deb.nodesource.com/setup_10.x | bash
RUN apt install -y nodejs


COPY ./frontend/ /usr/myapp/frontend
WORKDIR /usr/myapp/frontend
RUN npm config set registry https://registry.npm.taobao.org
RUN npm install
EXPOSE 5000
ENTRYPOINT [ "npm", "start" ]
```

### new version: 526 MB
```
FROM ubuntu:16.04


COPY source.list.ubuntu /etc/apt/sources.list
COPY ./frontend/ /usr/myapp/frontend
WORKDIR /usr/myapp/frontend

RUN apt-get update && \
    apt install -y curl && \
    curl -sL https://deb.nodesource.com/setup_10.x | bash && \
    apt install -y nodejs && \
    npm config set registry https://registry.npm.taobao.org && \
    npm install && \
    rm -rf /var/lib/apt/lists/* && \
    apt-get purge -y --auto-remove curl
    
EXPOSE 5000
ENTRYPOINT [ "npm", "start" ]
```

## backend

### old version: 385 MB
```
FROM ubuntu:16.04


COPY source.list.ubuntu /etc/apt/sources.list
RUN apt-get update
RUN apt install -y curl
RUN curl -sL https://deb.nodesource.com/setup_10.x | bash
RUN apt install -y nodejs


COPY ./backend/ /usr/myapp/backend
WORKDIR /usr/myapp/backend
RUN npm config set registry https://registry.npm.taobao.org
RUN npm install
EXPOSE 5000
ENTRYPOINT [ "npm", "start" ]
```

### new version: 317 MB
```
FROM ubuntu:16.04


COPY source.list.ubuntu /etc/apt/sources.list
COPY ./backend/ /usr/myapp/backend
WORKDIR /usr/myapp/backend

RUN apt-get update && \
    apt install -y curl && \
    curl -sL https://deb.nodesource.com/setup_10.x | bash && \
    apt install -y nodejs && \
    npm config set registry https://registry.npm.taobao.org && \
    npm install && \
    rm -rf /var/lib/apt/lists/* && \
    apt-get purge -y --auto-remove curl

EXPOSE 8000
ENTRYPOINT [ "npm", "start" ]
```

# 3.3
```
FROM ubuntu:16.04

COPY ./project /usr/project
COPY source.list.ubuntu /etc/apt/sources.list
WORKDIR /usr/project
# VOLUME ["/var/run/docker.sock","/var/run/docker.sock"]
RUN chmod +x setup.sh
RUN apt-get update && \
    apt-get -y install apt-transport-https \
        ca-certificates \
        curl \
        gnupg2 \
        software-properties-common && \
    curl -fsSL http://mirrors.aliyun.com/docker-ce/linux/ubuntu/gpg | apt-key add - && \
    add-apt-repository \
    "deb [arch=amd64] http://mirrors.aliyun.com/docker-ce/linux/ubuntu $(lsb_release -cs) stable" && \
    apt-get update && \
    apt-get -y install docker-ce

ENTRYPOINT [ "./setup.sh" ]
```

```
#!/bin/bash

git clone https://github.com/xjyssg/simple-web.git;

docker login -u reezexue -p 1996919dwsq

docker build -t web-app .

docker tag web-app reezexue/buildimage

docker push reezexue/buildimage
```

```
FROM python:3

WORKDIR /usr/webapp
COPY app.py /usr/webapp/app.py
COPY gunicorn.conf.py /usr/webapp/gunicorn.conf.py
RUN pip install -i https://pypi.tuna.tsinghua.edu.cn/simple/ --upgrade pip && \
    pip install -i https://pypi.tuna.tsinghua.edu.cn/simple/ flask gunicorn gevent

EXPOSE 5000
ENTRYPOINT ["gunicorn", "app:app", "-c", "./gunicorn.conf.py"]
```

# 3.4
## frontend
```
FROM node

COPY ./frontend/ /usr/myapp/frontend
WORKDIR /usr/myapp/frontend/
RUN npm config set registry https://registry.npm.taobao.org && \
    npm install && \
    useradd -m app && \
    chown -R app:app ../frontend
EXPOSE 5000

USER app

ENTRYPOINT [ "npm", "start" ]
```

## backend
```
FROM node

COPY ./backend/ /usr/myapp/backend
WORKDIR /usr/myapp/backend/
RUN npm config set registry https://registry.npm.taobao.org && \
    npm install && \
    useradd -m app && \
    chown -R app:app ../backend
EXPOSE 8000

USER app

ENTRYPOINT [ "npm", "start" ]
```

# 3.5
## backend 
- before: 967 MB
- after: 139 MB

```
FROM node:alpine

COPY ./backend/ /usr/myapp/backend
WORKDIR /usr/myapp/backend/
RUN npm config set registry https://registry.npm.taobao.org && \
    npm install && \
    adduser -D app && \
    chown -R app:app ../backend
EXPOSE 8000

USER app

ENTRYPOINT [ "npm", "start" ]
```

## frontend 
- before: 1.18 GB
- after: 348 MB

```
FROM node:alpine

COPY ./frontend/ /usr/myapp/frontend
WORKDIR /usr/myapp/frontend/
RUN npm config set registry https://registry.npm.taobao.org && \
    npm install && \
    adduser -D app && \
    chown -R app:app ../frontend
EXPOSE 5000

USER app

ENTRYPOINT [ "npm", "start" ]
```

# 3.6
image size: 129 MB
```
FROM node:latest as build-stage

COPY ./frontend/ /usr/myapp/frontend
WORKDIR /usr/myapp/frontend/
RUN npm config set registry https://registry.npm.taobao.org && \
    npm install && \
    npm run build

FROM node:alpine

WORKDIR /usr/myapp
COPY --from=build-stage /usr/myapp/frontend/dist /usr/myapp/dist
RUN adduser -D app && \
    chown -R app:app /usr/myapp/dist && \
    npm install -g serve
EXPOSE 5000
USER app

ENTRYPOINT ["serve", "-s", "-l", "5000", "dist" ]
```

# 3.7
## old: 927 MB
```
FROM python:3

WORKDIR /usr/webapp
COPY app.py /usr/webapp/app.py
COPY gunicorn.conf.py /usr/webapp/gunicorn.conf.py
RUN pip install -i https://pypi.tuna.tsinghua.edu.cn/simple/ --upgrade pip && \
    pip install -i https://pypi.tuna.tsinghua.edu.cn/simple/ flask gunicorn gevent

EXPOSE 5000
ENTRYPOINT ["gunicorn", "app:app", "-c", "./gunicorn.conf.py"]
```

## new: 183 MB
```
FROM python:3.6.7-slim

WORKDIR /usr/webapp
COPY app.py /usr/webapp/app.py
COPY gunicorn.conf.py /usr/webapp/gunicorn.conf.py
RUN pip install -i https://pypi.tuna.tsinghua.edu.cn/simple/ --upgrade pip && \
    pip install -i https://pypi.tuna.tsinghua.edu.cn/simple/ flask gunicorn gevent && \
    useradd -m app && \
    chown app:app app.py && \
    chown app:app gunicorn.conf.py

USER app 

EXPOSE 5000
ENTRYPOINT ["gunicorn", "app:app", "-c", "./gunicorn.conf.py"]
```