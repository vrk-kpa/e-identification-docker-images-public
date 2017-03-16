FROM nginx


RUN apt-get update && apt-get install --only-upgrade libssl1.0.0 openssl
COPY conf/templates /data00/templates/store
COPY conf/ansible /data00/templates/store/ansible
RUN mkdir -p /nginx_cache /nginx_long_cache
