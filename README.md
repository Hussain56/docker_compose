# Install and Configure appICE components

This repository contains **Docker-Compose file** of [appICE](https://www.appice.io/) components such as [CITUS](https://www.citusdata.com/), [MongoDB](http://www.mongodb.org), [REDIS](https://redis.io/), [Nodejs](https://nodejs.org/en/), [Nginx](https://www.nginx.com/) and [Supervisor](http://supervisord.org/).

### Installation

  Install [Docker](https://docs.docker.com/engine/install/) and then install [docker-compose](https://docs.docker.com/compose/install/).
  
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
