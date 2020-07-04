# Install and Configure appICE components

This repository contains **Docker-Compose file** of [appICE](https://www.appice.io/) components such as [CITUS](https://www.citusdata.com/), [MongoDB](http://www.mongodb.org), [REDIS](https://redis.io/), [Nodejs](https://nodejs.org/en/), [Nginx](https://www.nginx.com/) and [Supervisor](http://supervisord.org/).

### Installation

  Install [Docker](https://docs.docker.com/engine/install/) and then install [docker-compose](https://docs.docker.com/compose/install/).
  
## Citus

Citus is an extension to Postgres that distributes data and queries in a cluster of multiple machines. As an extension Citus supports new PostgreSQL releases. For more information, see the [Citus Data website](https://www.citusdata.com/).

This image provides a single running Citus instance (atop PostgreSQL 12.2), using standard configuration values. It is based on [the official PostgreSQL image][docker-postgres], Just like the standard PostgreSQL image, this image exposes port `5432`. In other words, all containers on the same Docker network should be able to connect on this port, and exposing it externally will permit connections from external clients (`psql`, adapters, applications).
