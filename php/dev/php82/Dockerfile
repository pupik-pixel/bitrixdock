FROM phpdockerio/php:8.2-fpm

LABEL org.opencontainers.image.source="https://github.com/bitrixdock/bitrixdock"

RUN apt-get update \
    && apt-get -y --no-install-recommends install \
    php8.2-gd \
    php8.2-imagick \
    php8.2-intl \
    php8.2-interbase \
    php8.2-mbstring \
    php8.2-mcrypt \
    php8.2-memcache \
    php8.2-memcached \
    php8.2-mysql \
    php8.2-opcache \
    php8.2-soap \
    php8.2-zip \
    && apt-get clean && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/* /usr/share/doc/*

COPY ./php.ini /etc/php/8.2/fpm/conf.d/90-php.ini
COPY ./php.ini /etc/php/8.2/cli/conf.d/90-php.ini

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
