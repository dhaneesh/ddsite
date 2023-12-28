FROM drupal:10.1-php8.1-fpm-alpine3.18

RUN set -eux; \
    \
    apk add --no-cache git gmp mysql-client aws-cli bash

COPY web/ /opt/drupal/web/

COPY sites/default/settings.php /opt/drupal/web/sites/default/

COPY docker-php-entrypoint /usr/local/bin/

COPY composer.* /opt/drupal/

COPY config /opt/drupal/config/

COPY blt /opt/drupal/blt/

COPY patches /opt/drupal/patches/

COPY scripts /opt/drupal/scripts/

COPY php.ini /usr/local/etc/php

RUN apk add patch;

RUN set -eux; \
    \
    apk add --no-cache --virtual .my-build-deps \
        coreutils \
        gmp-dev \
    ; \
    \
    docker-php-ext-configure gmp; \
    \
    docker-php-ext-install -j "$(nproc)" gmp; \
    \
    apk del --no-network .my-build-deps; \
    # composer update; \
    composer install --ignore-platform-req=ext-intl

RUN chmod ugo+rx /opt/drupal/cron.sh; \
    chown -R www-data:www-data /opt/drupal/config; \
    mkdir /opt/drupal/web/sites/default/files; \
    chown -R www-data:www-data /opt/drupal/web/sites/*; \ 
    mkdir -p /opt/drupal/config/default; \
    chown -R www-data:www-data /opt/drupal/config

VOLUME ["/opt/drupal"]
