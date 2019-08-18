FROM php:7.2-apache

RUN echo "ServerName localhost" >> /etc/apache2/apache2.conf

RUN apt-get update \
    && apt-get install -y --no-install-recommends locales apt-utils git libicu-dev g++ libpng-dev;

RUN echo "en_US.UTF-8 UTF-8" > /etc/locale.gen && \
    echo "fr_FR.UTF-8 UTF-8" >> /etc/locale.gen && \
    locale-gen

COPY php.ini /usr/local/etc/php/php.ini
COPY test /usr/local/etc/php/test

RUN curl -sSk https://getcomposer.org/installer | php -- --disable-tls && \
   mv composer.phar /usr/local/bin/composer

RUN docker-php-ext-configure intl
RUN docker-php-ext-install pdo pdo_mysql gd opcache intl zip calendar
RUN pecl install apcu-5.1.5 && docker-php-ext-enable apcu

RUN addgroup --system gponty --gid 1000 && adduser --system gponty --uid 1000 --ingroup gponty

WORKDIR /var/www/backend

COPY composer.json /var/www/backend/composer.json

RUN composer install

EXPOSE 80

EXPOSE 8000