FROM microdisseny/py3-development:ubuntu-20.04

MAINTAINER MICRODISSENY GISCUBE SL <tech@microdisseny.com>

RUN apt-get update && \
  apt-get upgrade -y && \
  apt-get install -y --no-install-recommends \
    postgresql && \
  apt-get clean && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

RUN mkdir -p /app

WORKDIR /app

ENTRYPOINT []
