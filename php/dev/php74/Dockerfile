FROM phpdockerio/php:7.4-fpm

LABEL org.opencontainers.image.source="https://github.com/bitrixdock/bitrixdock"

RUN apt-get update \
    && apt-get -y --no-install-recommends install \
    php7.4-gd \
    php7.4-imagick \
    php7.4-intl \
    php7.4-interbase \
    php7.4-mbstring \
    php7.4-mcrypt \
    php7.4-memcache \
    php7.4-memcached \
    php7.4-mysql \
    php7.4-opcache \
    php7.4-soap \
    php7.4-xdebug \
    php7.4-zip \
    && apt-get clean; rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/* /usr/share/doc/*

# DO NOT install php7.4-xdebug package for site running in production! It will slow it down significantly.

COPY ./php.ini /etc/php/7.4/fpm/conf.d/90-php.ini
COPY ./php.ini /etc/php/7.4/cli/conf.d/90-php.ini

RUN usermod -u 1000 www-data

WORKDIR "/var/www/bitrix"

# needle for vscode-server
RUN chown www-data:www-data /var/www

# install phpcs phpcbf php-cs-fixer phpmd
RUN cd /usr/bin && \
    curl -L https://squizlabs.github.io/PHP_CodeSniffer/phpcs.phar -o phpcs && \
    chmod +x /usr/bin/phpcs && \
    curl -L https://squizlabs.github.io/PHP_CodeSniffer/phpcbf.phar -o phpcbf && \
    chmod +x /usr/bin/phpcbf && \
    curl -L https://cs.symfony.com/download/php-cs-fixer-v3.phar -o php-cs-fixer && \
    chmod +x /usr/bin/php-cs-fixer && \
    curl -L https://phpmd.org/static/latest/phpmd.phar -o phpmd && \
    chmod +x /usr/bin/phpmd

# install nodejs npm bitrix/cli
RUN apt-get update && apt-get -y --no-install-recommends install nodejs npm && npm install -g @bitrix/cli

# install git ssh nano vim htop
RUN apt-get update && apt-get -y --no-install-recommends install git ssh nano vim htop

# needle for vscode-server
ENV SHELL /bin/bash

EXPOSE 9000
