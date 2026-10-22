FROM ghcr.io/modulairy/php:8.5.12-apache-bookworm@sha256:16fb4d2c17a22b60bba89a0b4ce923b12c2496d98b37c5abce517d1f1365f930
RUN set -eux; \
    apt-get update; \
    apt-get install -y --no-install-recommends git cron \
        libfreetype6-dev libjpeg62-turbo-dev libwebp-dev libpng-dev \
        libzip-dev libicu-dev libgmp-dev; \
    docker-php-ext-configure gd --with-freetype --with-jpeg --with-webp; \
    docker-php-ext-install -j"$(nproc)" gd zip mysqli pdo_mysql intl gmp; \
    a2enmod rewrite; \
    rm -rf /var/lib/apt/lists/*
ENV APACHE_DOCUMENT_ROOT=/var/www/html/public
RUN set -eux; \
    sed -ri 's!DocumentRoot /var/www/html!DocumentRoot ${APACHE_DOCUMENT_ROOT}!g' /etc/apache2/sites-available/000-default.conf; \
    printf '%s\n' '<Directory /var/www/html/public>' '    Options FollowSymLinks' '    AllowOverride All' '    Require all granted' '</Directory>' > /etc/apache2/conf-available/opencart.conf; \
    a2enconf opencart; \
    printf '%s\n' 'error_reporting=E_ALL' 'display_errors=Off' 'log_errors=On' > /usr/local/etc/php/conf.d/production-errors.ini
WORKDIR /var/www/html
