ARG IMAGE_NAME="rollupdev/php"
ARG PHP_VERSION
FROM ${IMAGE_NAME}:${PHP_VERSION}

ARG TARGETARCH

# Install and enable SourceGuardian and ionCube loaders
RUN set -eux \
    && PHP_VERSION=$(echo ${PHP_VERSION} | awk -F '.' '{print $1$2}') \
    && /bin/bash -c "if [[ '${TARGETARCH}' == 'arm64' ]] && (( ${PHP_VERSION} == 74 )); then install-php-extensions sourceguardian ioncube_loader; fi" \
    && /bin/bash -c "if [[ '${TARGETARCH}' == 'amd64' ]] && (( ${PHP_VERSION} <= 74 )); then install-php-extensions sourceguardian ioncube_loader; fi"
