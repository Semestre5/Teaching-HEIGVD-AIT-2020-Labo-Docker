# Base image is the Official HAProxy
FROM haproxy:2.2

LABEL Axel Vallon <axel.vallon@heig-vd.ch>
LABEL Robin Gaudin <robin.gaudin@heig-vd.ch>
LABEL Lev Pozniakoff <lev.pozniakoff@heig-vd.ch>

# Install some tools
# TODO: [HB] Update to install required tool to install NodeJS
RUN apt-get update && apt-get -y install wget curl vim iputils-ping rsyslog

# TODO: [S6] Install

# Install serf (for decentralized cluster membership: https://www.serf.io/)
RUN mkdir /opt/bin \
    && curl -sSLo /tmp/serf.gz https://releases.hashicorp.com/serf/0.8.2/serf_0.8.2_linux_amd64.zip \
    && gunzip -c /tmp/serf.gz > /opt/bin/serf \
    && chmod 755 /opt/bin/serf \
    && rm -f /tmp/serf.gz

# TODO: [HB] Install NodeJS

# TODO: [HB] Install Handlebars

# Copy all the scripts and make them executable
COPY scripts/ /scripts/
RUN chmod +x /scripts/*.sh

# Add Serf S6 setup
COPY services/serf /serf/
RUN chmod +x /serf/run

# TODO: [Serf] Copy events handler scripts

# Copy the haproxy and syslog config
COPY config/haproxy.cfg /usr/local/etc/haproxy/haproxy.cfg
COPY config/rsyslogd.cfg /etc/rsyslog.d/49-haproxy.conf

# TODO: [HB] Copy the haproxy configuration template

# Clean up
RUN apt-get clean && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

# TODO: [CFG] Create the nodes folder

# Expose the ports for Serf
EXPOSE 7946 7373

# Expose the HA proxy ports
EXPOSE 80 1936

# Define an environment variable
ENV ROLE balancer

# TODO: [S6] Replace the following instruction
# Command executed when the container starts
CMD [ "bash", "/serf/run" ]
