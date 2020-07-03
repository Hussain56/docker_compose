FROM ubuntu:18.04
LABEL maintainer="NGINX Docker Maintainers <hussain.sanjamala@semusi.com>"

RUN set -x \
   && apt-get update \
   && apt-get install -y apt-utils \
   && apt-get install --no-install-recommends --no-install-suggests -y gnupg1 ca-certificates \
   && \
    NGINX_GPGKEY=573BFD6B3D8FBC641079A6ABABF5BD827BD9BF62; \
    found=''; \
    for server in \
        ha.pool.sks-keyservers.net \
        hkp://keyserver.ubuntu.com:80 \
        hkp://p80.pool.sks-keyservers.net:80 \
        pgp.mit.edu \
    ; do \
        echo "Fetching GPG key $NGINX_GPGKEY from $server"; \
        apt-key adv --keyserver "$server" --keyserver-options timeout=10 --recv-keys "$NGINX_GPGKEY" && found=yes && break; \
    done; \
    test -z "$found" && echo >&2 "error: failed to fetch GPG key $NGINX_GPGKEY" && exit 1; \
    apt-get remove --purge --auto-remove -y gnupg1 && rm -rf /var/lib/apt/lists/* \
   && apt-get update \
   && apt-get install nginx -y \
   && apt-get install curl -y \
   && apt-get install gettext-base -y \
   && ln -sf /dev/stdout /var/log/nginx/access.log \
   && ln -sf /dev/stderr /var/log/nginx/error.log \
   && mkdir /docker-entrypoint.d

EXPOSE 80

STOPSIGNAL SIGTERM

CMD ["nginx", "-g", "daemon off;"]
