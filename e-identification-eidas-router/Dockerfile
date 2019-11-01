FROM httpd:2.4.39

RUN apt-get update && apt-get upgrade -y
COPY conf/templates /data00/templates/store
COPY conf/ansible /data00/templates/store/ansible
