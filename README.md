## Prerequisites:

You need to have `docker` installed.

# Running Individual Containers with Docker:

---

## Create Network

`docker network create goals-net`

---

## Run MongoDB Container

```
  docker run --name mongodb \
 -e MONGO_INITDB_ROOT_USERNAME=root \
 -e MONGO_INITDB_ROOT_PASSWORD=secret \
 -v data:/data/db \
 --rm \
 -d \
 --network goals-net \
 mongo
```

---

## Build Node API Image

`docker build -t goals-node .`

---

## Run Node API Container

```
 docker run --name goals-backend \
 -e MONGODB_USERNAME=root \
 -e MONGODB_PASSWORD=secret \
 -v logs:/app/logs \
 -v $(pwd):/app \
 -v /app/node_modules \
 --rm \
 -d \
 --network goals-net \
 -p 80:80 \
 goals-node
```

---

## Build React SPA Image

`docker build -t goals-react .`

---

## Run React SPA Container

```
 docker run --name goals-frontend \
 -v $(pwd):/app/src \
 --rm \
 -d \
 -p 3000:3000 \
 -it \
 goals-react
```

---

## Stop all Containers

`docker stop mongodb goals-backend goals-frontend`

# Running Entire Application with Docker Compose:

- cd into repository and then run `docker compose up -d` to start running the entire application on localhost:3000
