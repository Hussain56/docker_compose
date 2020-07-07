# Install and Configure appICE components

This repository contains **Docker-Compose file** of [appICE](https://www.appice.io/) components such as [CITUS](https://www.citusdata.com/), [MongoDB](http://www.mongodb.org), [REDIS](https://redis.io/), [Nodejs](https://nodejs.org/en/), [Nginx](https://www.nginx.com/) and [Supervisor](http://supervisord.org/).

### Installation

  Install [Docker](https://docs.docker.com/engine/install/) and then install [docker-compose](https://docs.docker.com/compose/install/).
  
## MongoDB Dockerfile

Build a [MongoDB](http://www.mongodb.org) docker image and container using Dockerfile.
#### Build Image:
   ```bash
   docker build -t mongodb mongod/.
   ```
Here -t denotes the tags of images and '.' denotes it location Dockerfile

#### RUN container
```bash
docker run -d -p 27017:27017 -v <db-dir>:/data/db --name Mongod mongodb
```
Here -p denotes port mapping between inside container port to our host port, mongodb has running its default port 27017 and -v denotes volume mapping.

## Redis

`Dockerfile` to create a [Docker](https://www.docker.com/) container image for [Redis](http://redis.io/).

Redis is an open source, BSD licensed, advanced key-value cache and store. It is often referred to as a data structure server since keys can contain strings, hashes, lists, sets, sorted sets, bitmaps and hyperloglogs.

Build a [Redis](http://redis.io/) docker image and container using Dockerfile.
#### Build Image:
   ```bash
   docker build -t redis-server redis/.
   ```
Here -t denotes the tags of images and '.' denotes it location Dockerfile

### Usage

#### Run `redis-server`
   ```bash
    docker run -d --name redis -p 6379:6379 redis-server
   ```

#### Run `redis-server` with persistent data directory. (creates `dump.rdb`)
  ```bash
    docker run -d -p 6379:6379 -v <data-dir>:/data --name redis redis-server
  ```
  
##Nodejs

Node.js is a platform built on Chrome's JavaScript runtime for easily building fast, scalable network applications. Node.js uses an event-driven, non-blocking I/O model that makes it lightweight and efficient, perfect for data-intensive real-time applications that run across distributed devices.

Build a [Nodejs](https://nodejs.org/en/) docker image and container using Dockerfile.
#### Build Image:
   ```bash
   docker build -t node nodejs/.
   ```
### Usage

#### Run `redis-server`
   ```bash
    docker run -d --name redis redis-server
   ```

## Citus

Citus is an extension to Postgres that distributes data and queries in a cluster of multiple machines. As an extension Citus supports new PostgreSQL releases. For more information, see the [Citus Data website](https://www.citusdata.com/).

This image provides a single running Citus instance (atop PostgreSQL 12.2), using standard configuration values. It is based on [the official PostgreSQL image][docker-postgres], Just like the standard PostgreSQL image, this image exposes port `5432`. In other words, all containers on the same Docker network should be able to connect on this port, and exposing it externally will permit connections from external clients.

Since Citus is intended for use within a cluster, there are many ways to deploy it. This repository provides configuration to permit two kinds of deployment: local (standalone) or local (with workers).

### Standalone Use

If we just want to run a single Citus instance, it’s pretty easy to get started:

```bash
docker run --name citus_standalone -p 5432:5432 citusdata/citus
```

we should now be able to connect to `127.0.0.1` on port `5432` using e.g. `psql` to run a few commands.

As with the PostgreSQL image, the default `PGDATA` directory will be mounted as a volume, so it will persist between restarts of the container. But while the above command will get you a running Citus instance, it won’t have any workers to exercise distributed query planning. For that, you may wish to try the included [`docker-compose.yml`][compose-config] configuration.

### Docker Compose

The included `docker-compose.yml` file provides an easy way to get started with a Citus cluster, complete with multiple workers. Just copy it to your current directory and run:

```bash
docker-compose -p citus up

# Creating network "citus_default" with the default driver
# Creating citus_worker_1
# Creating citus_master
# Creating citus_config
# Attaching to citus_worker_1, citus_master, citus_config
# worker_1    | The files belonging to this database system will be owned by user "postgres".
# worker_1    | This user must also own the server process.
# ...
```

That’s it! As with the standalone mode, you’ll want to find your `docker-machine ip` if you’re using that technology, otherwise, just connect locally to `5432`. By default, you’ll only have one worker:
```sql
SELECT master_get_active_worker_nodes();

--  master_get_active_worker_nodes
-- --------------------------------
--  (citus_worker_1,5432)
-- (1 row)
