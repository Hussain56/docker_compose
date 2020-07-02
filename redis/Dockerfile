FROM ubuntu:18.04
LABEL maintainer="hussain Sanjamala <hussain.sanjamala@semusi.com>"

RUN \
    apt-get update && \
    apt-get install redis-server -y && \
    systemctl enable redis.service && \
    apt-get install vim -y && \
    apt-get install git -y 

EXPOSE 6379

RUN mkdir /data && chown redis:redis /data
VOLUME /data

CMD ["redis-server"]
