FROM unicon/shibboleth-sp:3.0.4

COPY conf/templates/shibboleth2.xml /etc/shibboleth/shibboleth2.xml
COPY conf/templates/qa.oidc.tunnistus.fi.xml /etc/shibboleth/qa.oidc.tunnistus.fi.xml
COPY conf/templates/prod.oidc.tunnistus.fi.xml /etc/shibboleth/prod.oidc.tunnistus.fi.xml
COPY conf/templates/attribute-map.xml /etc/shibboleth/attribute-map.xml
COPY conf/templates/attribute-policy.xml /etc/shibboleth/attribute-policy.xml
COPY conf/templates/oidc.html /var/www/html/oidc.html
COPY conf/templates/tyyli.css /var/www/html/tyyli.css
COPY conf/templates/ssl.conf /etc/httpd/conf.d/ssl.conf

RUN /bin/bash -c 'openssl req -x509 -newkey rsa:3072 -nodes -keyout /etc/pki/tls/private/tls-key.pem -out /etc/pki/tls/certs/tls-cert.pem -days 1800 -subj /CN=oidctest.tunnistus.fi'
