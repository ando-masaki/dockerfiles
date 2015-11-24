# Dockerfile for fluentd + nginx
#
# Usage:
# sudo docker run -e GCP_PROJECT="YOUR_PROJECT_ID" -p 80:80 -t -i -d andomasaki/fluentd-nginx-bq 

FROM ubuntu:14.04
MAINTAINER ando_masaki

# environment
ENV DEBIAN_FRONTEND noninteractive
RUN echo "deb http://archive.ubuntu.com/ubuntu precise main universe" > /etc/apt/sources.list

# update, curl, sudo
RUN apt-get -y update && apt-get -y upgrade
RUN apt-get -y install curl 
RUN apt-get -y install sudo

# nginx
RUN apt-get -y install nginx
ADD nginx.conf /etc/nginx/nginx.conf
RUN curl -L http://nginx.org/LICENSE > /nginx-license.txt

# fluentd
RUN curl -L https://dl.google.com/cloudagents/install-logging-agent.sh | bash
ADD td-agent.conf /etc/td-agent/td-agent.conf
RUN curl -L https://raw.githubusercontent.com/fluent/fluentd/master/COPYING > /fluentd-license.txt

# fluent-plugin-dstat 
RUN apt-get -y install dstat
RUN /usr/lib/fluent/ruby/bin/fluent-gem install fluent-plugin-dstat --no-ri --no-rdoc -V

# fluent-plugin-bigquery
RUN /opt/google-fluentd/embedded/bin/fluent-gem install fluent-plugin-bigquery --no-ri --no-rdoc -V
RUN curl -L https://raw.githubusercontent.com/kaizenplatform/fluent-plugin-bigquery/master/LICENSE.txt > fluent-plugin-bigquery-license.txt

# start fluentd and nginx
EXPOSE 80
ENTRYPOINT /etc/init.d/td-agent restart && /etc/init.d/nginx start && /bin/bash

