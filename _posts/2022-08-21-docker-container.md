---
title: "Python development with Docker"
categories:
  - blog
tags:
  - python
---

In this article we will define and create a docker image which is then ran in docker container.

The example here is intended for local development. This is useful if you want run things on other operating system
than you are currently using, for example, using Windows but running Linux environments.

## Project structure

```bash
.
+-- app
|   +-- <application code>
+-- Dockerfile
+-- docker-compose.yml
+-- requirements.txt
```

## Dockerfile definition

The base Python image can be whatever is suitable for your workload.

How the `Dockerfile` looks like:

```
FROM python:3.10-slim-bullseye

WORKDIR /usr/src/app

COPY requirements.txt .
RUN pip install -r requirements.txt
```

## With Docker build and run

### Build the image

Then we build the image and name it `app-py310`.

In this example we build the image in project root where `Dockerfile` and `requirements.txt` are located.

```bash
docker build -t app-py310 .
```

### Start the container

Then we start a container in interactive mode (bash session) with the previously built image.

```bash
docker run -it --name app-py310 --rm --volume ${PWD}:/usr/src/dev app-py310 bash
```

## With Docker compose

Create `docker-compose.yml`

```bash
version: "3"
services:
  app:
    build: .
    stdin_open: true # same as -i
    tty: true        # same as -t
    command: /bin/bash
```

#### Build the image

```bash
docker-compose build app
```

#### Start the container

Here we attach our current directory (`${PWD}`) to `/usr/src/dev` so we can modify files locally, but
run necessary things in docker.

```bash
docker-compose run --volume ${PWD}:/usr/src/dev app
```
