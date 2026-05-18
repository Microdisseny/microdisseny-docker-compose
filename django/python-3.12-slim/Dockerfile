#
#                        BUILD TEMPORARY BUILD-SYSTEM
#
FROM microdisseny/py3-development:python-3.12-slim as build-system
ADD docker/config/known_hosts /root/.ssh/known_hosts
ADD ./requirements*.txt /tmp/
ARG EXTRA_REQUIREMENTS=devel
ARG PIP_PACKAGES
ARG APT_PACKAGES_BUILD
RUN \
  set -e; \
  if [ -n "$APT_PACKAGES_BUILD" ]; then \
    apt-get update && \
    apt -y install $APT_PACKAGES_BUILD; \
  fi; \
  if [ -n "$PIP_PACKAGES" ]; then \
    pip install $PIP_PACKAGES; \
  fi;
RUN --mount=type=ssh \
  set -e; \
  pip install -U pip; \
  pip install numpy; \
  mkdir /pip; \
  if [ -n "$EXTRA_REQUIREMENTS" -a -f "/tmp/requirements-$EXTRA_REQUIREMENTS.txt" ]; then \
    pip download -d /pip -r /tmp/requirements.txt -r "/tmp/requirements-$EXTRA_REQUIREMENTS.txt"; \
  else \
    pip download -d /pip -r /tmp/requirements.txt; \
  fi;

#
#                            BUILD RUNTIME SYSTEM
#
FROM microdisseny/py3-development:python-3.12-slim
RUN mkdir -p /app
RUN mkdir -p /docker
ADD docker/django/entrypoint.sh /docker/
ADD docker/django/bash.sh /docker/
RUN chmod +x /docker/*.sh
COPY --from=build-system /pip /pip
ARG PIP_PACKAGES
ARG APT_PACKAGES
RUN \
  set -e; \
  if [ -n "$APT_PACKAGES" ]; then \
    apt-get update && \
    apt -y install $APT_PACKAGES; \
  fi; \
  if [ -n "$PIP_PACKAGES" ]; then \
    pip install $PIP_PACKAGES; \
  fi;
RUN pip install -U pip
RUN pip install /pip/*
RUN rm -rf /pip
WORKDIR /app
ENTRYPOINT ["/docker/entrypoint.sh"]
