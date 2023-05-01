FROM php:8.2.0-fpm-alpine

RUN apk add --no-cache curl git build-base zlib-dev oniguruma-dev autoconf bash zip libzip-dev libpq-dev make nano g++ python3 supervisor git
RUN apk add --update linux-headers

ARG INSTALL_XDEBUG=false
RUN if [ ${INSTALL_XDEBUG} = true ]; \
    then \
      pecl install xdebug && docker-php-ext-enable xdebug; \
    fi;

COPY ./docker-symfony6-php8.2-nginx-postgresql-chrome-webdriver/php-fpm/xdebug.ini /usr/local/etc/php/conf.d/xdebug.ini

ADD https://github.com/mlocati/docker-php-extension-installer/releases/latest/download/install-php-extensions /usr/local/bin/

# Install PHP Extensions
RUN chmod +x /usr/local/bin/install-php-extensions && sync && \
    install-php-extensions gd zip intl imap xsl pgsql opcache bcmath mysqli pdo_pgsql redis amqp
RUN docker-php-ext-configure zip && docker-php-ext-install zip
#RUN apk add --no-cache libpq-dev && docker-php-ext-install pdo_pgsql

# Configure Extension
RUN docker-php-ext-configure \
    opcache --enable-opcache

#Configure non-root user
ARG PUID=1000
ARG PGID=1000

RUN apk --no-cache add shadow && \
    groupmod -o -g ${PGID} www-data && \
    usermod -o -u ${PUID} -g www-data www-data

COPY ./ /var/www
WORKDIR /var/www

COPY --from=composer:latest /usr/bin/composer /usr/bin/composer
## Installing composer
#RUN curl -sS https://getcomposer.org/installer -o composer-setup.php
#RUN php composer-setup.php --install-dir=/usr/local/bin --filename=composer
#RUN rm -rf composer-setup.php

# Chromium and ChromeDriver
ENV PANTHER_NO_SANDBOX 1
# Not mandatory, but recommended
ENV PANTHER_CHROME_ARGUMENTS='--disable-dev-shm-usage'
RUN apk add --no-cache chromium chromium-chromedriver



RUN mkdir /var/log/supervisor
#ECHO "Test" > /etc/supervisord.conf
#RUN touch /etc/supervisor/conf.d/supervisord.conf

#RUN echo user=root >>  /etc/supervisord.conf
COPY ./docker-symfony6-php8.2-nginx-postgresql-chrome-webdriver/supervisor/supervisord3.conf /etc/supervisord.conf
#CMD ["/usr/bin/supervisord", "-c", "/etc/supervisord.conf"]
#CMD ["/usr/bin/supervisord","-n"]
#CMD ["/usr/bin/supervisord"]
#CMD ["/usr/bin/supervisord", "-n", "-c", "/var/www/docker-symfony6-php8.2-nginx-postgresql-chrome-webdriver/supervisor/supervisord3.conf"]
CMD php-fpm

EXPOSE 9000

LABEL org.opencontainers.image.authors="maksims.alekseicev@gmail.com"
LABEL org.opencontainers.image.documentation="https://github.com/opencontainers/image-spec/blob/main/annotations.md"