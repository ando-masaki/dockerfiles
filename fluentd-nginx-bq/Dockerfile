# Dockerfile for fluentd + nginx
#
# Usage:
# sudo docker run -e GCP_PROJECT="YOUR_PROJECT_ID" -p 80:80 -t -i -d kazunori279/fluentd-nginx-bq 

FROM ubuntu:12.04
MAINTAINER ando_masaki-at-cygames.co.jp

# nginx
RUN apt-get install -y nginx
ADD nginx.conf /etc/nginx/nginx.conf

# fluentd
RUN curl -L https://dl.google.com/cloudagents/install-logging-agent.sh | bash

# fluent-plugin-dstat 
RUN apt-get install dstat
RUN /usr/lib/fluent/ruby/bin/fluent-gem install fluent-plugin-dstat --no-ri --no-rdoc -V

# fluent-plugin-bigquery
RUN /usr/lib/fluent/ruby/bin/fluent-gem install fluent-plugin-bigquery --no-ri --no-rdoc -V

# td-agent.conf
ADD td-agent.conf /etc/td-agent/td-agent.conf

# start and open services
EXPOSE 80
ENTRYPOINT /etc/init.d/td-agent restart && /etc/init.d/nginx start && /bin/bash 

