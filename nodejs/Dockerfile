FROM ubuntu:18.04
LABEL maintainer="hussain Sanjamala <hussain.sanjamala@semusi.com>"

VOLUME ["/var/www"]

RUN apt-get update -y && \
   apt-get install -y --no-install-recommends apt-utils && \
   apt install curl -y && \
   apt-get install gnupg -y && \
   apt-get install -y gnupg2 -y

RUN curl -s https://deb.nodesource.com/gpgkey/nodesource.gpg.key | apt-key add - && \
  echo "deb https://deb.nodesource.com/node_10.x xenial main" | tee /etc/apt/sources.list.d/node.list

RUN apt-get update -y && \
  apt-get upgrade -y && \
  apt-get install -y nodejs && \
  apt-get clean && \
  rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

RUN npm install -g yarn npx --force

WORKDIR /var/www/