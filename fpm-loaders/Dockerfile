ARG IMAGE_NAME="rollupdev/php"
ARG PHP_VERSION
FROM ${IMAGE_NAME}:${PHP_VERSION}-fpm

# Install and enable SourceGuardian and ionCube loaders
RUN install-php-extensions sourceguardian ioncube_loader
