FROM nginx:1.14.0

RUN apt-get update && apt-get upgrade -y && apt-get -y install logrotate cron
COPY conf/templates /data00/templates/store
COPY conf/ansible /data00/templates/store/ansible
COPY conf/etc-logrotate.d/nginx /etc/logrotate.d/nginx
RUN chmod 644 /etc/logrotate.d/nginx
RUN rm /etc/cron.daily/passwd /etc/cron.daily/dpkg /etc/cron.daily/exim4-base /etc/cron.daily/apt-compat
