FROM ubuntu:18.04

RUN \
  apt-get update && apt-get install -y wget && \
  apt-get install -y curl && \
  apt-get install -y vim && \
  apt-get install -y make && \
  apt-get install -y build-essential && \
  apt-get install -y make-guile && \
  cd /tmp && \
  wget http://download.redis.io/redis-stable.tar.gz && \
  tar xvzf redis-stable.tar.gz && \
  cd redis-stable && \
  make && \
  make install && \
  cp -f src/redis-sentinel /usr/local/bin && \
  mkdir -p /etc/redis && \
  cp -f *.conf /etc/redis && \
  rm -rf /tmp/redis-stable* && \
  sed -i '/bind/s/127.0.0.1 ::1/0.0.0.0 ::1/' /etc/redis/redis.conf && \
  sed -i '/protected-mode/s/yes/no/' /etc/redis/redis.conf && \
  sed -i '/supervised/s/no/systemd/' /etc/redis/redis.conf && \
  sed -i 's/# maxclients 10000/maxclients 20000/' /etc/redis/redis.conf


# Define mountable directories.
VOLUME ["/data"]

# Define working directory.
WORKDIR /data

# Define default command.
CMD ["redis-server", "/etc/redis/redis.conf"]

# Expose ports.
EXPOSE 6379
