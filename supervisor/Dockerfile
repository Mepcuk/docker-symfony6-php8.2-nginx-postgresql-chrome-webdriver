FROM php:8.2.0-fpm-alpine

RUN apk add --no-cache make nano g++ python3 supervisor
RUN apk add --update linux-headers


RUN mkdir /var/log/supervisor
#ECHO "Test" > /etc/supervisord.conf
#RUN touch /etc/supervisor/conf.d/supervisord.conf

#RUN echo user=root >>  /etc/supervisord.conf
COPY supervisord3.conf /etc/supervisord.conf
CMD ["/usr/bin/supervisord", "-c", "/etc/supervisord.conf"]
#CMD ["/usr/bin/supervisord","-n"]
#CMD ["/usr/bin/supervisord"]
#CMD ["/usr/bin/supervisord", "-n", "-c", "/var/www/docker-symfony6-php8.2-nginx-postgresql-chrome-webdriver/supervisor/supervisord3.conf"]

EXPOSE 9001

LABEL org.opencontainers.image.authors="maksims.alekseicev@gmail.com"
LABEL org.opencontainers.image.documentation="https://github.com/opencontainers/image-spec/blob/main/annotations.md"