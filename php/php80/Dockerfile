FROM php:8.0-fpm

LABEL org.opencontainers.image.source="https://github.com/bitrixdock/bitrixdock"

# Используется официальный образ php:8.0-fpm вместо phpdockerio/php:8.0-fpm
# т.к. пакеты для PHP 8.0 больше не доступны в ondrej/php PPA после достижения EOL.
# Расширения устанавливаются через компиляцию из исходников.

RUN apt-get update \
    && apt-get install -y --no-install-recommends \
    libfreetype6-dev \
    libjpeg62-turbo-dev \
    libpng-dev \
    libwebp-dev \
    libzip-dev \
    libmemcached-dev \
    libxml2-dev \
    libicu-dev \
    libmagickwand-dev \
    zlib1g-dev \
    && docker-php-ext-configure gd --with-freetype --with-jpeg --with-webp \
    && docker-php-ext-install -j$(nproc) \
    gd \
    intl \
    mysqli \
    pdo_mysql \
    soap \
    zip \
    opcache \
    && curl -fSL -o /tmp/memcache.tgz https://pecl.php.net/get/memcache-8.2.tgz \
    && curl -fSL -o /tmp/memcached.tgz https://pecl.php.net/get/memcached-3.4.0.tgz \
    && curl -fSL -o /tmp/imagick.tgz https://pecl.php.net/get/imagick-3.8.1.tgz \
    && curl -fSL -o /tmp/xdebug.tgz https://pecl.php.net/get/xdebug-3.5.0.tgz \
    && pecl install /tmp/memcache.tgz \
    && pecl install /tmp/memcached.tgz \
    && pecl install /tmp/imagick.tgz \
    && pecl install /tmp/xdebug.tgz \
    && rm -f /tmp/memcache.tgz /tmp/memcached.tgz /tmp/imagick.tgz /tmp/xdebug.tgz \
    && docker-php-ext-enable memcache memcached imagick xdebug \
    && apt-get install -y --no-install-recommends \
    libfreetype6 \
    libjpeg62-turbo \
    libpng16-16 \
    libwebp6 \
    libzip4 \
    libmemcached11 \
    libicu67 \
    libmagickwand-6.q16-6 \
    && apt-get purge -y --auto-remove -o APT::AutoRemove::RecommendsImportant=false \
    libfreetype6-dev \
    libjpeg62-turbo-dev \
    libpng-dev \
    libwebp-dev \
    libzip-dev \
    libxml2-dev \
    libicu-dev \
    libmagickwand-dev \
    zlib1g-dev \
    && apt-get clean && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/* /usr/share/doc/*

COPY --from=composer:2 /usr/bin/composer /usr/bin/composer

RUN curl -LkSso /usr/bin/mhsendmail \
    'https://github.com/mailhog/mhsendmail/releases/download/v0.2.0/mhsendmail_linux_amd64' && \
    chmod 0755 /usr/bin/mhsendmail

COPY ./php.ini /usr/local/etc/php/conf.d/90-php.ini

# Создаем симлинки для обратной совместимости с phpdockerio/php:8.0-fpm
# чтобы поддержать существующие volume mounts
RUN mkdir -p /etc/php/8.0/fpm /etc/php/8.0/cli \
    && ln -s /usr/local/etc/php/conf.d /etc/php/8.0/fpm/conf.d \
    && ln -s /usr/local/etc/php/conf.d /etc/php/8.0/cli/conf.d \
    && ln -s /usr/local/etc/php-fpm.d /etc/php/8.0/fpm/pool.d

RUN usermod -u 1000 www-data

RUN echo "www-data" > /etc/cron.deny && chmod 644 /etc/cron.deny

WORKDIR "/var/www/bitrix"

EXPOSE 9000
