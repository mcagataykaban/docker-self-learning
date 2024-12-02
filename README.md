# Development Environment Setup with Docker Compose

This repository demonstrates how to replace multiple Docker commands with a Docker Compose configuration for a simpler development environment setup.

## Before (Using Docker Commands)

Previously, we had to run multiple Docker commands separately:

---

## Create Network

```
docker network create goals-net
```

---

## Run MongoDB Container

```
docker run --name mongodb \
 -e MONGO_INITDB_ROOT_USERNAME=cagatay \
 -e MONGO_INITDB_ROOT_PASSWORD=secret \
 -v data:/data/db \
 --rm \
 -d \
 --network goals-net \
 mongo
```

---

## Build Node API Image

```
docker build -t goals-node .
```

---

## Run Node API Container

```
docker run --name goals-backend \
 -e MONGODB_USERNAME=<username> \
 -e MONGODB_PASSWORD=<password> \
 -v logs:/app/logs \
 -v /<projectLocalPath>/backend:/app \
 -v /app/node_modules \
 --rm \
 -d \
 --network goals-net \
 -p 80:80 \
 goals-node
```

---

## Build React SPA Image

```
docker build -t goals-react .
```

---

## Run React SPA Container

```
docker run --name goals-frontend \
 -v /<projectLocalPath>/frontend/src:/app/src \
 --rm \
 -d \
 -p 3000:3000 \
 -it \
 goals-react
```

---

## Stop all Containers

```
docker stop mongodb goals-backend goals-frontend
```

## After (Using Docker Compose)

Now, all these commands are replaced with a single `docker-compose.yml` file:

```
docker-compose up -d
```

## Benefits of Using Docker Compose

- **Simplified Commands**: Instead of running multiple Docker commands, we now use single commands

  - Start all services: `docker-compose up -d`
  - Stop all services: `docker-compose down`
  - View logs: `docker-compose logs`

- **Automatic Network Creation**: Docker Compose automatically creates and manages networks

- **Dependencies Management**: Services can be configured to wait for other services using `depends_on`

- **Environment Consistency**: All team members use the same configuration

## Prerequisites

- Docker
- Docker Compose
- Git

## Getting Started

1. Clone the repository:

```
git clone [repository-url]
cd [repository-name]
```

2. Start the environment:

```
docker-compose up -d
```

## Available Services

- Application (Node.js) - Port 80
- Mongodb - Port 27017
- React SPA - Port 3000
