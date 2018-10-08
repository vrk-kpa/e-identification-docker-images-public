FROM nginx:1.11.13

RUN apt-get update && apt-get upgrade -y
COPY conf/templates /data00/templates/store
COPY conf/ansible /data00/templates/store/ansible
RUN mkdir -p /nginx_cache /nginx_long_cache
