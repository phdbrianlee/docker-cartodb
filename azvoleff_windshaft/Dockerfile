FROM ubuntu:14.04
MAINTAINER Alex Zvoleff <azvoleff@conservation.org>

ENV WINDSHAFT_VERSION 2.87.5
ENV ENVIRONMENT production

# Get the add-apt-repository command
RUN sudo apt-get update && sudo apt-get install -y --no-install-recommends python-software-properties \ 
    && sudo apt-get install -y software-properties-common 
    
RUN sudo add-apt-repository -y ppa:cartodb/nodejs-010
RUN sudo add-apt-repository -y ppa:cartodb/cairo

RUN sudo apt-get update \
      && sudo apt-get install -y --no-install-recommends \
           build-essential checkinstall \
           pkg-config libcairo2-dev libjpeg8-dev libgif-dev \
           nodejs \
           zlib1g-dev libssl-dev libxml2-dev libxslt-dev libreadline6-dev libyaml-dev libffi-dev \
           curl \
           netcat \
           git \
      && sudo apt-get clean \
      && rm -rf /var/lib/apt/lists/*

RUN curl -sSL https://github.com/CartoDB/Windshaft-cartodb/archive/$WINDSHAFT_VERSION.tar.gz | tar -v -C / -xz

RUN cd /Windshaft-cartodb-$WINDSHAFT_VERSION && sudo npm install

RUN mkdir /Windshaft-cartodb-$WINDSHAFT_VERSION/logs

RUN cd /Windshaft-cartodb-$WINDSHAFT_VERSION/config/environments \
      && cp $ENVIRONMENT.js.example $ENVIRONMENT.js

COPY ./startup.sh /opt/startup.sh

EXPOSE 8181

CMD ["/bin/bash", "/opt/startup.sh"]
