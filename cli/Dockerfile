ARG PHP_VERSION
FROM php:${PHP_VERSION}

# Install all updates
RUN set -eux; \
    apt-get update -y; \
    apt-get upgrade -y; \
    rm -rf /var/lib/apt/lists/*

# Install PHP extension installer
ADD https://github.com/mlocati/docker-php-extension-installer/releases/latest/download/install-php-extensions /usr/local/bin/

# List of PHP extensions
ARG PHP_EXTENSIONS="bcmath gd gmp intl mcrypt opcache msgpack amqp redis imagick zip eio soap xmlrpc pcntl exif"

# Install PHP extensions and other utilities
RUN set -eux; \
    chmod +x /usr/local/bin/install-php-extensions; sync; \
	install-php-extensions ${PHP_EXTENSIONS}; \
	apt-get update; \
    apt-get install -y --no-install-recommends git perl npm patch gettext unzip bzip2 pwgen ncat jq; \
    rm -rf /var/lib/apt/lists/*

# Set primary INI file and PHP memory limit
RUN set -eux \
    && cp ${PHP_INI_DIR}/php.ini-development ${PHP_INI_DIR}/php-cli.ini \
    && perl -pi -e 's/memory_limit(.*)/memory_limit=-1/g' ${PHP_INI_DIR}/php-cli.ini

RUN set -eux; \
	{ \
		echo 'if [ -d /etc/profile.d ]; then'; \
        echo '  for i in /etc/profile.d/*.sh; do'; \
        echo '    if [ -r $i ]; then'; \
        echo '      . $i'; \
        echo '    fi'; \
        echo '  done'; \
        echo '  unset i'; \
        echo 'fi'; \
	} >> /etc/bash.bashrc;

# Setup Composer 1 & 2
ENV COMPOSER_ALLOW_SUPERUSER 1
ENV COMPOSER_HOME /tmp/composer
COPY --from=composer:1 /usr/bin/composer /usr/bin/composer
COPY --from=composer:1 /usr/bin/composer /usr/bin/composer1
COPY --from=composer:2 /usr/bin/composer /usr/bin/composer2
