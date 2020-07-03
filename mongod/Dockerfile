FROM ubuntu:18.04
LABEL maintainer="hussain Sanjamala <hussain.sanjamala@semusi.com>"

RUN groupadd -r mongodb && useradd -r -g mongodb mongodb

RUN set -eux; \
    apt-get update && \
    apt-get install -y apt-utils && \
    apt-get install --no-install-recommends --no-install-suggests -y gnupg1 ca-certificates && \
    apt-get install -y --no-install-recommends \
        ca-certificates \
        jq \
        numactl
RUN \
    apt-get update -y && \
    apt-get install -y wget && apt-get install -y curl && \
    apt-get install -y --no-install-recommends apt-utils && \
    apt-get install gnupg -y && \
    apt-get install -y gnupg2 -y && \
    wget -qO - https://www.mongodb.org/static/pgp/server-3.2.asc | apt-key add - && \
    echo "deb http://repo.mongodb.org/apt/ubuntu xenial/mongodb-org/3.2 multiverse" | tee /etc/apt/sources.list.d/mongodb-org-3.2.list

RUN \
    apt-get update -y && \
    apt-get install -y mongodb-org=3.2.22 mongodb-org-server=3.2.22 mongodb-org-shell=3.2.22 mongodb-org-mongos=3.2.22 mongodb-org-tools=3.2.22 && \
    apt-get install -y git && \
    apt-get install -y vim && \
    sed -i '/bindIp/s/127.0.0.1/0.0.0.0/' /etc/mongod.conf && \
    sed -i 's@/var/lib/mongodb@/data/db@g' /etc/mongod.conf && \
    rm -rf /var/lib/apt/lists/*

# pin a specific version
RUN \
    echo "mongodb-org hold" | dpkg --set-selections && \
    echo "mongodb-org-server hold" | dpkg --set-selections && \
    echo "mongodb-org-shell hold" | dpkg --set-selections && \
    echo "mongodb-org-mongos hold" | dpkg --set-selections && \
    echo "mongodb-org-tools hold" | dpkg --set-selections

# created data directory
RUN \
    mkdir -p /data/db && \
    chown -R mongodb:mongodb /data/db

VOLUME ["/data/db"]

WORKDIR /data

EXPOSE 27017
CMD ["mongod"]
