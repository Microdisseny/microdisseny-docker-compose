#
#                        BUILD TEMPORARY BUILD-SYSTEM
#
# Downloads all the pip with the given ssh key. Allows to use private repos
#  without the key
#
FROM microdisseny/py3-development:ubuntu-16.04 as build-system

# Copy private key
RUN mkdir /root/.ssh/
ADD docker/django/ssh/id_rsa /root/.ssh/
RUN chmod 600 /root/.ssh/id_rsa

ADD docker/config/known_hosts /root/.ssh/known_hosts

ADD ./requirements*.txt /tmp/

# Download python requirements
ARG EXTRA_REQUIREMENTS=devel
ENV EXTRA_REQUIREMENTS $EXTRA_REQUIREMENTS

RUN \
  pip3 install pip==20.2.2; \
  mkdir /pip; \
  pip3 download -d /pip uwsgi; \
  if [ -n "$EXTRA_REQUIREMENTS" -a -f "/tmp/requirements-$EXTRA_REQUIREMENTS.txt" ]; then \
    pip3 download -d /pip -r /tmp/requirements.txt -r "/tmp/requirements-$EXTRA_REQUIREMENTS.txt"; \
  else \
    pip3 download -d /pip -r /tmp/requirements.txt; \
  fi; \
  rm /root/.ssh/id_rsa


#
#                            BUILD RUNTIME SYSTEM
#
# The actual system that will be run
#
FROM microdisseny/py3-development:ubuntu-16.04

# Copy files
RUN mkdir -p /app
RUN mkdir -p /docker
ADD docker/django/entrypoint.sh /docker/
ADD docker/django/bash.sh /docker/
RUN chmod +x /docker/*.sh

# Install python requirements
COPY --from=build-system /pip /pip
RUN pip3 install pip==20.2.2 && \
  pip3 install /pip/* && \
  rm -rf /pip

# Configure run
WORKDIR /app
ENTRYPOINT ["/docker/entrypoint.sh"]
