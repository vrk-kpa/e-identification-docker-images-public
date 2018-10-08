FROM nginx:1.11.13

RUN apt-get update && apt-get upgrade -y && apt-get -y install logrotate cron
COPY conf/templates /data00/templates/store
COPY conf/ansible /data00/templates/store/ansible
COPY conf/etc-logrotate.d/nginx /etc/logrotate.d/nginx
RUN chmod 644 /etc/logrotate.d/nginx
RUN rm /etc/cron.daily/passwd /etc/cron.daily/apt /etc/cron.daily/dpkg
