FROM php:7.4-apache

ARG TZ
ENV COMPOSER_ALLOW_SUPERUSER 1
ENV COMPOSER_HOME /composer

RUN set -eux \
  && ln -sf /usr/share/zoneinfo/${TZ} /etc/localtime \
  && apt-get update \
  && apt-get install -y \
    zip \
    unzip \
    vim \
    libpq-dev \
  && docker-php-ext-install \
    pdo_mysql

RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/bin --filename=composer \
  && composer config -g repos.packagist composer https://packagist.jp

# for SSL
RUN openssl genrsa -out /etc/apache2/server.key 2048 2>/dev/null \
  && openssl req -new -key /etc/apache2/server.key -out /etc/apache2/server.csr -subj "/C=JP/" \
  && openssl x509 -in /etc/apache2/server.csr -days 365000 -req -signkey /etc/apache2/server.key > /etc/apache2/server.crt 2>/dev/null \
  && rm /etc/apache2/server.csr
COPY ./apache2/sites-available /etc/apache2/sites-available
RUN a2enmod ssl && a2ensite default-ssl
