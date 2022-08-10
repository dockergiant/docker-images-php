ARG PHP_VERSION
FROM php:${PHP_VERSION}

# Install all updates
RUN set -eux; \
    apt-get update -y; \
    apt-get upgrade -y; \
    apt-get install -y --no-install-recommends git perl npm patch gettext unzip bzip2 pwgen nmap ncat jq \
    libmcrypt-dev librabbitmq-dev imagemagick libmagickwand-dev libmagickcore-dev libgmp-dev libzip-dev; \
    pecl install mcrypt msgpack amqp redis imagick;

RUN set -eux \
    && PHP_VERSION=$(echo ${PHP_VERSION} | awk -F '.' '{print $1$2}') \
    && /bin/bash -c "if ((${PHP_VERSION} >= 80)); then pecl install eio-beta xmlrpc; else pecl install eio && docker-php-ext-install xmlrpc; fi" \
    && /bin/bash -c "if ((${PHP_VERSION} >= 80)); then pecl install eio-beta xmlrpc; else pecl install eio ; fi"

# Install PHP extensions
RUN docker-php-ext-install bcmath gd gmp intl opcache zip soap
RUN docker-php-ext-enable mcrypt msgpack amqp redis imagick eio xmlrpc

RUN rm -rf /var/lib/apt/lists/*
# Set primary INI file and PHP memory limit
RUN set -eux \
    && cp ${PHP_INI_DIR}/php.ini-development ${PHP_INI_DIR}/php.ini \
    && perl -pi -e 's/memory_limit(.*)/memory_limit=-1/g' ${PHP_INI_DIR}/php.ini

# Setup Composer 1 & 2
ENV COMPOSER_ALLOW_SUPERUSER 1
ENV COMPOSER_HOME /tmp/composer
COPY --from=composer:1 /usr/bin/composer /usr/bin/composer
COPY --from=composer:1 /usr/bin/composer /usr/bin/composer1
COPY --from=composer:2 /usr/bin/composer /usr/bin/composer2
