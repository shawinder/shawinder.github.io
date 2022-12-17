---
layout: post
lang: Docker
title: Debug Angular application locally using Docker Host Mounting
comments: true
description : How to debug angular changes locally using Docker Host Mounting
date: 2022-12-16
header_desc: Debug Angular application locally using Docker Host Mounting
---

- package.json

```js
{
  "name": "my-app",
  "version": "0.0.0",
  "scripts": {
    "start": "ng serve",
    "start:docker": "ng serve --host 0.0.0.0"
    ....
```

- Dockerfile 

```shell
FROM node:10.24-alpine
WORKDIR /app
COPY package.json ./
RUN npm install
CMD [ "npm", "run", "start:docker"]
EXPOSE 4200
```

- docker-compose.yml

```shell
version: '3.4'
services:
  app:
    build:
      context: ./
      dockerfile: Dockerfile
    ports:
      - '4200:4200'
    volumes:
      - './:/app'
      - '/app/node_modules'
    depends_on:
      - db
  db:
    image: mysql:5.7
    restart: always
    environment:
      MYSQL_DATABASE: 'sample'
      MYSQL_USER: 'testuser'
      MYSQL_PASSWORD: 'testpass'
      MYSQL_ROOT_PASSWORD: 'testpass'
    ports:
      - '3306:3306'
    expose:
      - '3306'
    volumes:
      - db:/var/lib/mysql
volumes:
  db:
```
