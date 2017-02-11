#
# Windshaft container
#
FROM phusion/baseimage:0.9.18
MAINTAINER Volkan Unsal <spocksplanet@gmail.com>

ENV WINDSHAFT_VERSION 2.87.5

# Configuring locales
ENV DEBIAN_FRONTEND noninteractive

RUN apt-get update &&\
    apt-get install -y -q \
    build-essential \
    autoconf \
    automake \
    checkinstall \
    git-core \
    git \
    curl \
    bison \
    ca-certificates \
    gdal-bin \
    gnupg \
    imagemagick \
    libc6-dev \
    libcairo2-dev \
    libcurl4-gnutls-dev \
    libffi-dev \
    libgdal1-dev \
    libgeos-c1 \
    libgeos-dev \
    libgif-dev \
    libgdbm-dev \
    libgmp-dev \
    libicu-dev \
    libjpeg8-dev \
    libjson0 \
    libjson0-dev \
    libmapnik-dev \
    libpango1.0-dev \
    libpq5 \
    libpq-dev \
    libproj-dev \
    libreadline6 \
    libreadline6-dev \
    libssl-dev \
    libsqlite3-dev \
    libtool \
    libyaml-dev \
    libxml2-dev \
    libxslt-dev \
    mapnik-utils \
    ncurses-dev \
    openssl \
    pkg-config \
    proj-bin \
    proj-data \
    python2.7-dev \
    python-argparse \
    python-chardet \
    python-gdal \
    python-mapnik \
    python-simplejson \
    python-setuptools \
    sqlite3 \
    zlib1g \
    zlib1g-dev \
  --no-install-recommends

# Install NodeJS
RUN curl https://nodejs.org/download/release/v0.10.41/node-v0.10.41-linux-x64.tar.gz| tar -zxf - --strip-components=1 -C /usr

# Install NPM
# RUN npm install -g npm@1.4.29

# Install Windshaft
# RUN git clone git://github.com/CartoDB/Windshaft-cartodb.git
RUN curl -sSL https://github.com/CartoDB/Windshaft-cartodb/archive/$WINDSHAFT_VERSION.tar.gz | tar -v -C / -xz
RUN cd /Windshaft-cartodb-$WINDSHAFT_VERSION && npm install && mkdir /Windshaft-cartodb-$WINDSHAFT_VERSION/logs

# Assume the following linked services
# - postgres:5432
# - redis:6379
# - varnish:6082    // <- port to telnet interface
# - varnish:6081    // <- port to HTTP interface
# - statsd:8125
ADD ./config/WS-dev.js \
      /Windshaft-cartodb-$WINDSHAFT_VERSION/config/environments/development.js

EXPOSE 8181

# runit entries. These are scripts to run as daemons after bootup.
RUN mkdir /etc/service/node
ADD ./daemons/node.sh /etc/service/node/run

CMD ["/sbin/my_init"]

# Clean up APT when done.
RUN apt-get clean && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*
