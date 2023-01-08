FROM php:8.2.0-fpm-alpine

RUN apk add --no-cache curl git build-base zlib-dev oniguruma-dev autoconf bash

RUN apk add --update linux-headers
RUN pecl install xdebug && docker-php-ext-enable xdebug
COPY ./docker-symfony6-php8.2-nginx-postgresql-chrome-webdriver/php-fpm/xdebug.ini /usr/local/etc/php/conf.d/xdebug.ini

ARG PUID=1000
ARG PGID=1000

RUN apk --no-cache add shadow && \
    groupmod -o -g ${PGID} www-data && \
    usermod -o -u ${PUID} -g www-data www-data

COPY ./ /var/www
WORKDIR /var/www

COPY --from=composer:latest /usr/bin/composer /usr/bin/composer

# Chromium and ChromeDriver
ENV PANTHER_NO_SANDBOX 1
# Not mandatory, but recommended
ENV PANTHER_CHROME_ARGUMENTS='--disable-dev-shm-usage'
RUN apk add --no-cache chromium chromium-chromedriver


CMD php-fpm

EXPOSE 9000

LABEL org.opencontainers.image.authors="maksims.alekseicev@gmail.com"
LABEL org.opencontainers.image.documentation="https://github.com/opencontainers/image-spec/blob/main/annotations.md"