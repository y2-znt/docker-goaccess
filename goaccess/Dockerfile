FROM debian:bullseye

RUN apt-get update && \
    apt-get install -y goaccess apache2 apache2-utils cron curl

# Create reports directory
RUN mkdir -p /var/www/html/reports && \
    chown www-data:www-data /var/www/html/reports

COPY apache.conf /etc/apache2/sites-available/000-default.conf

# Create log directory for nginx logs
RUN mkdir -p /var/log/nginx && \
    touch /var/log/nginx/access.log && \
    chown -R www-data:www-data /var/log/nginx

COPY crontab /etc/cron.d/goaccess
RUN chmod 0644 /etc/cron.d/goaccess && crontab /etc/cron.d/goaccess

RUN a2enmod auth_basic

# Create startup script
RUN echo '#!/bin/bash\n\
# Generate htpasswd file\n\
htpasswd -cb /etc/apache2/.htpasswd ${GOACCESS_USER} ${GOACCESS_PASSWORD}\n\
\n\
goaccess /var/log/nginx/access.log -o /var/www/html/reports/index.html --log-format=COMBINED --real-time-html &\n\
service cron start\n\
apachectl -D FOREGROUND' > /start.sh && \
    chmod +x /start.sh

CMD ["/start.sh"]
