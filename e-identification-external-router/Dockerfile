FROM nginx

RUN apt-get update && apt-get install --only-upgrade libssl1.0.0 openssl && apt-get -y install logrotate cron
COPY conf/ansible /data00/templates/store/ansible
COPY conf/etc-logrotate.d/nginx /etc/logrotate.d/nginx
RUN chmod 644 /etc/logrotate.d/nginx
RUN rm /etc/cron.daily/passwd /etc/cron.daily/apt /etc/cron.daily/dpkg
