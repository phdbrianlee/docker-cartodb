FROM ubuntu:14.04  
MAINTAINER Alex Zvoleff <azvoleff@conservation.org>

RUN sudo locale-gen en_US.UTF-8
RUN sudo update-locale LANG=en_US.UTF-8 LC_ALL=en_US.UTF-8

ENV SQL_VERSION 1.43.1 #1.24.0
ENV ENVIRONMENT production

# Get the add-apt-repository command
RUN sudo apt-get update && sudo apt-get install -y --no-install-recommends python-software-properties \ 
    && sudo apt-get install -y software-properties-common 
    
RUN sudo add-apt-repository -y ppa:cartodb/nodejs-010
RUN sudo add-apt-repository -y ppa:cartodb/cairo

# Step 10 : 未成功
RUN sudo apt-get update \
      && sudo apt-get install -y --no-install-recommends \
           build-essential checkinstall \
           nodejs \
           zip gdal-bin \ 
           zlib1g-dev libssl-dev libxml2-dev libxslt-dev libreadline6-dev libyaml-dev libffi-dev \
           curl \
           netcat \
           git \
      && sudo apt-get clean \
      && rm -rf /var/lib/apt/lists/*

RUN curl -sSL https://github.com/CartoDB/CartoDB-SQL-API/archive/$SQL_VERSION.tar.gz | tar -v -C / -xz

RUN cd /CartoDB-SQL-API-$SQL_VERSION && npm install \ 
    && mkdir /CartoDB-SQL-API-$SQL_VERSION/logs

RUN cd /CartoDB-SQL-API-$SQL_VERSION/config/environments \
      && cp $ENVIRONMENT.js.example $ENVIRONMENT.js

COPY ./startup.sh /opt/startup.sh

EXPOSE 8080

CMD ["/bin/bash", "/opt/startup.sh"]
