#
#                        BUILD TEMPORARY BUILD-SYSTEM
#
# Downloads all the pip with the given ssh key. Allows to use private repos
#  without the key
#
FROM microdisseny/py3-development:ubuntu-24.04 as build-system

ADD docker/config/known_hosts /root/.ssh/known_hosts

ADD ./requirements*.txt /tmp/

# Download python requirements
ARG EXTRA_REQUIREMENTS=devel
ARG PIP_PACKAGES
ARG APT_PACKAGES

RUN \
  set -e; \
  if [ -n "$APT_PACKAGES_BUILD" ]; then \
    apt-get update && \
    apt -y install $APT_PACKAGES_BUILD; \
  fi; \

  if [ -n "$PIP_PACKAGES" ]; then \
    pip3 install $PIP_PACKAGES; \
  fi;

RUN --mount=type=ssh \
  set -e; \
  pip3 install -U pip; \
  pip3 install numpy; \
  mkdir /pip; \
  if [ -n "$EXTRA_REQUIREMENTS" -a -f "/tmp/requirements-$EXTRA_REQUIREMENTS.txt" ]; then \
    pip3 download -d /pip -r /tmp/requirements.txt -r "/tmp/requirements-$EXTRA_REQUIREMENTS.txt"; \
  else \
    pip3 download -d /pip -r /tmp/requirements.txt; \
  fi;


#
#                            BUILD RUNTIME SYSTEM
#
# The actual system that will be run
#
FROM microdisseny/py3-development:ubuntu-24.04

# Copy files
RUN mkdir -p /app
RUN mkdir -p /docker
ADD docker/django/entrypoint.sh /docker/
ADD docker/django/bash.sh /docker/
RUN chmod +x /docker/*.sh

# Install python requirements
COPY --from=build-system /pip /pip
ARG PIP_PACKAGES

RUN \
  set -e; \
  if [ -n "$APT_PACKAGES" ]; then \
    apt-get update && \
    apt -y install $APT_PACKAGES; \
  fi; \
  if [ -n "$PIP_PACKAGES" ]; then \
    pip3 install $PIP_PACKAGES; \
  fi;
RUN pip3 install -U pip
RUN pip3 install /pip/*
RUN rm -rf /pip

# Configure run
WORKDIR /app
ENTRYPOINT ["/docker/entrypoint.sh"]
