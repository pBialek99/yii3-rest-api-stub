FROM php:8.5-fpm-alpine

# INSTALL COMPOSER
COPY --from=composer:2 /usr/bin/composer /usr/bin/composer

# INSTALL DEPENDENCIES
RUN apk add --no-cache \
        libzip-dev \
        postgresql-dev \
        icu-dev \
# INSTALL EXTENSIONS
    && docker-php-ext-install \
        pdo_pgsql \
        intl \
        zip \
# INSTALL BUILD DEPENDENCIES FOR XDEBUG
    && apk add --no-cache --virtual .xdebug-build-deps $PHPIZE_DEPS linux-headers \
# INSTALL XDEBUG
    && pecl install xdebug \
    && docker-php-ext-enable xdebug \
# REMOVE BUILD DEPENDENCIES
    && apk del .xdebug-build-deps

# SET WORKING DIRECTORY
WORKDIR /var/www/html

# COPY XDEBUG CONFIGURATION
COPY ./docker/php/conf.d/xdebug.ini /usr/local/etc/php/conf.d/xdebug.ini