FROM nginx

RUN apt-get update && apt-get install --only-upgrade libssl1.0.0 openssl
COPY conf/ansible /data00/templates/store/ansible

