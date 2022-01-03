# The base image is one of the offical one
FROM node:latest

LABEL Axel Vallon <axel.vallon@heig-vd.ch>
LABEL Robin Gaudin <robin.gaudin@heig-vd.ch>
LABEL Lev Pozniakoff <lev.pozniakoff@heig-vd.ch>

# Install the required tools to run our webapp and some utils
RUN apt-get update && apt-get -y install wget curl vim && apt-get clean && npm install -g bower

# We copy the application and make sure the dependencies are installed before
# other operations. Doing so will reduce the time required to build this image
# as downloading NPM dependencies can be quite long.
COPY app /backend/app
RUN cd /backend/app && npm install && bower install --allow-root

# TODO: [S6] Install

# Install serf (for decentralized cluster membership: https://www.serf.io/)
RUN mkdir /opt/bin \
    && curl -sSLo /tmp/serf.gz https://releases.hashicorp.com/serf/0.8.2/serf_0.8.2_linux_amd64.zip \
    && gunzip -c /tmp/serf.gz > /opt/bin/serf \
    && chmod 755 /opt/bin/serf \
    && rm -f /tmp/serf.gz

# Copy the starting script and make it executable
COPY scripts /scripts/
RUN chmod +x /scripts/*.sh

# Add Serf S6 setup
COPY services/serf /serf/
RUN chmod +x /serf/run

# Clean up
RUN apt-get clean && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

# Expose the ports for Serf
EXPOSE 7946 7373

# Expose the web application port
EXPOSE 3000

# Define an environment variable for the role of the container
ENV ROLE backend

# TODO: [S6] Replace the following instruction
# Command executed when the container starts
CMD [ "bash", "/serf/run" ]
