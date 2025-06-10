FROM bitrixdock/php56-fpm:latest

LABEL org.opencontainers.image.source="https://github.com/bitrixdock/bitrixdock"

COPY ./php.ini /etc/php5/fpm/conf.d/90-php.ini
COPY ./php.ini /etc/php5/cli/conf.d/90-php.ini

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

# install nvm nodejs npm yarn bitrix/cli
ARG NODE_VERSION
ENV NVM_DIR=/root/.nvm
RUN cd /root && \
    curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.40.3/install.sh | bash && \
    bash -c "source $NVM_DIR/nvm.sh && nvm install ${NODE_VERSION} && nvm alias default $NODE_VERSION && \
    nvm use default"
ENV NODE_PATH $NVM_DIR/v$NODE_VERSION/lib/node_modules
ENV PATH $NVM_DIR/versions/node/v$NODE_VERSION/bin:$PATH
RUN npm install --global yarn
RUN yarn global add @bitrix/cli

# install git ssh nano vim htop
RUN apt-get update && apt-get -y --no-install-recommends install git ssh nano vim htop

# needle for vscode-server
ENV SHELL /bin/bash

EXPOSE 9000
