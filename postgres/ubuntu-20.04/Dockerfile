FROM microdisseny/py3-development:ubuntu-20.04

MAINTAINER MICRODISSENY GISCUBE SL <tech@microdisseny.com>

RUN apt-get update && \
  apt-get upgrade -y && \
  apt-get install -y --no-install-recommends \
    postgresql-server-dev-12 \
    postgresql-client-12 \
    postgresql-12-postgis-3 postgresql-12-postgis-scripts \
    postgresql postgresql-contrib && \
  apt-get clean && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

RUN mkdir -p /app

WORKDIR /app

ENTRYPOINT []
