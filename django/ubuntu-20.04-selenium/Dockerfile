#
#                        BUILD TEMPORARY BUILD-SYSTEM
#
# Downloads all the pip with the given ssh key. Allows to use private repos
#  without the key
#
FROM microdisseny/py3-development:ubuntu-20.04-selenium as build-system

ADD docker/config/known_hosts /root/.ssh/known_hosts

ADD ./requirements*.txt /tmp/

# Download python requirements
ARG EXTRA_REQUIREMENTS=devel
ENV EXTRA_REQUIREMENTS $EXTRA_REQUIREMENTS

RUN --mount=type=ssh \
  set -e && \
  pip3 install pip -U; \
  mkdir /pip; \
  if [ -n "$EXTRA_REQUIREMENTS" -a -f "/tmp/requirements-$EXTRA_REQUIREMENTS.txt" ]; then \
    pip3 download -d /pip -r /tmp/requirements.txt -r "/tmp/requirements-$EXTRA_REQUIREMENTS.txt"; \
  else \
    pip3 download -d /pip -r /tmp/requirements.txt; \
  fi;

  #
  #                            BUILD RUNTIME SYSTEM
  #
  # The actuall system that will be run
  #
FROM microdisseny/py3-development:ubuntu-20.04-selenium

  # Copy files
  RUN mkdir -p /app
  RUN mkdir -p /docker
  ADD docker/django/entrypoint.sh /docker/
  ADD docker/django/bash.sh /docker/
  RUN chmod +x /docker/*.sh

  # Install python requirements
  COPY --from=build-system /pip /pip
  RUN pip3 install pip -U
  RUN pip3 install /pip/*
  RUN rm -rf /pip

  # Configure run
  WORKDIR /app
  ENTRYPOINT ["/docker/entrypoint.sh"]
