ARG IMAGE_NAME="rollupdev/php"
ARG PHP_VERSION
FROM ${IMAGE_NAME}:${PHP_VERSION}-fpm

ARG TARGETARCH
# Install and enable SourceGuardian and ionCube loaders

# Install and enable SourceGuardian and ionCube loaders
RUN if [[ ${TARGETARCH} = "arm64" ]] && (( ${PHP_VERSION} > 73 )); then install-php-extensions sourceguardian ioncube_loader; fi
RUN if [[ ${TARGETARCH} = "amd64" ]]; then install-php-extensions sourceguardian ioncube_loader; fi
